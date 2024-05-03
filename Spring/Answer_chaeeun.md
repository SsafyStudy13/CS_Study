# Spring

## Spring 핵심 개념

- **POJO(Plain Old Java Object, 순수 객체)**

  : 특정 프레임워크나 환경에 종속되지 않고 일반적인 자바 객체를 의미

  : 스프링에서는 POJO를 활용하여 애플리케이션의 비즈니스 로직을 간결하고 유연하게 구현 가능

  : POJO는 특정 인터페이스를 구현하거나 특정 클래스를 상속받지 않아도 되며, 테스트하기 쉽고 재사용성이 높은 코드 작성에 용이

- **AOP(Aspect-Oriented Programming, 관점 지향 프로그래밍)**

  : 횡단 관심사(cross-cutting concern)를 모듈화하여 코드의 중복을 줄이고 관리하기 쉽도록 도와줌

  : 주요 기능을 핵심 로직과 부가적인 로직으로 분리한 개발에 용이

  ​	→ 로깅, 트랜잭션 관리, 보안 등과 같은 부가적인 기능을 핵심 비즈니스 로직과 분리하여 관리 가능

- **PSA(Portable Service Abstraction, 이식 가능 서비스 추상화)**

  : 스프링에서는 다양한 서비스 인터페이스를 추상화하여 개발자가 특정 기술에 종속되지 않고 서비스를 사용할 수 있도록 함

  : 개발자는 서비스 인터페이스를 변경하지 않고도 다양한 기술을 쉽게 전환하고 사용 가능

- **DI(Dependency Injection, 의존성 주입)**

  : 객체 간의 의존 관계를 외부에서 주입하는 디자인 패턴

  : 스프링에서는 DI를 통해 객체 간의 결합도를 낮추고 유연한 코드 작성 가능

  : DI는 생성자 주입, 세터 주입, 필드 주입 등의 방식으로 의존성을 주입 가능

  : XML 설정이나 어노테이션을 통해 주입할 객체 지정 가능

  : DI를 사용하면 테스트 용이성 향상, 객체 간의 결합도가 낮아져 유지보수성 향상

</br>

## Spring Framework

: 자바 기반의 엔터프라이즈 애플리케이션 개발을 보다 쉽고 효율적으로 만들기 위한 풍부한 기능과 기능성 제공

: 대규모 데이터 처리, 트랜잭션이 동시, 대규모로 발생하는 환경에서의 개발 지원

→ 의존성 주입(DI), 관점 지향 프로그래밍(AOP), 트랜잭션 관리, 보안, 데이터 액세스, 웹 개발 등의 목적으로 사용

: 자바 객체를 담고 직접 관리, 객체의 생성, 소멸, 라이프 사이클을 관리

→ IOC 기반의 프레임워크

> ### IOC (Inversion of Control)
>
> 소프트웨어의 제어 흐름이 개발자가 아닌 프레임워크 또는 컨테이너에 의해 관리되는 디자인 패턴
>
> → 느슨한 결합 : 객체 간 결합도를 낮추고 객체 간 의존성을 외부에서 주입하여 유연하고 재사용 가능한 코드 작성에 도움
>
> → 모듈화 : 애플리케이션을 모듈 단위로 나누어, 모듈 간의 의존성을 외부에서 주입함으로써 유지보수성 향상
>
> → 테스트 용이성 : 모의 객체를 주입하여 테스트함으로써 단위 테스트를 쉽게 작성하고 실행할 수 있도록 도움

</br>

## Bean

: Spring IOC 컨테이너가 관리하는 객체

: 빈으로 등록된 객체의 기본 스코프는 싱글톤으로 정해짐

​	싱글톤 : 객체가 하나만 만들어지므로 재사용 가능, 메모리 절약 가능

​				런타임 시 성능 최적화에 유리함

​				↔ 프로토타입 : 매번 다른 객체가 만들어짐

→ 의존성 관리가 용이함

### 등록 방법

1. **XML 기반 설정 방식** - `<Bean>` 태그 이용

   : XML 파일에 `<bean>` 태그를 사용하여 빈을 정의

   : 각 빈의 클래스 이름과 식별자를 지정하고, 필요에 따라 프로퍼티나 생성자 인자 설정 가능

   ```xml
   <bean id="userService" class="com.example.UserService">
       <property name="userRepository" ref="userRepository"/>
   </bean>
   ```

   

2. **XML 기반 설정 방식** - `componentScan` 사용

   : XML 파일에 `componentScan`을 사용하여 base-package에서부터 `Component`를 상속받은 `@Repository`, `@Service` 등의 어노테이션을 찾아 빈으로 등록

   : Spring 2.5부터 가능한 방식

   ```xml
   <beans>
       <context:component-scan base-package="com.example"/>
   </beans>
   ```

   ```java
   @Repository
   public class UserRepository {
   }
   ```

   ```java
   @Service
   public class UserService {
   
       @Autowired
       UserRepository userRepository;
   
       public void setUserRepository(UserRepository userRepository) {
           this.userRepository = userRepository;
       }
   }
   ```

   

