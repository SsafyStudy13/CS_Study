## 1. 스프링에서 빈을 등록하는 방법에 대해 설명해주세요. (유지연)
`Spring`에서 `빈`을 **등록**할 수 있는 방법은 크게 `애너테이션`을 사용하는 방법과 `XML config` 파일을 사용하는 방법 두 가지로 나뉩니다.  
`애너테이션`을 사용하는 경우, 빈 자동등록에 사용 가능한 애너테이션을 빈으로 사용할 클래스에 달아줍니다. 이러한 `스테레오타입 애너테이션`으로는 `@Component`와 이를 상속한 `@Repository`, `@Controller`, `@Service`가 있습니다.  
`스테레오타입 애너테이션`을 사용해 빈을 등록할 경우, 반드시 `component-scan`으로 어떤 패키지를 스캔할 지 설정해주어야 합니다. 이 역시 xml에서 `context:component-scan`을 등록하거나, `@Configuration` 애너테이션이 달린 설정 클래스에서 `@ComponentScan(basePackages="패키지명")` 애너테이션을 달아 설정할 수 있습니다.  
`XML config` 파일을 사용하는 경우, `XML` 파일의  `<beans>` 태그 내부에 `<bean>` 태그를 통해 등록합니다.   이후, `ClassPathXmlApplicationContext`로 xml 설정 파일을 기반으로 한 컨텍스트를 생성할 수 있습니다.  
추가로 bean에 값을 주입하기 위해서 `<bean>` 태그 내부에 `<constructor-arg>` 태그로 생성자를 통해 주입받을 값을 넘기거나, `<property>` 태그로 설정자를 통해 주입받을 값을 넘길 수 있습니다.  

## 2. DI에 대해 설명해주세요. (유지연)
`DI, 의존성 주입`이란, 어떤 객체나 함수가 필요로 하는 다른 객체나 함수를 내부에서 생성하는 것이 아닌, 외부에서 전달 받는 프로그래밍 기법을 말합니다.  
이를 통해 객체를 생성하고, 사용하는데 `관심사(concern)`를 분리하므로써 프로그램이 `느슨한 결합(loosely coupled)`을 갖도록 하는 것을 목표로 합니다.  
Spring에서 DI는 객체가 그들의 종속성을 정의하는 형태의 IoC라고 할 수 있습니다. 종류로는 `생성자 기반 DI`, `설정자 기반 DI`, `필드 기반 DI`가 있습니다. 이 중에서 주로 `생성자 기반 DI`가 권장되는데, NPE를 방지할 수 있고 final 선언이 가능하기 때문입니다.  
`생성자 기반 DI`는 설정하고자 하는 의존성들을 인수로 받는 생성자를 호출해 의존성을 설정합니다. IoC 컨테이너는 인수들의 타입, index와 같은 attribute를 토대로 의존성을 주입합니다.  
`설정자 기반 DI`는 인수가 없는 생성자/정적 팩토리 메서드를 통해 빈을 인스턴스화 한 후, 설정자를 호출해 의존성을 설정합니다.  
`필드 기반 DI`는 `@Autowired` 애너테이션으로 필드를 표시해 의존성을 주입하는 방식으로, 객체를 생성하는 동안 빈을 주입할 생성자, 설정자가 없는 경우 IoC 컨테이너가 Reflection을 이용해 주입하게 됩니다.   Reflection을 사용하기 때문에 더 많은 비용이 들고, 종속성을 추가하기 쉬워 단일 책임 원칙을 위배할 가능성이 높아 권장되지 않는 방식입니다.  

