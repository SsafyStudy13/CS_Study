## 1\. 가비지 컬렉션에 대해 설명해주세요.

가비지컬렉션이란 자바의 메모리 관리 방법 중 하나로 JVM(자바 가상 머신)의 Heap 영역에서 동적으로 할당했던 메모리 중 필요 없게 된 메모리 객체(garbage)를 모아 주기적으로 제거하는 프로세스를 말합니다.

장점: Java 프로세스가 한정된 메모리를 효율적으로 사용할수 있게 도와주며, 개발자가 직접 메모리 관리를 하지 않아도 됨  
단점: 제어가 힘들며, 다른 동작이 멈춰(STW) 오버헤드가 발생할 수 있음

동작 방식과 종류 추후 업데이트 예정

## 2\. 객체지향 프로그래밍이 뭔가요?

객체지향 프로그래밍은 데이터와 해당 데이터를 처리하는 메소드를 하나로 묶어서 객체를 만들고, 이러한 객체들이 서로 상호작용하면서 프로그램을 구성하는 것입니다.

특징으로는 캡슐화, 상속, 다형성, 추상화 등이 있고, 모듈 재사용으로 확장 및 유지보수가 용이합니다.  
  

객체 지향 프로그래밍에서는 코드를 객체 단위로 구성하므로, 같은 기능을 하는 코드를 여러 번 작성할 필요가 없음 -> 모듈 재사용

또한, 각 객체가 독립적으로 작동하므로, 코드의 유지보수 용이

## 3\. 오버라이딩과 오버로딩의 차이점을 설명해주세요.

오버로딩(Overloading) : 같은 이름의 메서드 여러개를 가지면서 매개변수의 유형과 개수가 다르도록 하는 기술 ex) 생성자  
오버라이딩(Overriding) : 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의해서 사용 ex) toString

다형성에 속함

## 4\. 자바의 메모리 영역에 대해 설명해주세요.

Static 영역(Method 영역): Static으로 선언된 변수, 생성자, 메소드가 저장됨, JVM 종료 시 메모리에서 해제

Heap 영역: 참조형 데이터 객체의 실제 데이터가 저장, Stack 영역에서 실제 데이터가 존재하는 Heap 영역의 참조값을 가지고 있음

Stack 영역: 기본 자료형, 지역 변수, 매개 변수 저장, Heap 영역의 데이터들을 가르키는 Reference(참조 주소) 할당

메소드가 호출되면 메모리에 할당, 종료 시 삭제

new 키워드로 인스턴스를 생성 할 때, Heap 영역에는 생성된 객체가 저장, Stack 영역에서 생성된 객체에 대한 주소 값(Reference)이 저장됨

## 5\. SOLID 원칙에 대해 설명해주세요.

SOLID는 객체 지향 설계 5원칙으로 단일 책임 원칙, 개방 폐쇄 원칙, 리스코프 치환 원칙, 인터페이스 분리 원칙, 의존 역전 원칙이 있습니다.

-   SRP (Single Responsibility) 단일 책임 원칙  
    클래스는 단 한개의 책임을 가져야 함  
    클래스를 변경하는 이유는 단 하나여야 함
-   OCP (Open-Closed) 개방-폐쇄 원칙  
    신규 기능 확장에는 열려있어야 하고, 변경에는 닫혀 있어야 함  
    즉, 기존의 코드를 변경하지 않고 기능을 수정하거나 추가할 수 있도록 설계해야 함
-   LSP (Liskov Substitution) 리스코프 치환 원칙  
    하위 타입 객체는 상위 타입 객체에서 가능한 행위를 수행할 수 있어야 함→ 즉, 상위 타입 객체를 하위 타입 객체로 치환해도 정상적으로 동작해야 함
-   ISP (Interface Segregation) 인터페이스 분리 원칙  
    클라이언트는 자신이 사용하는 메소드에만 의존해야 한다는 원칙  
    한 클래스는 자신이 사용하지 않는 인터페이스는 구현하지 않아야 함→ 하나의 통상적인 인터페이스보다는 차라리 여러 개의 세부적인 (구체적인) 인터페이스가 나음  
    인터페이스는 해당 인터페이스를 사용하는 클라이언트를 기준으로 잘게 분리되어야 함