3. **Java 기반 설정 방식**

   : 설정 클래스 Config.java 파일을 생성한 뒤 `@Configuration` 을 클래스 선언부에 추가

   : 내부에 속한 메서드에 `@Bean` 어노테이션을 붙여 빈 등록

   ```java
   @Configuration
   public class ApplicationConfig {
       @Bean
       public UserRepository userRepository() {
           return new UserRepository();
       }
   }
   ```

</br>

### Scope

: 빈이 존재할 수 있는 범위

: 빈의 생명 주기와 관련

- 지원 스코프

  `싱글톤(Singleton)` : 기본 스코프, 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프

  `프로토타입(Prototype)` : 스프링 컨테이너는 빈의 생성과 의존 관계  주입까지만 관여, 매우 짧은 범위의 스코프, 종료 메서드는 호출되지 않음

  - 웹 스코프

    `요청(Request)` : 웹 요청이 들어오고 나갈 떄까지 유지되는 스코프 

    `세션(Session)` : 웹 세션이 생성되고 종료될 때까지 유지되는 스코프 

    `애플리케이션(Application)` : 웹의 서블릿 컨텍스트와 같은 범위로 유지되는 스코프

</br>

## DI (Dependency Injection)

: 스프링 컨테이너에서 객체 빈을 먼저 생성한 뒤, 생성한 객체를 지정한 객체에 주입하는 디자인 패턴

→ 객체 간의 결합도를 낮추고 유연한 코드를 작성할 수 있도록 도와줌

→ 스프링에서는 DI를 사용하여 빈을 생성하고 의존성을 주입하므로 확장성이 뛰어난 코드 작성 가능

- 의존성 주입 방식 → `@Autowired` 어노테이션 사용

  1. **필드 주입 (Field Injection)**

     : 선언된 필드에 생성된 객체를 주입해주는 방식

  2. **수정자 주입 (Setter Based Injection)**

  3. **생성자 주입 (Constructor Based Injection)** → 권장 방식

     : 인스턴스가 생성될 때 1회 호출되는 것이 보장 → 불변 객체 보장

     : 필드에 `final` 키워드 사용 가능

     : 클래스 내 생성자가 1개이고 주입받을 객체가 빈으로 등록됨

     : `@RequiredArgsConstructor` 어노테이션으로 필드를 포함한 생성자를 포함시켜주고, @Autowired 어노테이션을 생략하므로 가독성이 좋은 코드로 사용 가능

</br>

## Spring Filter와 Interceptor

: 중복된 코드를 제거할 수 있는 기능

### Filter

: 서블릿 필터로서, HTTP 요청 및 응답을 변경하거나 조작할 수 있는 기능을 제공

: 로깅, 인코딩, 보안 등의 작업 수행

: 디스패처 서블릿은 스프링의 가장 앞 단에 존재하므로 스프링 범위 밖인 웹 컨테이너에서 처리

: Filter 인터페이스 구현 메서드

 	1. init : 필터 객체를 초기화하고 서비스에 추가하기 위한 메서드
 	1. doFilter : HTTP 요청이 디스패처 서블릿으로 전달되기 전에 웹 컨테이너에서 실행되는 메서드
 	1. destroy : 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메서드

### Interceptor

: 스프링 MVC에서 컨트롤러 호출 전후에 특정 작업을 수행할 수 있는 인터셉터

: 로깅, 권한 체크, 세션 관리 등의 작업 수행

: 스프링 컨테이너에서 처리

: Interceptor 인터페이스 구현 메서드

 	1. preHandle : 컨트롤러가 호출되기 전에 실행되는 메서드
 	2. postHandle : 컨트롤러를 호출된 후에 실행되는 메서드
 	3. afterCompletion : 모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 완료된 후에 실행되는 메서드