## 3. Spring Filter와 Interceptor에 대해 설명하고, 사용 예시를 설명해주세요. (유지연)
`Servlet Filter`는 `Jakarta EE`에 포함된 `Java Servlet`의 일부로, `Servlet Container(Web Container)`와 `Servlet` 사이에서 클라이언트로부터 서버로 들어오는 요청, 응답을 검사하고 특정 작업을 수행할 수 있는 인터페이스 입니다. 필터는 여러 개의 필터로 구성된 필터 체인을 이룰 수 있으며, 요청은 체인의 순서대로, 응답은 체인의 역순으로 필터를 거칩니다.  
`Filter`를 지정하면, `Filter`가 매핑된 모든 요청과 그에 대한 응답이 `doFilter()` 메서드를 거치게 됩니다. 따라서 해당 메서드 내에서 `Request`/`Response` 객체를 검사하거나 특정 작업을 수행한 뒤 Filter 체인의 다음 엔티티를 호출하거나, 요청에 대한 처리를 차단할 수 있습니다.  
`Filter`는 인증, 로깅 및 검사, 이미지/데이터 전처리 등에 사용되며, `Spring MVC` 외부에서 동작하기 때문에 `Spring MVC` 내부에서 분리하고자 하는 기능들을 주로 `Filter`로 처리하게 됩니다.  

`Interceptor`는 Spring에서 `DispatcherServlet`과 `Controller` 사이에서 요청, 응답을 검사하고 특정 작업을 수행할 수 있는 인터페이스로, Spring MVC 프레임워크의 일부입니다.  
`Interceptor`에는 상황 별로 호출되는 3개의 메서드가 존재합니다.  
`preHandle`은 `HandlerMapping`이 적절한 Handler 객체를 선택한 뒤, `HandlerAdapter`가 핸들러를 호출하기 이전에 호출되는 인터셉션 포인트입니다.  
`postHandle`은 대상 핸들러의 호출 이후 호출되며, `DispatcherServlet`에 의해 뷰가 렌더링 되기 전 호출됩니다. 즉, 핸들러가 성공적으로 실행된 뒤, 호출되는 인터셉션 포인트입니다. `preHandle`과 달리 실행 체인 역순으로 실행된다는 특징이 있습니다.  
`AfterCompletion`은 요청이 처리되고, 렌더링이 완료될 때 호출되는 콜백 함수입니다. `preHandle` 메서드가 성공적으로 완료된 경우에만 호출됩니다. `postHandle`과 마찬가지로 실행 체인의 역순으로 호출됩니다.  
`Interceptor`는 애플리케이션 로깅과 같은 횡단 관심사 문제를 처리하거나, 세부 권한 검사, Spring Context/Model 조작과 같은 작업에 주로 사용되며, `Handler(Controller)`와 `ModelAndView`에 접근할 수 있다는 점이 유용한 케이스에 대해 주로 사용됩니다.  