-   DIP (Dependency Inversion) 의존 역전 원칙  
    의존 관계를 맺을 때, 하위개념보다는 상위개념을 의존하여야 함  
    추상화 O, 구체화 X

정리  
SRP 와 ISP 는 객체가 커지는 것을 막아준다. 객체가 단일 책임을 갖도록 하고 클라이언트마다 특화된 인터페이스를 구현하게 함으로써 한 기능의 변경이 다른 곳까지 미치는 영향을 최소화하고, 이는 기능 추가 및 변경에 용이하도록 만들어 준다.  
LSP 와 DIP 는 OCP 를 서포트한다. OCP 는 자주 변화되는 부분을 추상화하고 다형성을 이용함으로써 기능 확장에는 용이하되 기존 코드의 변화에는 보수적이도록 만들어 준다. 여기서 '변화되는 부분을 추상화'할 수 있도록 도와주는 원칙이 DIP 이고, 다형성 구현을 도와주는 원칙이 LSP 인 것이다.

## 6\. 자바의 컴파일 과정에 대해 설명해주세요.

개발자가 .java 파일을 생성한다.  
build를 한다.  
java compiler의 javac의 명령어를 통해 바이트코드(.class)를 생성한다.  
Class Loader를 통해 JVM 메모리 내로 로드한다.  
실행엔진을 통해 컴퓨터가 읽을 수 있는 기계어로 해석된다.(각 운영체제에 맞는 기계어)  
  

1\. 개발자가 자바 소스코드(.java)를 작성합니다.  
2\. 자바 컴파일러(Java Compiler)가 자바 소스파일을 컴파일합니다. 이때 나오는 파일은 자바 바이트 코드(.class)파일로 아직 컴퓨터가 읽을 수 없는 자바 가상 머신이 이해할 수 있는 코드입니다. 바이트 코드의 각 명령어는 1바이트 크기의 Opcode와 추가 피연산자로 이루어져 있습니다.  
3\. 컴파일된 바이트 코드를 JVM의 클래스로더(Class Loader)에게 전달합니다.  
4\. 클래스 로더는 동적로딩(Dynamic Loading)을 통해 필요한 클래스들을 로딩 및 링크하여 런타임 데이터 영역(Runtime Data area), 즉 JVM의 메모리에 올립니다.

-   클래스 로더 세부 동작
    -   로드 : 클래스 파일을 가져와서 JVM의 메모리에 로드합니다.
    -   검증 : 자바 언어 명세(Java Language Specification) 및 JVM 명세에 명시된 대로 구성되어 있는지 검사합니다.
    -   준비 : 클래스가 필요로 하는 메모리를 할당합니다. (필드, 메서드, 인터페이스 등등)
    -   분석 : 클래스의 상수 풀 내 모든 심볼릭 레퍼런스를 다이렉트 레퍼런스로 변경합니다.
    -   초기화 : 클래스 변수들을 적절한 값으로 초기화합니다. (static 필드)

5\. 실행엔진(Execution Engine)은 JVM 메모리에 올라온 바이트 코드들을 명령어 단위로 하나씩 가져와서 실행합니다. 이때, 실행 엔진은 두가지 방식으로 변경합니다.

-   인터프리터 : 바이트 코드 명령어를 하나씩 읽어서 해석하고 실행합니다. 하나하나의 실행은 빠르나, 전체적인 실행 속도가 느리다는 단점을 가집니다.
-   JIT 컴파일러(Just-In-Time Compiler) : 인터프리터의 단점을 보완하기 위해 도입된 방식으로 바이트 코드 전체를 컴파일하여 바이너리 코드로 변경하고 이후에는 해당 메서드를 더이상 인터프리팅 하지 않고, 바이너리 코드로 직접 실행하는 방식입니다. 하나씩 인터프리팅하여 실행하는 것이 아니라 바이트 코드 전체가 컴파일된 바이너리 코드를 실행하는 것이기 때문에 전체적인 실행속도는 인터프리팅 방식보다 빠릅니다.

## 7\. 클래스와 인터페이스의 차이점에 대해 설명해주세요.

추상 클래스는 '추상 메소드'가 하나 이상 포함되거나 abstract로 정의된 경우를 뜻함, 선언과 구현이 모두 존재

