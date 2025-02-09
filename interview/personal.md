개인적으로 기술 면접에 준비하기 위한 문항들입니다.

-----------------------

### JVM 구조

<details>
    <summary> 예비 답안 </summary>
    <br />

- `jvm` 은 자바 가상 머신입니다. 
    - 자바와 운영체제 사이에서 중개자 역할을 수행하며 자바가 운영체제에 구애받지 않고 프로그램을 실행할 수 있도록 도와줍니다.
- JVM 의 구조는 
    - JVM 내로 클래스 파일을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈인 `Class Loader`
    - 클래스 로더를 통해 JVM 내의 Runtime Data Area 에 배치된 바이트 코드들을 명령어 단위로 실행하는 `Execution Engine`
    - 힙 메모리 영역에 생성된 객체들 중에서 참조되지 않은 객체들(Unreachable)을 탐색 후, 제거하는 역할을 하는 `Garbage Collector`
    - JVM 의 메모리 영역으로, 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역인 `Runtime Data Area`
    
```
jvm 은 크게 클래스 로더(Class Loader), 실행 엔진(Execution Engine), 런타임 데이터 영역(Runtime Data Area)으로 구성됩니다.
```
</details>

-----------------------

### JVM 의 메모리 구조

<details>
    <summary> 예비 답안 </summary>
    <br />

- JVM 의 메모리 구조는 모든 쓰레드에서 공유되는 Method 영역과 Heap 영역, 나머지 영역인 PC Register, Stack, Native Method Stack 영역으로 구분할 수 있습니다.

    - `Method 영역` → 모든 스레드가 공유하는 영역으로, 클래스/인터페이스/메소드/필드/static 변수 등의 바이트 코드를 보관합니다.
    - `Heap 영역` → 모든 스레드가 공유하는 영역으로, new 키워드로 생성된 모든 객체와 배열이 저장되는 영역입니다. 또한 메소드 영역에 로드된 클래스만 생성이 가능하고 GC 가 참조되지 않는 메모리를 확인하고 제거하는 영역입니다.
    - `PC Register` → 스레드가 시작될 때 생성되며, 스레드가 어떤 부분을 무슨 명령어로 실행해야 할 지에 대한 기록을 하는 부분으로, JVM 명령의 주소를 가집니다.
    - `Stack` → 메서드 호출 시마다 각각의 스택 프레임이 생성됩니다. 그리고 메서드 안에서 사용되는 값들을 젖아하고 , 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장하며 메서드 수행이 끝나면 프레임별로 됩니다.
    - `Native Method Stack` → 자바 외의 언어로 작성된 네이티브 코드를 위한 스택입니다.
    
</details>

-----------------------

### Garbage Collector 이란 무엇인지

<details>
    <summary> 예비 답안 </summary>
    <br />

- GC, 가비지 컬렉션은 JVM의 Heap 영역에서 더 이상 참조되지 않는 객체를 일정 주기로 찾아내고 메모리를 회수하는 기능입니다.
- 프로그램이 사용하지 않는 메모리를 주기적으로 해제함으로써 애플리케이션의 안전성과 지속 가능성을 유지합니다.
- 힙 영역은 Young 과 Old Generation 영역으로 나뉘는데, 이 영역은 Minor GC 와 Full GC 를 판가름하는 대상입니다.
    - `Young Generation` 영역은 짧게 살아남는 메모리들이 존재하는 공간입니다. 모든 객체는 처음에는 Young Generation 에 생성되며, Young Generation 의 공간은 Old Generation 에 비해 상대적으로 적기 때문에 메모리 상의 객체를 찾아 제거하는데 적은 시간이 걸립니다. (Minor GC)
    - 새롭게 생성되는 객체는 Young Generation 영역 중 `Eden` 에서 생성되며, Eden 공간이 가득차면 MinorGC가 동작하여 생존한 객체가 증가된 age-bit과 함께 `Survivor0` 영역으로 넘어가게 됩니다.
    - 위의 과정을 통해 Young Generation 의 마지막 영역인 `Survivor1` 영역의 GC에서도 살아남는다면 Old Generation 영역으로 넘어가게 됩니다.
    - `Old Generation` 은 길게 살아남는 메모리들이 존재하는 공간입니다. Old Generation의 객체들은 처음에는 Young Generation 에 의해 시작되었으나, GC 과정 중에 제거되지 않은 경우 Old Generation로 이동합니다. (Major GC)
    - Old Generation 에서 발생하는 Major GC 는 매우 큰 공간이기 때문에 데이터를 지우는데 많은 시간이 소요되며, Major GC 가 발생하면 Thread 가 멈추고(Stop The World) Mark and Sweep 작업을 해야 해서 CPU에 부하를 줄 수 있습니다.

### Mark And Sweep
![poster](../image/jvm/ms.png)