## 4. PSA에 대해 설명해주세요. (박규영)
Spring의 핵심 개념 중 하나인 `PSA(Portable Service Abstraction)`는 환경과 세부기술의 변경에 관계없이 일관된 방식으로 기술에 접근할 수 있도록 해주는 설계 원칙을 의미합니다. 즉, 개발자가 구현사항에 집중하기 보다 비즈니스 로직에 더욱 집중할 수 있도록 해줍니다.  
이러한 설계 원칙은 프레임워크에서 기술적인 세부 사항을 추상화해 제공하므로써 실현됩니다. 따라서 개발자는 추상화된 상위 클래스만 바라보며, 구체적인 구현사항은 연결된 하위 클래스에 따라 바꿀수 있게 됩니다.  
Spring에서 이러한 PSA가 적용된 예시로는 Spring MVC에서 `doGet()`, `doPost()`와 같은 메서드를 직접 구현하지 않고, `@GetMapping`, `@PostMapping`을 통해 매핑하는 것이 있습니다. 또한 중간에 DB를 JDBC를 통해 접근하거나, JPA를 통해 접근하든 상관없이 `@Transactional` 애너테이션이 동일하게 트랜잭션을 처리하는 것 역시 이러한 PSA의 예시 중 하나입니다.  
- [스프링 AOP와 PSA의 이해 (f-lab.kr)](https://f-lab.kr/insight/understanding-spring-aop-and-psa)
 - [[Spring] Spring의 핵심기술 PSA - 개념과 원리 (tistory.com)](https://sabarada.tistory.com/127)
## 5. 빈의 스코프에 대해 설명해주세요. (박규영)
Bean의 `scope`는 ApplicationContext에서 Bean의 생명주기와 가용성을 관리하는 방법을 나타냅니다. `빈 정의(bean definition)`에서 지정할 수 있으며, Spring의 경우 6개의 scope를 지원합니다.  
이 중 4개는 `WebApplicationContext`와 같은 web-aware `ApplicationContext`를 사용하는 경우에만 사용할 수 있습니다. 이 외에도 사용자 지정 scope도 만들 수 있습니다.  
`singleton`은 빈 정의의 범위를 각 IoC 컨테이너의 단일 객체 인스턴스로 지정합니다. 즉, 해당 빈을 단일 인스턴스로 생성해 관리하며, 빈에 대한 모든 요청은 캐시된 동일한 객체를 반환하게 됩니다. 따라서 해당 객체에 대한 수정은 객체에 대한 다른 모든 참조에 영향을 줍니다.  
`prototype`은 빈 정의의 범위를 원하는 수 만큼의 객체 인스턴스로 지정합니다. 즉, 컨테이너에서 요청할 때 마다 새로운 객체를 생성해 다른 인스턴스를 반환합니다. 다른 scope들과 달리, Spring은 `prototype` 빈의 전체 생명 주기를 관리하지 않습니다. 달리 말하자면, IoC 컨테이너는 `prototype` 객체를 인스턴스화 하고 의존성을 주입한 뒤, 클라이언트에게 넘겨주고 더 이상 관리하지 않습니다. 따라서 초기화 생명 주기 콜백인 `init()`은 잘 동작하지만, 파괴 생명 주기 콜백인 `destroy()`는 `prototype` 객체에서 호출되지 않습니다.  
`request`는 빈 정의의 범위를 단일 HTTP 요청 생명 주기로 지정합니다. 즉, 각 HTTP 요청마다 객체 인스턴스를 생성하며, HTTP 요청이 완료되면 빈은 삭제됩니다.  
`session`은 빈 정의의 범위를 HTTP 세션의 생명 주기로 지정합니다. 즉, 각 HTTP 세션마다 객체 인스턴스를 생성하며, HTTP 세션이 삭제되면 빈도 삭제됩니다.  
`application`은 빈 정의의 범위를 `ServletContext`의 생명 주기로 지정합니다. 즉, 동일한 서블릿 컨테이너(예를 들어 `Tomcat`)에 있는 모든 서블릿 간에 공유됨을 의미합니다.  
`websocket`은 빈 정의의 범위를 `WebSocket`의 생명 주기로 지정합니다. 즉 WebSocket 세션 동안 동일한 객체 인스턴스를 반환함을 의미합니다.  
## 6. Spring Framework를 왜 사용해야 할까요? 이유에 대해 설명해주세요. (박규영)

## 7. Spring MVC의 동작방식에 대해 설명해주세요. (김은솔)
클라이언트 요청에 따른 Spring MVC의 동작 흐름은 다음과 같습니다.  
첫 번째로 클라이언트의 요청을 단일 프론트 컨트롤러 서블릿인 DispatcherServlet이 수신합니다. 이후 DispatcherServlet은 요청 URL에 대응되는 컨트롤러를 HandlerMapping에게 질의합니다. 이후 HandlerMapping에게 받아온 컨트롤러로 요청을 전송하고, 컨트롤러가 요청을 처리한 결과를 ModelAndView 형태로 반환합니다.  
받아온 ModelAndView는 렌더링 될 실제 JSP 정보를 갖고있지 않기 때문에, ViewResolver에게 ModelAndView의 논리적 이름을 전달해, 알맞은 view를 불러옵니다. 이후 Model 값과 함께 view의 `render()` 메서드를 호출해 알맞은 JSP를 렌더링하게 됩니다.  

## 8. DispatcherServlet에 대해 설명해주세요. (김은솔)
`DispatcherServlet`이란, Spring의 단일 프론트 컨트롤러로, 서버로 들어오는 `HttpRequest`를 애플리케이션의 다른 컨트롤러로 위임하는 역할을 담당합니다. 정확히는 Handler, 컨트롤러 엔드포인트, 응답 객체를 지정하는 애너테이션과 함께, Spring 애플리케이션 내에 구현된 `HandlerAdapter` 인터페이스에 따라 요청을 처리합니다.  
다른 서블릿들과 마찬가지로, Java Configuration이나 `web.xml`에서 선언되고 매핑되어야 합니다. 이후 `DispatcherServlet`는 Spring Configuration을 사용해 RequestMapping, ViewResolver 등의 위임 컴포넌트들을 찾고 구성합니다.  
웹 애플리케이션은 원하는 수의 `DispatcherServlet`을 정의할 수 있으며, 각 서블릿은 자체 네임스페이스에 대해 동작하고 자체 애플리케이션 컨텍스트를 불러옵니다.  
- [DispatcherServlet :: Spring Framework](https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-servlet.html)

## 9. DTO를 사용한 이유를 말해주세요. (김은솔)
`DTO`란 데이터 전송 객체로, 주로 애플리케이션 하위 시스템이나, 비즈니스 로직, 프레젠테이션, 영속성 레이어 간에 데이터를 주고받는데 사용되는 디자인 패턴을 말합니다.  
DTO를 사용하게 된, DTO가 가진 장점은 다음과 같습니다.  
첫째, 계층 간 메서드 호출을 줄일 수 있습니다. 매개변수와 같은 데이터들을 단일 객체로 통합해 주고받을 수 있어 데이터 전달을 위한 호출 빈도가 줄어듭니다. 이러한 호출 수를 줄이는 것은 특히 원격 환경에서 데이터를 주고받아야 한다면 이를 통해 네트워크 사용과 오버헤드를 줄일 수 있습니다.  
둘째, 관심사 분리입니다. Spring에서는 일반적으로 컨트롤러-서비스 계층간, 또는 서비스-영속성 계층 간 전송되는 데이터를 캡슐화해, 관심사를 분리하고 코드의 모듈성을 향상시킬 수 있습니다. 만약 DB의 값을 갖는 엔티티를 그대로 사용한다면, 프레젠테이션에서 필요한 추가적인 데이터를 함께 전달할 수 없어 구현사항이 제한되거나, DB가 그에 맞게 변경되어야 할 것입니다. 반면 DTO를 사용한다면 동일한 도메인을 활용해 여러가지 프레젠테이션을 만들 수 있습니다.  
- [Differences Between Entities and DTOs | Baeldung](https://www.baeldung.com/java-entity-vs-dto)
- [The DTO Pattern (Data Transfer Object) | Baeldung](https://www.baeldung.com/java-dto-pattern)
- [[Spring Boot] DTO 는 왜, 언제 사용할까? (tistory.com)](https://e-una.tistory.com/72)
- [잊을만 하면 돌아오는 정산 신병들 | 우아한형제들 기술블로그 (woowahan.com)](https://techblog.woowahan.com/2711/)
## 10. HTTP API와 REST API에 대해서 설명해주세요. (송채은)
`HTTP API`는 인터넷 상에서 `HTTP`프로토콜에 따라 서로 다른 두 시스템에서 데이터를 주고받는 모든 API를 의미합니다.   
`REST API`는 `REST` 아키텍처 스타일을 따르는 웹 API를 말합니다.  
`REST` 아키텍처 스타일을 따르기 때문에, 다름과 같은 규약을 준수해야 합니다.  
먼저 요청에 기술된 자원을 식별할 수 있어야 합니다. REST API의 경우,웹이므로 URI를 통해 자원을 식별할 수 있어야 합니다. 여기서 말하는 `자원`은 실제로 클라이언트가 응답으로 받는 자료형을 얘기하는 것이 아니라, 서버 애플리케이션의 자원의 내부 표현과 다를 수 있습니다. 예를 들어 서버 내부에서는 텍스트로 다루지만, 클라이언트에게는 HTML 형식으로 전달될 수 있습니다.  
그리고 클라이언트는 원하는 자원을 수정/삭제하기에 충분한 정보를 리소스 표현에 담고 있어야 합니다. 즉, 자원을 지칭하는 메시지와 메타데이터만으로도 서버 상의 자원을 수정/삭제할 수 있어야 합니다.  
또한 각 메시지는 메시지 스스로를 어떻게 처리해야 하는지에 대한 정보도 포함해야 합니다. 즉 전달되는 메시지의 미디어 타입에 따라 어떤 파서를 써야하는지와 같은 정보도 포함해야 합니다.  
마지막으로 클라이언트가 작업을 완료하는데에 관련된 자원에 접근해야 한다면, 서버에서 더 많은 자원을 동적으로 검색할 수 있도록 표현에 추가적인 하이퍼링크를 넣어 전송해야 합니다.  
- [RESTful API란 무엇인가요? - RESTful API 설명 - AWS (amazon.com)](https://aws.amazon.com/ko/what-is/restful-api/)
- [REST - 위키백과, 우리 모두의 백과사전 (wikipedia.org)](https://ko.wikipedia.org/wiki/REST)
## 11. annotation 이란 무엇인지, 어떤 것들이 있는지 설명해주세요. (송채은)
`애너테이션`은 Java 프로그램에 추가적인 정보를 제공하는데 사용되는 메커니즘을 말합니다.  
종류로는 상위 클래스의 메서드를 재정의했음을 표시하는 `@Override`, 빈을 등록하는데 사용되는 `@Component`, `@Repository`, `@Controller`, `@Service`, 의존성을 주입하는데 사용되는 `@Autowired`, 빈을 구분할 수 있게 해주는 구분자 역할을 해주는 `@Qualifier`, DB 작업에 관련된 메서드를 트랜잭션이 되도록 해주는 `@Transactional` 애너테이션 등이 있습니다.  
## 12. singleton pattern 에 대해 설명해주세요. (송채은)
`싱글턴` 디자인 패턴은 한 클래스에 하나의 인스턴스만 존재하도록 허용하고, 인스턴스에 접근할 수 있는 전역 액세스 지점을 제공하는 디자인 패턴을 말합니다.  
한 인스턴스만 생성해 애플리케이션 전체에서 사용할 수 있어 메모리를 절약할 수 있고, 단일 액세스 지점을 제공하기 애플리케이션의 모든 컴포넌트들이 동일한 인스턴스를 사용함을 보장할 수 있습니다. 이러한 장점 때문에, DB 커넥션이나 Configuration과 같이 시스템 전역에 공유되어야 하는 자원이나, 로그 서비스처럼 자주 사용되는 자원에 적합한 패턴입니다.  
다만, 직접 클래스 안에서 객체를 생성하므로 SOLID 원칙 중 `DIP`를 위반하고, 다른 객체를 쓸 경우 코드가 변경되어야 하기 때문에 `OCP`도 위반할 수 있습니다.  
또한 싱글턴 패턴이 적용된 클래스의 싱글턴을 제거하려면 상당한 리팩토링이 수반되기 때문에 코드 유연성이 떨어진다는 단점이 존재합니다. 그리고 가장 큰 문제점으로 동시성 문제가 존재합니다. 전역 변수이기 때문에 누구나 접근할 수 있고, 불변 객체가 아닌 한 누구나 수정할 수 있습니다. 따라서, 싱글턴 클래스를 쓰는 한 곳에서 문제가 발생한다면, 어디에서 이 문제를 유발했는지 확인하기 위해 해당 클래스를 쓰는 모든 코드를 확인해봐야 합니다. 또한, 멀티스레드 환경에서는 각 스레드가 만든 변화에 따라 레이스 컨디션과 같은 동기화 문제가 발생할 수 있습니다.  