블로그 링크 : https://no-delay.tistory.com/66

## Spring Bean이란?

Spring에서 Bean은 **Spring IoC 컨테이너에 의해 관리되는 Java Object**

또한, Spring에서는 등록되어 있는 Bean을 **싱글톤 객체**로 생성하여 관리

코드 중에 new 연산자로 객체를 생성한 경우 그 객체는 Spring IoC 컨테이너에 의해 관리되지 않기 때문에  
Bean이라고 할 수 없음

## Spring IoC 컨테이너란?

Spring IoC 컨테이너는 인스턴스의 생명주기를 관리하며, 생성된 인스턴스들에게 추가적인 기능을 제공

그 중 의존성 주입도 있음

단, 의존성 주입을 받으려면 전제 조건으로 빈이 되어야 함

그래서 Bean 어떻게 등록하는데!

## Bean 등록하는 방법

### 1\. 수동 등록

#### 방법

설정 클래스에 @Configuration 어노테이션을 붙이고, 메소드에 @Bean 어노테이션 붙임

#### 주의 사항

-   Bean 이름은 메서드 이름으로 생성 -> 중복 주의
-   @Configuration 안에서 @Bean을 사용해야 싱글톤 보장  
-   이유

@Configuration이 붙었을때의 결과를 보면 CGLIB라는게 붙은걸 볼수있다.  
사실 이것은 내가 만든 클래스가 아닌 스프링에서 CGLIB라는 바이트코드 조작 라이브러리를 사용해서 AppConfig를 상속받은 임의의 클래스를 만들고 그것을 스프링 빈으로 등록했기 때문에 이런결과가 나오게 된다.  
  
이때 임의의 클래스를 통해서 싱글톤이 되는것을 유지해준다.