Mark-Sweep 이란 다양한 GC에서 사용되는 객체를 솎아내는 내부 알고리즘입니다. 가비지 컬렉션이 동작하는 기초적인 청소 과정이라고 생각하면 됩니다.

원리는 가비지 컬렉션이 될 대상 객체를 `식별(Mark)`하고 `제거(Sweep)`하며 객체가 제거되어 파편화된 메모리 영역을 앞에서부터 `채워나가는 작업(Compaction)`을 수행하게 됩니다.

- Mark 과정 : 먼저 `Root Space` 로부터 그래프 순회를 통해 연결된 객체들을 찾아내어 각각 어떤 객체를 참조하고 있는지 찾아서 마킹합니다.
- Sweep 과정 : 참조하고 있지 않은 객체, 즉 Unreachable 객체들을 Heap 에서 제거합니다.
- Compact 과정 : Sweep 후에 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 압축합니다. (가비지 컬렉터 종류에 따라 하지 않는 경우도 존재합니다.)

⭐️ Mark And Sweep 방식은 루트로 부터 해당 객체에 접근이 가능한지가 해제의 기준이 됩니다. JVM GC에서의 Root Space는 `Heap 메모리 영역을 참조`하는 method area, static 변수, stack, native method stack 이 있습니다.

</details>

-----------------------

### Spring Framework 란

<details>
    <summary> 예비 답안 </summary>
    <br />

- 자바 기반 애플리케이션 개발을 지원하는 오픈 소스 프레임워크 입니다.
- 엔터프라이즈급 애플리케이션을 개발하기 위한 모든 기능을 종합적으로 제공하는 경량화된 솔루션입니다.
    - 대규모 데이터 처리와 트랜잭션이 동시에 여러 사용자로부터 행해지는 매우 큰 규모의 환경을 매니징하는 엔터프라이즈 환경
- Spring Framework 는 경량 컨테이너로 자바 객체를 담고 직접 관리합니다.
    
</details>

-----------------------

### Spring Framework 를 사용하는 이유

<details>
    <summary> 예비 답안 </summary>
    <br />

- 생산성을 높이고 유지보수를 용이하게 할 수 있습니다. 
- 프로젝트를 여러 모듈로 나눠, 각 모듈이 독립적으로 개발/배포/테스트가 가능하게 할 수 있습니다.
    - 모듈화된 아키텍처를 구현할 수 있으며 Spring 은 다양한 기술과 통합이 가능합니다.
    - 필요에 따라 새 기능을 추가하거나 확장에 용이합니다.

- 특징
1. `IoC` → `객체 생성과 의존성 관리를 개발자가 아닌 프레임워크가 대신 처리하여 코드의 결합도를 낮추고 테스트를 용이하게 만든다.` 즉, 객체를 매번 new 로 생성하지 않고, 컨테이너가 필요 시 주입하기 때문에 코드의 복잡성을 줄이고, 개발자가 비즈니스 로직에만 집중할 수 있게 해줍니다.
2. `AOP` → 로깅, 트랜잭션 관리와 같은 횡단 관심사를 분리해 코드의 가독성과 재사용성을 높인다. 즉, 공통된 기능을 비즈니스 로직과 분리할 수 있다는 장점으로 유지보수에 용이하다.
3. `DI` → `의존성 주입, 클래스 간 결합도를 낮추고, 새로운 요구사항에 맞춰 변경해야 할 부분을 최소화 할 수 있습니다`. 특정 구현체가 변경 시 인터페이스를 통해 쉽게 교체가 가능합니다.
4. `데이터 접근 간소화` → JDBC, JPA 와 같은 데이터 접근 기술과의 통합을 제공하여 데이터 처리를 간단하게 만들어줍니다. 즉, 데이터베이스 작업에 필요한 반복적인 코드를 대폭 줄일 수 있습니다.
5. `모듈화된 설계` → Core, Data Access, Web, Security 등 다양한 모듈로 구성되어 필요에 따라 선택적으로 사용할 수 있다.
6. 유연한 설정 방식 → XML, Java Config, 어노테이션 기반 설정을 모두 지원한다. 즉, 설정 파일의 중앙화 때문에 환경 변화에 유연하게 대응이 가능합니다.
    
</details>

-----------------------

### logger 를 지향하고, System.out.println() 을 지양하는 이유

<details>
    <summary> 예비 답안 </summary>
    <br />