![image](https://github.com/SsafyStudy13/CS_Study/assets/122426072/fb56bffd-116b-4641-af75-341e7383eaed)

|                                      | Filter                                                       | Interceptor                                                  |
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 관리되는 컨테이너                    | 서블릿 컨테이너                                              | 스프링 컨테이너                                              |
| 스프링의 예외처리 여부               | X                                                            | O                                                            |
| Request/Response 객체 조작 가능 여부 | O                                                            | X                                                            |
| 용도                                 | - 공통된 보안, 인증, 인가 관련 작업<br /> - 모든 요청에 대한 로깅, 감사<br />- 이미지/데이터 압축 및 문자열 인코딩<br />- spring과 분리되어야하는 기능 | - 세부적인 보안 및 인증, 인가 공통 작업<br />- API 호출에 대한 로깅 또는 감사<br />- Controller로 넘겨주는 데이터 가공 |

</br>

## PSA (Portable Service Abstraction)

: 스프링이 제공하는 다양한 **서비스 인터페이스를 추상화**한 개념

: 특정 클래스가 추상화된 상위 클래스를 일관되게 바라보며 하위 클래스 기능을 사용하는 것

: PSA 적용 시, 기존 코드를 수정하지 않으면서 확장 가능

: 일관성 있는 방식으로 서비스 기능에 접근할 수 있고, **기술을 유연하게 사용**할 수 있음

ex) JdbcTemplate은 JDBC에 대한 추상화 제공

ex) RestTemplate은 HTTP 클라이언트에 대한 추상화 제공

</br>

## Spring MVC 패턴

: Model, View, Controller로 구분하여 코드를 작성하는 개발 패턴

: UI 영역과 비지니스 로직으로 구분되어 서로 영향을 주지 않으면서 개발과 유지보수 가능

- Model : 클라이언트에게 응답으로 돌려주는 작업 처리 결과 데이터
- View : Model 을 이용하여 화면에 보이는 리소스를 제공하는 역할
- Controller : Model과 View 중간에서 요청을 받고 처리해주는 역할