[CGLIB 참고 링크](https://memodayoungee.tistory.com/151)

#### 언제 사용해야 될까?

-   애플리케이션에 광범위하게 영향을 주는 경우 (설정 정보를 명확하게 나타내야 유지보수하기 좋음)
-   다형성을 활용하여 여러 구현체를 등록해주어야 할 때 (어떤 빈들이 조회되어 주입되는지, 각 이름은 무엇인지 쉽게 파악 가능)

### 2\. 자동 등록

#### 방법

클래스에 @Component 어노테이션 붙임

@Component를 갖는 어노테이션 : @Controller, @Service, @Repository

#### 과정

Spring이 Component Scan을 통해 @Component 어노테이션이 있는 클래스를 찾아 자동으로 빈 등록

#### 언제 사용?

기본적으로 자동 빈 등록 방식 권장, 클래스에 있는 @Component만 보아도 빈으로 등록되었는지 확인이 가능하기 때문

---

## DI란?

의존성 주입(외부에서 두 객체 간의 관계를 결정해주는 디자인 패턴), 객체를 직접 생성하는 게 아니라 외부에서 생성한 후 주입하는 방식

### 과정

A 객체에서 B, C 객체를 의존할 때, A 객체에서 B, C 객체를 직접 생성하는 것이 아니라 외부(IoC 컨테이너)에서 생성된 객체를 주입

인터페이스를 사이에 둬서 클래스 레벨에서는 의존관계가 고정되지 않도록 하고 런타임 시에 관계를 동적으로 주입하여 유연성을 확보하고 결합도를 낮춤

객체 자체가 코드 상에서 객체 생성에 관여하지 않아도 되서 객체 사이의 의존도 낮출 수 있음

### DI(의존성 주입) 3가지 방법

#### 1\. 생성자 주입

```
@Controller
public class PetController{

    private final PetService petService;

	@Autowired
    public PetController(PetService petService){
    	this.petService = petService;
    }
}

/////////////////////////////////////////////////
// @Autowired 생략 & Lombok 사용 (생성자 생략 가능)

@Controller
@RequiredArgsConstructor
public class PetController{
    private final PetService petService;
}
```

클래스의 생성자를 통해 의존성 주입

생성자 주입은 인스턴스가 생성될 때 1회 호출 보장, 필드에 final 키워드 사용 가능

@Autowired (Spring 4.3부터 생략 가능)

Lombok 라이브러리의 @RequiredArgsConstructor (final 붙은 주입에만 가능)활용하여 생성자 생략 가능

final 키워드를 사용하지 않으면 @AllArgsConstructor 사용

Spring 공식 문서에서 권장, final 키워드 사용으로 객체의 불변성 보장

#### 2\. 필드 주입

```
@Controller
public class PetController{
	@Autowired
    private PetService petService;
}
```

클래스에 선언된 필드에 생성된 객체를 주입

**\[문제점\]**

-   final 키워드 사용 불가 -> 불변성 보장 X (생성자 이후에 호출되서 final 키워드 사용 X)
-   클래스 외부에서 접근이 불가능해서 테스트하기 어려움
-   단일 책임 원칙 위반
-   주입이 동시에 일어나 겹치는 경우, 순환참조 에러 발생

순환 의존이란 서로 다른 클래스가 서로를 참조하게 되는 상황이다. Class A가 Class B의 객체를 이용하고 Class B가 Class A의 객체를 이용하게 되면, 서로의 객체가 생성되지 않아 콜스택이 쌓이다 결국 오버플로우를 발생시킨다.  
이는 다른 의존 주입을 통해서도 발생할 수 있지만 Field Injection은 의존하고 있는 객체가 생성되지 않아도 의존받는 객체가 생성될 수 있어 컴파일 단계에서는 문제가 없다. 결국 런타임 단계까지 가서 순환 참조를 잡아낼 수 있기 때문에 매우 곤란해질 수 있다.

#### 3\. Setter 주입

```
@Controller
public class PetController{

    private PetService petService;

	@Autowired
    public void setPetService(PetService petService){
    	this.petService = petService;
    }
}
```

setter or 사용자 정의 메서드를 통해 의존 관계 주입

**\[문제점\]**

-   final 키워드 사용 불가 -> 불변성 보장 X (생성자 이후에 호출되서 final 키워드 사용 X)

---

## Spring Filter란?

javax.servlet.Filter는 Java Servlet API의 일부로, 요청과 응답을 거른뒤 정제하는 역할

### 특징

-   DispatcherServlet에 요청이 전달되기 전 / 후에 url 패턴에 맞는 모든 요청에 대해 부가 작업을 처리할 수 있는 기능 제공
-   Request, Response 조작 가능 (필터 체이닝에서 request, response를 넘겨줄 수 있어서)
-   FilterChain(필터 체인)을 통해 여러 필터가 연쇄적으로 동작 가능
-   웹 컨테이너(서블릿 컨테이너)에서 동작

### 언제 사용되나요?

주로 요청에 대한 인증, 권한 체크 등에 사용됨

들어온 요청이 DispatcherServlet에 전달되기 전에 헤더를 검사해 인증 토큰이 있는지 없는지, 올바른지 올바르지 않은지 등을 검사

#### 구현 방법

-   @Configuration + FilterRegistrationBean
-   @Component
-   @WebFilter + @ServletComponentScan
-   @WebFilter + @Component

### Filter 인터페이스 메서드

```
public interface Filter {

    public default void init(FilterConfig filterConfig) throws ServletException {}

    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException;

    public default void destroy() {}
}
```

-   init() : 필터 객체를 초기화하고 서비스에 추가하기 위한 메소드, 웹 컨테이너가 1회 init 메소드를 호출하여 필터 객체를 초기화하면 이후의 요청들은 doFilter를 통해 처리
-   doFilter() : Request, Response가 필터를 거칠 때 수행되는 메소드

url-pattern에 맞는 모든 HTTP 요청이 디스패처 서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행되는 메소드이다.

doFilter의 파라미터로는 FilterChain이 있는데, FilterChain의 doFilter 통해 다음 대상으로 요청을 전달하게 된다. chain.doFilter() 전/후에 우리가 필요한 처리 과정을 넣어줌으로써 원하는 처리를 진행할 수 있다.

-   destroy() : 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메소드, 웹 컨테이너에 의해 1번 호출

#### 사용 경험

다이어리 프로젝트에서 SpringSecurity와 Jwt를 이용한 인증을 구현할 때, Filter 사용

 [GitHub - HaruPalette/HaruPalette: AI 음성 다이어리 서비스

AI 음성 다이어리 서비스. Contribute to HaruPalette/HaruPalette development by creating an account on GitHub.

github.com](https://github.com/HaruPalette/HaruPalette)

## Interceptor란?

org.springframework.web.servlet의 HandlerInterceptor 인터페이스로, 스프링 MVC에서 웹 애플리케이션 내에 특정한 uri 호출을 가로채는 역할을 함

### 특징

-   Dispatcher Servlet이 Controller를 호출하기 전 / 후에 인터셉터가 끼어들어 요청과 dmd답을 참조하거나 가공할 수 있는 기능을 제공
-   Request, Response 조작 불가능
-   스프링 컨텍스트에서 동작

### 처리 과정

디스패처 서블릿이 핸들러 매핑을 통해 컨트롤러를 찾도록 요청하는데, 그 결과로 실행 체인(HandlerExecutionChain)을 돌려줌

여기서 1개 이상의 인터셉터가 등록되어 있다면 순차적으로 인터셉터들을 거쳐 컨트롤러가 실행되도록 하고,

인터셉터가 없다면 바로 컨트롤러를 실행

디스패처 서블릿이 여러 인터셉터 목록을 가지고 있고, for문으로 순차적으로 실행시킨다.

그리고 true를 반환하면 다음 인터셉터가 실행되거나 컨트롤러로 요청이 전달되며, false가 반환되면 요청이 중단된다.

\=> 그래서 다른 Request/Response 객체를 넘겨줄 수 없음

### 언제 사용되나요?

-   세부적인 보안 및 인증 / 인가 공통 작업
-   API 호출에 대한 로깅 또는 검사
-   Controller로 넘겨주는 정보 가공
-   JWT 토큰 정보를 파싱해서 컨트롤러에게 사용자의 정보를 제공하도록 가공 가능
-   여러 목적으로 API 호출에 대한 정보들을 기록해야 하는 상황에 HttpServletRequest나 HttpServletResponse를 제공해주는 인터셉터는 클라이언트의 IP나 요청 정보들을 기록하기에 용이

#### 예시 (세션을 통한 인증)

요청을 받아 들이기 전, 세션에서 로그인한 사용자가 있는지 확인해보고 없다면 로그인 페이지로 redirect 시킬 수 있습니다.  
Interceptor가 없다면 모든 컨트롤러마다 해당 로직을 넣어야 하니, 꽤나 번거롭고 비효율적입니다.  
혹은, 주기적으로 결과 페이지에 등장하는 데이터들을 인터셉터에서 응답을 가로채어 데이터를 추가한 다음 보낼 수도 있죠.  
메일 알림 개수를 조회한 후 추가한다거나, 헤더 데이터 같은 것들이 있을 수 있습니다.

### HandlerInterceptor 인터페이스 메소드

```
public interface HandlerInterceptor {
    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return true;
    }

    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
    }

    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
    }
}
```

-   preHandle() : Controller가 호출되기 전에 실행, 컨트롤러 이전에 처리해야 하는 전처리 작업이나 요청 정보를 가공하거나 추가하는 경우에 사용
-   postHandle() : Controller가 호출된 후에 실행 (View 렌더링 전), 컨트롤러 이후에 처리해야 하는 후처리 작업이 있을 때 사용 가능

컨트롤러가 반환하는 ModelAndView 타입의 정보가 제공되는데,

최근에는 JSON 형태로 데이터를 제공하는 RestAPI 기반의 컨트롤러(@RestController)를 만들면서 자주 사용되지 않음

-   afterCompletion() : 모든 뷰에서 최종 결과를 생성하는 일을 포함해 모든 작업이 완료된 후에 실행 (View 렌더링 후), 요청 처리 중에 사용한 리소스를 반환할 때 사용 가능

---

## PSA(Portable Service Abstraction)란?

![image](https://github.com/SsafyStudy13/CS_Study/assets/57094856/8e54aedb-6724-4ac2-af5d-a42762e51a97)

환경의 변화와 관계없이 일관된 방식의 기술로의 접근 환경을 제공하는 추상화 구조(하나의 추상화로 여러 서비스를 묶어둔 것)

특정 클래스가 추상화된 상위 클래스를 일관되게 바라보며 하위 클래스의 기능을 사용하는 것이 PSA의 기본 개념

PSA가 적용된 코드는 개발자의 기존에 작성된 코드를 수정하지 않으면서 확장할 수 있으며, 어느 특정 기술에 특화되어 있지 않음 -> 확장성 높음

## 왜 사용하나요?

어떤 서비스를 이용하기 위한 접근 방식을 일관된 방식으로 유지하여

애플리케이션에서 사용하는 기술이 변경되더라도 최소한의 변경만으로 변경된 요구 사항을 반영하기 위해 사용  
   
\=> PSA를 통해서 애플리케이션의 요구 사항 변경에 유연하게 대처 가능  
   
Spring은 상황에 따라 기술이 바뀌더라도 변경된 기술에 일관된 방식으로 접근할 수 있는 PSA를 지원

종류 : Spring Web MVC, Spring Transaction, Spring Cache, Spring Data, 메일 서비스 등이 있음

---

## **빈 스코프란?**

스코프는 빈이 존재할 수 있는 범위를 뜻함

## **종류**

| **Scope** | **Description** |
| --- | --- |
| singleton | (기본값) 스프링 IoC 컨테이너 당 하나의 인스턴스만 사용   앱이 구동되는 동안 하나만 사용 |
| prototype | 매번 새로운 빈을 정의해서 사용 |
| request(Web) | HTTP 라이프사이클마다 한 개의 빈을 사용(웹 요청이 들어오고 나갈 때까지)   Web-Aware ApplicationContext에서만 사용 가능 |
| session(Web) | HTTP 세션마다 하나의 빈을 사용(웹 세션이 생성되고 종료될 때까지)   Web-Aware ApplicationContext에서만 사용 가능 |
| application(Web) | ServeltContext 라이프사이클 동안 한 개의 빈만 사용   Web-Aware ApplicationContext에서만 사용 가능 |
| websocket(Web) | websocket 라이프사이클 안에서 한 개의 빈만 사용   Web-Aware ApplicationContext에서만 사용 가능 |

### **Prototype Bean 주의 사항**

싱글톤 스코프의 빈이 프로토타입의 빈을 주입 받는 경우

싱글톤 스코프의 빈이 생성되고 의존 관계가 주입되는 시점에만 프로토타입 빈이 조회되고, 이후에는 계속 같은 빈이 사용

**이 문제점을 해결하려면!**

#### **proxy mode 사용**

```
@Component
@Scope(value = "prototype", proxyMode = ScopedProxyMode.TARGET_CLASS)
public class ProtoType {
}
```

#### **ObjectProvider 객체 사용**

```
@Component
public class Single {

    @Autowired
    ObjectProvider<ProtoType> protoType;

    public ProtoType getProtoType() {
        return protoType.getIfAvailable();
    }
}
```

#### **그럼 언제 Prototype Bean을 사용하나요?**

-   여러 인스턴스를 검색해야 하는 경우
-   인스턴스를 지연해야 되거나 선택적으로 찾아야 하는 경우
-   순환 종속성을 깨기 위해서
-   스코프에 포함된 인스턴스로부터 더 작은 범위의 인스턴스를 찾아 추상화하기 위해

---

## Spring Framework를 왜 사용해야 할까요? 이유에 대해 설명해주세요.

제가 생각하는 가장 큰 이유는 확장성때문입니다.
Spring Framework는 모듈화, 의존성 주입을 통해 새로운 요구사항이나 기능을 빠르게 추가할 수 있고, 다양한 도구 및 라이브러리(스프링 부트, JPA등등)를 지원이 되기 때문에 개발 범위가 넓기 때문입니다.
한가지를 더 고른다면 개발자 친화적이기 때문입니다.
IoC, DI, AOP(관점지향 프로그래밍)을 통해 코드 재사용성, 유지보수성이 높기 때문입니다.


## **Spring Framework란?**

자바 엔터프라이즈 개발을 편하게 해주는 오픈소스 경량급 애플리케이션 프레임워크

#### 경량급

과거 EJB가 느리고 무거운 자바 서버(WAS)에서만 동작한 반면, 스프링은 WAS로부터 독립적인 개발 환경을 구축할 수 있습니다. 가장 단순한 서버환경인 톰캣(Tomcat)이나 제티(Jetty) 위에서도 완벽하게 동작

#### 애플리케이션 프레임워크

특정 계층이나, 기술, 업무 분야에 국한되지 않고 애츨리케이션의 전 영역을 포괄하는 범용적인 프레임워크

### IoC(제어의 역행)

코드의 최종 호출을 개발자가 결정하는 것이 아니라 스프링 프레임워크 내부에서 이루어지는 것

스프링은 이미 구조가 설계되어 있어, 스프링을 사용하는 개발자는 부품을 만들어 조립하는 형태로 개발합니다. 이는 객체의 의존성을 역전시켜 객체 간의 결합도를 줄이고 유연한 코드를 작성할 수 있게 돕습니다. 이를 통해, 가독성이나 코드 중복, 유지 보수가 쉬워지는 장점이 있습니다.

### DI(의존성 주입)

각각의 계층이나 서비스들 간에 의존성이 존재할 경우 스프링 프레임워크에서 스프링 내부의 객체들 간의 관계를 만들어주는 것

 [\[CS\] Spring DI란?

DI란?의존성 주입(외부에서 두 객체 간의 관계를 결정해주는 디자인 패턴), 객체를 직접 생성하는 게 아니라 외부에서 생성한 후 주입하는 방식과정A 객체에서 B, C 객체를 의존할 때, A 객체에서 B

no-delay.tistory.com](https://no-delay.tistory.com/68)

### AOP(관점 지향 프로그래밍)

프로그램 내 로직을 핵심 기능과 부가 기능(횡단 관심사라고도 말합니다)으로 분리하여 바라보는 프로그래밍 방식으로, 여러 모듈에서 공통적으로 사용하는 부가 기능을 핵심 기능에서 분리하여 관리하는 기능을 말합니다. 비즈니스 로직으로부터 부가 기능을 담당하는 로직을 분리함으로써, 코드를 단순하고 깔끔하게 작성하도록 돕습니다.

---

## Spring MVC란?

웹 계층에 서블릿(Servlet) API를 기반으로 클라이언트의 요청을 처리하는 모듈이 있는데 이를 스프링 웹 MVC(spring-web-mvc) 또는 스프링 MVC라고 한다.  
Spring MVC는 클라이언트의 요청을 편리하게 해주는 기능을 제공  
  

## 동작 방식과 구성 요소

#### DispatcherServlet

DispatcherServlet은 HttpServlet을 상속받아 사용하고, 서블릿으로 동작한다.

Dispatcher-Servlet란?HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러과거와 비교과거에는 모든 서블릿을 URL 매핑을 위해 web.xml에 모두 등록해

no-delay.tistory.com](https://no-delay.tistory.com/75)

#### DispatcherServlet → FrameworkServlet → HttpServletBean → HttpServlet

DispacherServlet을 사용하면 서블릿으로 등록하면서 모든 경로에 (urlPatterns=”/”)에 대해 매핑한다.

#### 요청 흐름

서블릿이 호출되면 HttpServlet이 제공하는 service()가 호출된다.  
스프링 MVC는 FramworkServlet.service()를 시작으로 여러 메서드가 호출되면서 DispacherServlet.doDispatch()가 최종적으로 호출된다.

#### 동작 순서

핸들러 조회 : 핸들러 매핑을 통해 URL에 매핑된 핸들러(컨트롤러)를 조회한다.  
핸들러 어댑터 조회 : 핸들러를 실행할 수 있는 핸들러 어댑터를 조회한다.  
핸들러 어댑터 실행 : 핸들러 어댑터를 실행한다.  
핸들러 실행 : 핸들러 어댑터가 실제 핸들러를 실행한다.  
ModelAndView 반환 : 핸들러 어댑터는 핸들러가 반환하는 정보를 ModelAndView로 변환하여 반환한다.  
viewResolver 호출 : 뷰 리졸버를 찾고 실행한다.  
View 반환 : 뷰 리졸버는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환한다.  
뷰 렌더링 : 뷰를 통해 뷰를 렌더링한다.

---

## Dispatcher-Servlet란?

HTTP 프로토콜로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임해주는 프론트 컨트롤러

### 과거와 비교

과거에는 모든 서블릿을 URL 매핑을 위해 web.xml에 모두 등록해주어야 했지만, dispatcher-servlet이 해당 어플리케이션으로 들어오는 모든 요청을 핸들링해주고 공통 작업을 처리면서 상당히 편리하게 이용할 수 있게 되었습니다. 우리는 컨트롤러를 구현해두기만 하면 디스패처 서블릿가 알아서 적합한 컨트롤러로 위임을 해주는 구조가 되었습니다.

### 정적 자원 처리

#### 1\. 정적 자원 요청과 애플리케이션 요청을 분리

-   /apps 의 URL로 접근하면 Dispatcher Servlet이 담당한다.
-   /resources 의 URL로 접근하면 Dispatcher Servlet이 컨트롤할 수 없으므로 담당하지 않는다.

모든 요청에 URL을 붙여주어야 하므로 코드가 복잡하고 직관적이지 않음

#### 2\. 애플리케이션 요청을 탐색하고 없으면 정적 자원 요청으로 처리

Dispatcher Servlet이 요청을 처리할 컨트롤러를 먼저 찾고, 요청에 대한 컨트롤러를 찾을 수 없는 경우에, 2차적으로 설정된 자원(Resource) 경로를 탐색하여 자원을 탐색  
\=> 영역 분리로 효율적인 리소스 관리를 지원할 뿐 아니라 추후에 확장을 용이하게 해준다는 장점이 있습니다.

### 동작 방식

![image](https://github.com/SsafyStudy13/CS_Study/assets/57094856/657fbb0b-6fcb-4361-a42d-b774c01b43eb)

1.  클라이언트의 요청을 디스패처 서블릿이 받음
2.  요청 정보를 통해 요청을 위임할 컨트롤러를 찾음
3.  요청을 컨트롤러로 위임할 핸들러 어댑터를 찾아서 전달함
4.  핸들러 어댑터가 컨트롤러로 요청을 위임함
5.  비지니스 로직을 처리함
6.  컨트롤러가 반환값을 반환함
7.  핸들러 어댑터가 반환값을 처리함
8.  서버의 응답을 클라이언트로 반환함

출처: [https://mangkyu.tistory.com/18](https://mangkyu.tistory.com/18) \[MangKyu's Diary:티스토리\]

---

## DTO(Data Transfer Object)란?

프로세스 간에 데이터를 전달하는 객체

데이터를 전송하기 위해 사용하는 객체라서 그 안에 비즈니스 로직 같은 복잡한 코드는 없고 순수하게 전달하고 싶은 데이터만 담겨있음

주로 클라이언트와 서버가 데이터를 주고받을 때 사용

**그럼 왜 사용하느냐!**

### 사용 이유

1.  View Layer와 DB Layer의 역할을 분리하기 위해서  
    \-> 객체를 표현하기 위한 계층과 저장하는 계층의 역할을 분리하기 위해서 DTO를 사용한다.
2.  Entity 객체의 변경을 피하기 위하여  
    \-> Entity 객체를 그대로 사용하면 프로그래머의 의도와 다르게 데이터가 변질될 수 있다. 
3.  View와 통신하는 DTO 클래스는 자주 변경된다  
    \-> View(클라이언트)와 통신하는 DTO 클래스, 예를 들어 ResponseDTO, RequestDTO는 요구사항에 따라 자주 변경된다. 어떤 요청에서는 특정 값이 추가될 수도 있고, 특정값이 없을 수도 있다.  따라서 Entity 클래스와 분리하여 관리해야 한다. 
4.  도메인 모델링을 지키기 위하여  
    도메인 설계를 잘하였다고 하더라도 원하는 데이터를 표시하기가 쉽지 않을 수 있다. 예를 들어 Entity 클래스의 특정 컬럼들을 조합하여 특정 포맷을 출력하고 싶다고 하자. Entity 클래스에 표현을 위한 필드나 로직이 추가되면 객체 설계를 망가뜨릴 수 있다. 따라서 DTO에 표현을 위한 로직을 추가해서 사용하는 것이 Entity의 도메인 모델링을 지킬 수 있다.
5.  오버헤드 발생 가능성 차단  
    Entity 그대로 사용한다면 불필요한 데이터를 사용할 수 있다.

출처 : [https://velog.io/@witwint/Spring-DTO%EC%9D%98-%EC%82%AC%EC%9A%A9%EC%9D%B4%EC%9C%A0](https://velog.io/@witwint/Spring-DTO%EC%9D%98-%EC%82%AC%EC%9A%A9%EC%9D%B4%EC%9C%A0)

---

## HTTP API란?

HTTP는 웹 환경에서 정보를 주고받기 위한 프로토콜로, HTTP API는 통신 규약으로 소통하는 API

## REST API란?

REST는 네트워크 아키텍처 스타일(네트워크 자원을 정의하고 처리하는 방법)로, REST API는 HTTP의 장점을 최대한 활용하기 위해 만들어진 API

### RESTful 설계 규칙

#### 1\. URI로 자원(리소스) 표현

```
GET /members/delete/1  (X)
DELETE /members/1.     (O)
```

delete와 같은 행위에 대한 표현이 들어가면 안됨

행위를 표현하고자 할 때는 HTTP Method (GET, POST, PUT, DELETE)로 표현

#### 2\. 정보의 자원을 표현

document, collection, store, controller 4가지 방식으로 자원을 표현할 수 있습니다. document란, 1개의 객체를 나타내는 것으로 일반적으로 단수명사로 나타나지며 collection 뒤에 나타나게 됩니다. collection은 resource(document)들의 집합입니다. 일반적으로 복수 명사를 사용합니다.

```
GET /sports/soccer/players/13
```

위와 같은 URI를 보면 sports, players는 collection으로 나타나지고 있고 soccer, 13은 도큐멘트로 URI를 구성하고 있습니다.

+) 그 외 자원 표현 방법들

-   명사형을 사용합니다.
-   소문자를 사용합니다.
-   밑줄 ( \_ ) 은 사용하지 않습니다.
-   하이픈 ( - )을 사용합니다.

#### 3\. 경로에 대한 표현

```
/university/sookmyung/ (X)
/university/sookmyung  (O)
```

슬래시 구분자 ( / ) 는 계층 관계를 나타내는데 사용합니다. URL 마지막에는 슬래시 구분자를 포함하지 않습니다. 또한, 경로 부분 중 변하는 부분은 유일한 값으로 대체합니다. (ex. id 값)

출처 : [https://bentist.tistory.com/37](https://bentist.tistory.com/37), [https://velog.io/@beneficial/HTTP%EC%99%80-REST-API%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80](https://velog.io/@beneficial/HTTP%EC%99%80-REST-API%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

---

##   annotation 이란 무엇인지, 어떤 것들이 있는지 설명해주세요.

사전적 의미로는 주석이라는 뜻이지만, 자바에서 Annotaion(@)은 코드 사이에 특별한 의미, 기능을 수행하도록 하는 기술이다.  
프로그램 코드의 일부가 아닌 프로그램에 관한 데이터를 제공하고, 코드에 정보를 추가하는 정형화된 방법이다.  
어노테이션을 사용하면 코드가 깔끔해지고 재사용이 가능하다.

-   컴파일러에게 코드 작성 문법 에러를 체크하도록 정보를 제공
-   소프트웨어 개발 툴이 빌드나 배치시 코드를 자동으로 생성할 수 있도록 정보를 제공
-   실행 시(런타임 시) 특정 기능을 실행하도록 정보를 제공

### 종류 참고

https://velog.io/@rara_kim/Spring-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98Annotation)

---

##   singleton pattern 에 대해 설명해주세요.

객체 지향 프로그래밍에서 특정 클래스가 단 하나만의 인스턴스를 생성하여 사용하기 위한 패턴

생성자를 여러 번 호출하더라도 인스턴스가 하나만 존재하도록 보장하여 애플리케이션에서 동일한 객체 인스턴스에 접근할 수 있도록 한다.

### 싱글톤 패턴을 사용하는 이유

커넥션 풀, 스레드 풀, 디바이스 설정 객체 등과 같은 경우 인스턴스를 여러 개 만들게 되면 불필요한 자원을 사용하게 되고, 프로그램이 예상치 못한 결과를 낳을 수 있다. 따라서 객체를 필요할 때마다 생성하는 것이 아닌 단 한 번만 생성하여 전역에서 이를 공유하고 사용할 수 있게 하기 위해 싱글톤 패턴을 사용한다.

### 싱글톤 패턴의 장단점

#### 장점

유일한 인스턴스 : 싱글톤 패턴이 적용된 클래스의 인스턴스는 애플리케이션 전역에서 단 하나만 존재하도록 보장한다. 이는 객체의 일관된 상태를 유지하고 전역에서 접근 가능하도록 한다.  
메모리 절약 : 인스턴스가 단 하나뿐이므로 메모리를 절약할 수 있다. 생성자를 여러 번 호출하더라도 새로운 인스턴스를 생성하지 않아 메모리 점유 및 해제에 대한 오버헤드를 줄인다.  
지연 초기화 : 인스턴스가 실제로 사용되는 시점에 생성하여 초기 비용을 줄일 수 있다.

#### 단점

결합도 증가 : 싱글톤 패턴은 전역에서 접근을 허용하기 때문에 해당 인스턴스에 의존하는 경우 결합도가 증가할 수 있다.  
테스트 복잡성 : 싱글톤 패턴은 단 하나의 인스턴스만을 생성하고 자원을 공유하기 때문에 싱글톤 클래스를 의존하는 클래스는 결합도 증가로 인해 테스트가 어려울 수 있다.  
상태 관리의 어려움 : 만약, 싱글톤 클래스가 상태를 가지고 있는 경우 전역에서 사용되어 변경될 수 있다. 이로 인해 예상치 못한 동작이 발생할 수 있다.  
전역에서 접근 가능 : 애플리케이션 내 어디서든 접근이 가능한 경우, 무분별한 사용을 막기 힘들다. 이로 인해 변경에 대한 복잡성이 증가할 수 있다.

#### 싱글톤 조건

-   new 키워드를 사용할 수 없도록 생성자에 private 접근 제어자를 지정해야 한다.
-   유일한 단일 객체를 반환할 수 있는 정적 메서드가 필요하다.
-   유일한 단일 객체를 참조할 정적 참조 변수가 필요하다.

출처: [https://ittrue.tistory.com/563](https://ittrue.tistory.com/563) \[IT is True:티스토리\]