1. 성능 문제

    System.out.println(stdout) 은 Blocking I/O 로 동작하므로, 로그를 출력하는 동안 애플리케이션이 멈출 수 있습니다. 특히, 대량의 로그를 출력하는 경우 성능이 크게 저하됩니다. 예시로 stdout 을 다량 사용하면 GC 의 영향을 받아 애플리케이션 응답 속도가 느려질 수 있습니다.<br>
    또한, System.out 은 synchronized 메서드(Thread-safe)라서 여러 스레드가 동시에 로그를 출력할 경우 성능 병목이 발생할 수 있습니다.
    
    💡 Blocking I/O 는 입출력(I/O) 작업이 완료될 때까지 프로그램 실행이 멈추는 방식으로, 한 번에 하나의 작업만 수행되며 현재 작업이 끝나야만 다읍 작업이 시작될 수 있습니다.

    💡 Thread-safe 란, 여러 개의 스레드가 동시에 같은 자원에 접근해도 문제가 발생하지 않는 상태를 의미합니다. 즉, 여러 스레드가 동시에 실행해도 데이터가 손상되지 않습니다. 하지만 Thread-safe 를 위해 synchronized 키워드를 사용하므로 성능 병목(Bottleneck)이 발생합니다. 즉, 여러 스레드가 동시에 실행되면 한 스레드가 출력하는 동안 다른 스레드는 대기합니다.

2. 로그 관리 효율성 증가

    stdout 으로 출력하면 전체 로그를 출력해야 하므로 필요없는 로그까지 출력될 수 있습니다. 또한 stdout 은 서버를 재시작하면 로그가 사라지는 휘발성이기 때문에 log4j 를 사용하여 파일 또는 원격 서버로 저장이 가능합니다.
</details>

-----------------------

### Filter 과 Interceptor

<details>
    <summary> 예비 답안 </summary>
    <br />

`Filter` 는 HTTP 요청을 가로채어, 특정 작업을 수행할 수 있도록 하는 컴포넌트입니다. 주로 보안/로깅/데이터 처리/요청 수정 등 다양한 작업을 처리할 수 있습니다. Spring Filter 는 Java Servlet API 의 javax.servlet.Filter 인터페이스를 구현하며, Spring Boot 환경에서는 이를 더욱 간편하게 활용할 수 있습니다.

`Intercepter` 는 Spring 의 HandlerIntercepter 인터페이스를 구현하여 요청 전/후 및 완료 단계에서 처리 로직을 삽입할 수 있다.

특징|Filter|Intercepter
|---|---|----|
위치     | 서블릿 컨테이너 레벨에서 동작 | Spring 의 HandlerMapping 레벨에서 동작
적용 범위 | 모든 요청 및 응답 처리 가능 | 주로 Spring MVC 의 컨트롤러 요청/응답에 처리
기술 스택 | Servlet API 기반 | Spring AOP 기반
용도     | 요청/응답 가로채기,로깅, 보안 처리 등 | 컨트롤러 전/후 처리, 비즈니스 로직 전/후 처리

실제 요청 흐름

클라이언트 -> Filter -> DispatcherServlet -> Intercepter (preHandle) -> Controller -> Intercepter (postHandler) -> Intercepter (afterCompletion) 응답
</details>

-----------------------

### Checked vs Unchecked Exception 

<details>
    <summary> 예비 답안 </summary>
    <br />

`Checked Exception` 은 RuntimeException 을 상속하지 않는 클래스로, 컴파일 시점에 컴파일러에서 확인하는 예외입니다. 반드시 에러 처리를 해야 하는 특징(try/catch or throw) 을 가지고 있습니다.
ex )
    IOException: 파일 또는 네트워크 연결에서 읽기 또는 쓰기와 같은 입력/출력 작업과 관련된 오류
    SQLException: 데이터베이스 액세스 및 쿼리와 관련된 오류
    ClassNotFoundException: 동적으로 클래스 로드와 관련된 오류
    InterruptedException: 스레드 중단 및 동기화와 관련된 오류의 경우

`Unchecked Exception` 는 RuntimeException 을 상속하는 클래스로, 런타임 단계에서 확인이 가능하며 에러 처리를 강제하지 않습니다. 개발자가 예상치 못한 에러가 발생할 수 있기 때문에 예외처리를 강제하지 않는다는 의미입니다.

</details>

-----------------------

### Maven vs Gradle
<details>
    <summary> 예비 답안 </summary>
    <br />


|비교 항목|Maven|Gradle|
|---| ---- | ---- |
|빌드 스크립트|	XML (pom.xml) | Groovy/Kotlin (build.gradle)|
|빌드 속도|	느림 (단계별 실행) | 빠름 (태스크 단위 실행, 캐싱 지원)|
|의존성 관리|	선언적 방식 | 유연한 방식|
|사용 방식|	설정 기반 (Configuration) | 코드 기반 (Convention)|
|확장성|	플러그인 기반 | 커스텀 태스크 작성 가능|
|병렬 처리|	기본적으로 지원 X | 병렬 실행 가능|

    Maven 은 설정 중심이며, Gradle 은 코드 기반으로 속도가 빠르고 유연하다는 차이점이 있습니다.
    
</details>

-----------------------