![image](https://github.com/SsafyStudy13/CS_Study/assets/122426072/87538257-1d20-4771-ab66-2bae428e3466)

DispatcherServlet이 클라이언트의 요청을 받음

→ Handler Mapping을 통해 요청을 처리할 컨트롤러를 찾음

→ 해당 컨트롤러를 실행하여 비즈니스 로직을 처리

→ View Resolver를 통해 결과 화면 생성

</br>

## DispatcherServlet

: Spring MVC의 핵심 서블릿

- 수행 역할
  - 클라이언트의 요청을 받아 적절한 컨트롤러에게 요청 전달
  - ViewResolver를 통해 뷰를 렌더링하여 응답 생성

: **Front Controller**라고도 불림

: `HttpServlet` 을 상속받아 사용

> ### servlet, 서블릿
>
> : Java EE 웹 애플리케이션 개발을 위한 스펙으로 **웹 애플리케이션 서버에서 동작하는 Java 클래스**
>
> : 클라이언트의 요청을 처리하고 동적 웹 콘텐츠 생성에 사용됨

</br>

## DTO (Data Transfer Object)

: 계층 간 데이터 교환을 위해 사용되는 객체

: 주로 **서비스 계층과 뷰 계층 간의 데이터 전달**에 사용

→ 뷰에서 필요한 데이터를 한 번에 전달하여 효율적인 통신 가능

→ 비즈니스 로직과 뷰의 분리를 유지하여 유지보수성 향상

→ 각 로직에 맞는 필드만 생성함으로써 DTO를 통해 어떤 값들이 매핑되는지 쉽게 파악 가능

- Entity 대신 DTO를 사용하는 이유?

  : Entity 클래스는 데이터베이스와 맞닿아 있기 때문에, 사소한 기능 변경을 위해 Entity를 변경하는 것은 너무 큰 변경이기 때문

  : Controller에서 결과 값으로 여러 테이블을 조인하는 경우가 빈번하기 때문에 Entity 만으로 표현하기 어려운 경우가 많기 때

  : Controller와 Service 사이에서 강한 의존을 방지하기 위함

</br>

## HTTP API와 REST API

### HTTP API

: HTTP 프로토콜을 통해 데이터를 주고받는 인터페이스를 제공하는 API

: 주로 XML 또는 JSON 형식으로 데이터를 전송하며, SOAP 등의 프로토콜을 사용할 수도 있음

> ### HTTP(HyperText Transfer Protocol)
>
> : 서버-클라이언트 간 데이터를 주고 받기 위한 프로토콜
>
> : 인터넷에서 HyperText를 교환하기 위한 통신 귀약, 80번 포트 사용
>
> : Application 레벨의 프로토콜, TCP/IP 위에서 동작
>
> : Stateless 프로토콜
>
> : Method, Path, Version, Headers, Body 등으로 구성

> ### HTTPS(HyperText Transfer Protocol Secure)
>
> : HTTP에 데이터 암호화가 추가된 프로토콜
>
> : 443번 포트 사용
>
> : 대칭키 암호화, 비대칭키 암호화 방식 사용

> ### SOAP(Simple Object Access Protocol)
>
> : HTTP, HTTPS, SMTP를 통해 XML 기반의 데이터를 네트워크에서 교환하는 프로토콜

</br>

### REST API (Representational State Transfer API)

: REST 아키텍처 스타일을 따르는 API

​	→ REST : HTTP를 잘 활용하기 위한 원칙

: 자원을 표현하고 해당 자원에 대한 행위(조회, 생성, 수정, 삭제)를 HTTP 메서드(GET, POST, PUT, DELETE)를 통해 수행하는 인터페이스

- **HTTP API와의 차이**

  → REST API는 HTTP 프로토콜을 따르면서 아래 4가지 조건 충족 **(로이 필딩 논문 기반)**

  - **자원의 식별**

    : 모든 리소스는 고유한 식별자(URI, Uniform Resource Identifier)를 가짐

    ​	→ URL(Uniform Resource Locator)

    ​		: 웹 주소, 네트워크 상에서 리소스가 어디 있는지 알려줌

    ​		: URI의 하위개념

  - **메시지를 통한 리소스 조작**

    : 클라이언트가 리소스를 조작하기 위해 필요한 모든 정보는 요청 메시지에 포함되어야함

    : 요청 메시지는 리소스의 상태를 나타내고, 서버는 이를 해석하여 적절한 동작 수행

  - **자기서술적 메시지(self-descriptive)**

    : 요청과 응답은 자기서술적이어야 함

    : 메시지 자체에 필요한 모든 정보가 포함되어야하고, 이를 통해 클라이언트와 서버는 상호 작용하는 동안 필요한 모든 정보 추출 가능

  - **애플리케이션의 상태에 대한 엔진으로서 하이퍼 미디어(HATEOAS)**

    : 클라이언트와 서버 간의 통신은 하이퍼미디어를 통해 이루어져야함

    : 서버는 클라이언트에게 다음 동작에 대한 링크를 포함하여 응답함으로써 클라이언트가 상호작용을 지속적으로 수행할 수 있도록 함

</br>

## Annotation

: 자바 소스 코드에 사용하여 특정 의미를 가지고 기능을 수행하도록 하는 방법

: 코드에 정보를 추가하는 정형화된 방법

→ 코드의 가독성과 재사용성을 높임

- 종류

  `@ComponentScan`

  : @Component, @Service, @Repository, @Controller, @Configuration이 붙은 빈들을 찾아서 Context에 빈을 등록해주는 어노테이션

  `@Component`

  : 해당 클래스를 스프링 빈으로 등록하는 데 사용

  : 주로 일반적인 컴포넌트나 라이브러리 클래스에 사용

  `@Service`

  : 비즈니스 로직을 담당하는 클래스에 사용

  : 주로 서비스 계층의 구현체에 사용되며, 트랜잭션 처리, 예외 처리 등의 비즈니스 로직 수행

  `@Repository`

  : 데이터 액세스 계층의 구현체에 사용

  : 주로 데이터베이스와의 상호 작용이 필요한 DAO(Data Access Object) 클래스에 사용

  `@Controller`

  : 스프링 MVC에서 컨트롤러를 정의할 때 사용

  : HTTP 요청을 처리하고, 적절한 응답을 반환하는 역할

  `@Autowired`

  : 의존성 주입을 수행

  : 주로 생성자, 필드, 세터 등의 방식으로 의존성을 주입할 때 사용

  `@RequestMapping`

  : HTTP 요청과 특정 메서드 또는 컨트롤러 메서드를 매핑하여 해당 요청에 대한 처리를 지정

  `@GetMapping`

  : HTTP GET 요청을 처리하는 메서드를 지정하는 데 사용

  `@PostMapping`

  : HTTP POST 요청을 처리하는 메서드를 지정하는 데 사용

</br>

## Singleton pattern

: 애플리케이션에서 인스턴스가 **단 하나만 생성되도록 보장**하는 디자인 패턴

: 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴

→ 자원을 절약하거나 중복 생성을 방지하기 위해 사용

→ 여러 컴포넌트 간 상태를 공유하기에 용이

→ **스프링 컨테이너가 싱클톤 빈의 생명주기를 관리**해주므로 개발에 용이

→ 스프링의 DI는 주로 싱글톤 스코프를 기반으로 동작하므로 DI와의 호환에 있어서 가장 효율적

</br>

### 싱글턴 패턴 만드는 방법

1. private으로 자기 자신 객체 만들기

   : static 사용 이유? 객체를 생성하지 않고 클래스 이름으로 접근하기 위함

   ```java
   private static Person instance = new Person();
   ```

2. 생성자를 private으로 막기

   : 외부에서 새로운 객체를 만들지 못하게 하기 위함

   ```java
   private Person() {
       this.name = "유일한 사람";
       this.age = 20;
   }
   ```

3. 유일한 객체에 접근할 수 있는 통로 만들기

   : getter 사용

   ```java
   public static Person getInstance() {
   	return instance;
   }
   ```

</br>