인터페이스는 모든 메소드가 추상 메소드

추상 메서드: 메서드의 선언부만 존재하고 구현코드가 없는 메서드

\[목적\]

추상 클래스는 기본적으로 클래스 이며 이를 상속, 확장하여 사용

인터페이스는 해당 인터페이스를 구현한 객체들에 대한 동일한 사용방법과 동작을 보장하기 위해 사용

## 8\. 접근 제어자란 무엇인지, 어떤 것들이 있는지 설명해주세요.

클래스와 클래스 멤버(변수, 메소드, 생성자)를 사용할 때, 접근할 수 있는 범위를 지정

종류로는 public, default(패키지), protected, private이 있습니다.

public: 모든 클래스에서 접근 가능(클래스와 클래스 멤버 둘다 사용 가능)

default: 패키지 안의 클래스에서만 접근 가능(접근제어자를 생략하면 이 경우에 해당, 클래스와 클래스 멤버 둘다 사용 가능)

protected: 같은 패키지 안의 클래스와 다른 패키지의 자식 클래스에 접근 가능(클래스 멤버만 사용 가능)

private: 같은 클래스 안에서만 접근 가능(클래스 멤버만 사용 가능)

사용 이유: 캡슐화를 지켜주기 위해

## 9\. 자바의 예외 처리 방법에 대해 설명해주세요.

try-catch와 throws가 있습니다.

try-catch는 실행할 부분인 try, 어떤 예외가 발생할 경우 처리하는 부분인 catch, 예외가 나도 실행하는 부분인 finally(필수X)로 이루어 집니다. 보통 catch 부분에 로그를 남기는 처리를 합니다.

반면 throws는 예외를 떠넘기는 방법으로 메소드 앞에 throws가 있다면, 해당 메소드를 호출한 쪽에 예외를 전가합니다.

예외를 처리하지 않으면 비정상 종료가 되면서 이 예외는 JVM에게 넘겨지게 됩니다. JVM이 받아서 JVM 기본 예외처리기에 의해 마지막으로 처리를 하게 되어 오류를 출력합니다.

## 10\. 자바의 Exception 종류에 대해 설명해주세요.

RuntimeException(UnCheckedException): 컴파일러가 예외를 체크하지 않아, 컴파일 후 런타임 시 발생할 수 있는 예외, 명시적인 예외 처리를 해주지 않아도 컴파일이 됨

ex) NullPointerException(NPE), ArithmeticException (0으로 나눌경우), ArrayIndexOutOfBoundsException

OtherException(CheckedException): 컴파일 시점에서 예외 발생과 처리를 확인, 예외 처리가 필요

ex) IOException, SQLException

## 11\. 자바에서 왜 String을 불변 객체로 설정했는지 설명해주세요.

성능, 보안, 캐싱, 동기화같은 이유로 불변 객체로 만들었습니다.

String Literal로 생성 시 Heap 영역 내 String Constant Pool에 저장, new로 생성하면 Heap 영역에 매번 객체 생성됨

String Constant Pool를 통해 같은 값에 대해서는 String 객체를 다시 만들지 않고 이미 존재하는 객체를 참조하여, 재사용성을 높일 수 있습니다.

변경 가능한 문자열은 SQL 주입에 취약해지는 등 시간이 지남에 따라 보안에 위험이 생길 수 있습니다.

불변객체라 변경되지 않는 문자열을 보장하기 때문에 캐싱이 가능합니다.

값이 바뀌지 않기때문에 멀티스레드 환경에서 Thread-safe합니다.

불변 객체: 생성된 후 내부 상태가 일정하게 유지, 참조 업데이트 불가

## 12\. 알고있는 자바 JDK 버전과 각 버전 별 변경 사항에 대해 설명해주세요.

JDK 1.8

-   람다(명칭없이 구현부 선언 익명 메소드 생성 문법)를 통해 선언 간소화
-   Stream API: 순차, 병렬 갖겁을 지원, 컬렉션 반복 처리로 간결한 코드 작성 가능

JDK 10

-   로컬 변수 타입 'var'로 선언 가능

JDK 11

-   HTTP 클라이언트 API 정식 포함(HTTP 2.0 지원으로 웹 소켓 기능 포함)
-   람다 파라미터 var 사용 가능
