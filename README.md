## < 스프링 부트와 aws로 혼자 구현하는 웹 서비스 >

# CHAP 2

## 테스트 코드 작성 하는 이유
### 기존 개발방식
1. 코드 작성
2. 프로그램(Tomcat) 실행
3. Postman과 같은 API 테스트 도구로 HTTP 요청
4. 요청 결과 print문으로 검증
5. 결과가 다르면 다시 Tomcat을 중지하고 코드를 수정


#### 문제점
- 매번 코드를 수정할때마다 2-5 반복
    - Tomcat 껐다 켰다가 반복
- 눈으로 검증해야 하는 Print문
- 개발자가 만든 기능 보호

### WAS
- Web Application Server
- 언제 어디서나 같은 환경에서 스프링 부트를 배포

## 컨트롤러
### RestController
- 컨트롤러를 JSON을 반환하는 컨트롤러로 만들어 준다
- 예전에는 @ResponseBody를 각 메소드마다 선언했던 것을 한번에 사용할 수 있게 해준다고 생각하면 된다

### GetMapping
- HTTP Method인 Get의 요청을 받을 수 있는 API를 만들어 준다

## 테스트 코드
### RunWith(SpringRunner.class)
- 테스트를 진행할 때 JUnit에 내장된 실행자 외에 다른 실행자를 실행시킨다.
- 여기서는 SpringRunner라는 스프링 실행자를 사용한다.
- 즉, 스프링 부트 테스트와 JUnit 사이에 연결자 역할을 한다.

### WebMvcTest
- 여러 스프링 테스트 어노테이션 중, Web에 집중할 수 있는 어노테이션
- 선언하면 @Controller, @ControllerAdvice 등을 사용할 수 있다.
- 단, @Service, @Component, @Repository 등은 사용할 수 없다.
- 이 (HelloController)프로젝트에선 컨트롤러만 사용하기 때문에 선언

### Autowired
- 스프링이 관리하는 빈을 주입 받는다

### private MockMvc mvc
- 웹 API를 테스트할 때 사용한다.
- 스프링 MVC 테스트의 싲가점
- 클래스를 통해 HTTP GET, POST 등에 대한 API 테스트 가능

### mvc.perform("/hello"))
- MockMvc를 통해 /hello 주소로 HTTP GET 요청을 한다.
- 체이닝이 지원되어 아래와 같이 여러 검증 기능 선언 가능

### .andExpect(status().isOk())
- mvc.perform의 결과 검증
- HTTP header의 Status를 검증
- 200, 404, 500 등의 상태 검증

### .andExpect(content().string(hello))
- mvc.perform의 결과 검증
- Controller에서 "hello"를 리턴하기 때문에 값이 맞는지 검증

## 롬복
### Getter
- 선언된 모든 필드의 get 메소드를 생성해준다
### RequiredArgsConstructor
- 선언된 모든 final 필드가 포함된 생성자를 생성한다
- final이 없는 필드는 생성자 포함 x

## 롬복 테스트
### assertThat
- assertj라는 테스트 검증 라이브러리의 검증 메소드
- 검증하고 싶은 대상을 메소드 인자로 받는다
- 메소드 체이닝이 지원되어 isEqualTo와 같이 메스드를 이어서 사용

### isEqualTo
- assertj의 동등 비교 메소드
- assertThat에 있는 값과 isEqualTo의 값을 비교해서 같으면 성공

### Junit의 assertThat에 비해 assertj의 assertThat의 장점
- CoreMatchers와 달리 추가적으로 라이브러리가 필요하지 않다.
- 자동완성이 좀 더 확실하게 지원된다.

### RequestParam
- 외부에서 API로 넘긴 파라미터를 가져오는 어노테이션
- 여기서는 외부에서 name(@RequestParam("name"))이란 이름으로 넘긴 파라미터를 메소드 파라미터 name(String name)에 저장한다.

### param
- API 테스트할 때 사용될 요청 파라미터 설정
- 값은 String만 허용된다.
- 숫자/날짜 등의 데이터도 등록할때 문자열로 해야한다.

### jsonPath
- JSON 응답값을 필드별로 검증할 수 있는 메소드
- $를 기준으로 필드명을 명시
- name과 amount를 검증하니 $.name, $.amount로 검증한다.