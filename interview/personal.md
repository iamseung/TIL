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
jvm 은 크게 jvm 내로 클래스 파일을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈인 클래스 로더(Class Loader), jvm 내의 Runtime Data Area에 배치된 코드들을 명령어 단위로 실행하는 실행 엔진(Execution Engine), JVM 의 메모리 영역으로, 자바 애플리 케이션을 실행할 때 사용되는 데이터들을 적재하는 런타임 데이터 영역(Runtime Data Area), 힙 메모리 영역에 더 이상 참조되지 않는 객체들을 탐색 후 제거하는 역할을 하는 갈비지 컬렉터(Garbage Collector)으로 구성됩니다.
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
    - `Stack` → 메서드 호출 시마다 각각의 스택 프레임이 생성됩니다. 그리고 메서드 안에서 사용되는 값들을 저장하고, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장하며 메서드 수행이 끝나면 프레임별로 됩니다.
    - `Native Method Stack` → 자바 외의 언어로 작성된 네이티브 코드를 위한 스택입니다.

```
JVM 의 메모리 구조는 모든 쓰레드에서 공유되는 Method 영역과 Heap 영역, 나머지 영역인 PC Register, Stack, Native Method Stack 영역으로 구분할 수 있습니다.
클래스, 인터페이스, 메소드, 필드, static 변수 등의 데이터를 보관하는 Method 영역, new 키워드로 생성된 모든 객체와 배열이 저장되는 영역인 Heap 영역, 쓰레드가 어떤 부분을 무슨 명령으로 실행해야할 지에 대한 기록을 하는 부분으로 현재 수행중인 JVM 명령의 주소를 갖는 PC Register 영역, 자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역인 Native method stack 영역, 메서드 호출 시마다 각각의 스택 프레임이 생성되며 메서드 안에서 사용되는 값들을 저장하고, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장하는 Stack 영역이 있습니다.
```

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

### Spring Framework 와 Spring Boot 의 차이

<details>
    <summary> 예비 답안 </summary>
    <br />

### Spring Framework 와 Spring Boot 의 차이

1. **설정의 단순화**: 
Spring Framework를 사용하여 애플리케이션을 설정하려면 XML 또는 Java 기반의 설정이 필요하며, 데이터 소스, 뷰 리졸버, 컴포넌트 스캔 등 많은 세부 사항을 처리해야 한다. 반면에 Spring Boot는 이러한 설정을 자동화해준다. Spring Boot는 '의견을 가진(opinionated)' 설정을 통해 애플리케이션 개발에 최적화된 기본 설정을 제공한다.
2. **내장 서버**: 
Spring Framework 애플리케이션을 실행하려면 별도의 서버(WAS)가 필요하다. 반면에 Spring Boot는 Tomcat, Jetty, Undertow 등의 서버를 내장하고 있어, 별도의 WAS 설치 없이 애플리케이션을 실행할 수 있다.
3. **스타터 의존성**: 
Spring Boot는 스타터 의존성을 제공한다. 이는 필요한 라이브러리들을 그룹화하여 제공하므로, 개별적인 라이브러리를 찾고 추가하는 번거로움을 줄여준다.
4. **Actuator**: 
Spring Boot Actuator는 애플리케이션의 모니터링과 관리를 위한 기능을 제공한다. 이는 Spring Framework 자체에는 포함되어 있지 않는 기능이다.

### Spring Boot 가 가지고 있는 추가적인 기능

1. **YAML 지원**: 
Spring Boot는 설정 파일을 작성할 때 Java나 XML 뿐만 아니라 YAML 파일도 지원한다. 
이는 구조화된 데이터를 표현하는 데 매우 유용한 형식으로, 특히 복잡한 데이터 구조를 다룰 때 가독성이 더 좋다.
2. **Spring Boot DevTools**: 
Spring Boot는 개발 중에 애플리케이션을 자동으로 재시작하고, 라이브 리로드를 제공하는 DevTools를 제공한다. 
이는 개발 과정을 더욱 효율적으로 만들어 준다.
3. **배너 커스터마이징**: 
Spring Boot는 애플리케이션 시작 시 나타나는 배너를 커스터마이징 할 수 있게 해준다. 
이는 사소한 기능일 수 있지만, 애플리케이션의 개성을 표현하거나 명확한 식별 정보를 제공하는 데 도움이 될 수 있다.
4. **스프링 부트의 Executable JARs/WARs**: 
스프링 부트는 실행 가능한 JARs 또는 WARs 생성이 가능하며, 이는 단독으로 실행 가능한 스프링 애플리케이션을 만드는데 매우 편리하다.
이는 전통적인 WAR 파일 배포와 비교하여 배포와 실행을 단순화한다.

```
Spring Boot는 Spring Framework 위에 구축되어 동일한 기술 스택을 사용하지만, 설정의 자동화, 내장 서버, 스타터 의존성 등을 통해 개발과 배포 과정을 대폭 단순화시켜준다. 
이는 개발자가 복잡한 설정과 인프라에 대한 걱정 없이 비즈니스 로직에 집중하게 해준다는 장점이 있다.
```

https://www.inflearn.com/blogs/3315?gad_source=1&gclid=CjwKCAiAiOa9BhBqEiwABCdG81uOlX88AWI7HFBQmJDPDzUyQ9o4j7AgZ4JVQpAt9F6wY3yVRiVD1RoC3g0QAvD_BwE

    
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

### HashMap vs TreeMap
<details>
    <summary> 예비 답안 </summary>
    <br />

- **`HashMap`은 해시 테이블을 기반으로 key를 탐색하므로 평균적으로 O(1)의 속도를 가집니다.**
- **반면, `TreeMap`은 Red-Black Tree(균형 이진 탐색 트리)를 기반으로 O(log N)의 속도를 가지며, key를 자동으로 정렬해줍니다.**

**따라서, 빠른 탐색이 필요하면 `HashMap`을, key 정렬이 필요하면 `TreeMap`을 사용합니다.**
    
</details>

-----------------------

### IoC, 제어의 역전이란
<details>
    <summary> 예비 답안 </summary>
    <br />

IoC 는 객체의 생성과 관리를 스프링 프레임워크가 대신하도록 위임하도록 하는 것입니다.스프링 컨테이너에 Bean 을 미리 등록하고, 필요한 곳에서 컨테이너의 빈을 가져와 사용할 수 있게 해줍니다.
이때 Bean 을 가져올 때 의존성 주입(DI) 방식을 사용하여 객체 간의 의존성을 자동으로 주입받을 수 있게 됩니다.

예시로 Controller, Service 와 같은 객체들의 동작을 우리가 직접 구현하기는 하지만, 해당 객체들이 어느 시점에 호출될 지는 신경쓰지 않습니다. 단지 프레임워크가 요구하는대로 객체를 생성하면, 프레임워크가 해당 객체들을 가져다가 생성하고, 메서드를 호출하고, 소멸시킵니다. → 프로그램의 제어권이 역전된 것
    
</details>

-----------------------


### HTTP vs HTTPS
<details>
    <summary> 예비 답안 </summary>
    <br />

HTTP와 HTTPS의 가장 큰 차이는 보안성입니다.

HTTP(HyperText Transfer Protocol)는 데이터를 암호화하지 않고 평문으로 전송하는 반면, HTTPS(HyperText Transfer Protocol Secure)는 SSL/TLS 프로토콜을 적용해 데이터를 암호화하여 안전하게 전송합니다.

이로 인해 HTTPS는 `중간자 공격(MITM)`과 같은 보안 위협을 방지할 수 있으며, 사용자 신뢰도와 웹사이트의 안전성을 높이는 데 중요한 역할을 합니다.

기술적인 차이로는,

HTTP는 포트 80을 사용하고, HTTPS는 포트 443을 사용합니다.
HTTPS는 SSL/TLS 인증서를 필요로 하며, 이를 통해 웹사이트의 신뢰성을 검증합니다.
또한, HTTPS는 검색 엔진 최적화(SEO)에서도 HTTP보다 유리하며, 최근 웹 브라우저들은 HTTP 사이트에 대해 "보안되지 않음" 경고를 표시하기 때문에, 대부분의 웹사이트가 HTTPS를 기본적으로 사용하고 있습니다.
결론적으로, HTTPS는 HTTP의 보안성을 강화한 프로토콜이며, 현대적인 웹 환경에서는 HTTPS가 필수적으로 사용되어야 합니다.
    
</details>

-----------------------

### 패스워드 인코더를 사용한 데이터베이스의 저장 과정
<details>
    <summary> 예비 답안 </summary>
    <br />

1. 사용자가 패스워드를 입력
- 사용자가 회원가입 또는 로그인 시 비밀번호를 입력합니다.

2. 패스워드 해싱 (Password Hashing)
- 입력된 패스워드는 단순한 암호화(encryption)가 아니라 일방향 해시 함수(One-way Hash Function) 를 사용하여 변환됩니다.
- 대표적인 해시 알고리즘
    - BCrypt
    - PBKDF2
    - Argon2
    - SHA-256 (단독 사용은 추천되지 않음)

3. 솔트(Salt) 추가
- 해시 함수 적용 전에 솔트(Salt) 라는 무작위 문자열을 추가하여 저장합니다.
- 솔트는 사용자의 비밀번호가 동일하더라도 결과 값이 달라지도록 하기 위해 사용됩니다.
- 솔트를 적용하지 않으면 동일한 비밀번호가 같은 해시 값으로 저장되기 때문에 보안이 취약해집니다.

4. 해시된 패스워드를 저장
- 결과적으로 나온 해시 값(암호화된 패스워드)을 데이터베이스에 저장합니다.
- 일반적으로 비밀번호 자체는 저장되지 않고, 해시 값과 솔트 값만 저장됩니다.
    
</details>

-----------------------

### BCrypt 란 ?
<details>
    <summary> 예비 답안 </summary>
    <br />

BCrypt는 보안성이 뛰어난 비밀번호 해싱 알고리즘으로, 비밀번호를 안전하게 저장하는 데 사용됩니다.
특히, 솔트(Salt) 추가, 비용 인자(Cost Factor) 적용, Brute Force(무차별 대입 공격) 방어 기능을 제공합니다.

#### 1. BCrypt의 특징

✅ 1) 단방향 해싱 (One-way Hashing)
- 암호화가 아니라 해싱(Hashing) 기법을 사용하여, 해싱된 값에서 원래의 패스워드를 복원할 수 없음.

✅ 2) 솔트(Salt) 자동 생성
- 랜덤한 솔트(Salt)를 생성하여 패스워드와 함께 해싱하므로, 동일한 패스워드라도 해시 결과가 다름.
- 이를 통해 레인보우 테이블 공격(Rainbow Table Attack) 을 방어할 수 있음.

✅ 3) 비용 인자(Cost Factor) 설정 가능
- 연산 횟수를 조정하여, 컴퓨팅 성능에 따라 보안 강도를 조절할 수 있음.
- 기본적으로 2^cost 만큼의 반복 연산을 수행하여, brute-force 공격에 대한 방어력이 높음.

✅ 4) 느린 연산 속도
- 보안 강화를 위해 해싱 속도가 빠르지 않음.
- 해커가 수많은 비밀번호를 대입하는 무차별 대입 공격(Brute Force Attack)을 어렵게 만듦.

#### 2. BCrypt 해싱 구조
BCrypt 해시 값의 형식은 다음과 같다 

```
$2a$10$K1qLkjF9VQCd8N95fB6NUu8d87dBD23S3WxL6y/4EXR1MiPx8FlpG
```

1. $2a$ → 알고리즘 버전 ($2a$, $2b$, $2y$ 등)
2. 10$ → Cost Factor (2^10 = 1024번 해싱 반복)
3. K1qLkjF9VQCd8N95fB6NUu8d87dBD23S3WxL6y/4EXR1MiPx8FlpG → 솔트 + 해시된 비밀번호

</details>

-----------------------

### WAS(Web Application Server)란?
<details>
    <summary> 예비 답안 </summary>
    <br />

- WAS는 동적인 웹 애플리케이션을 실행하고 클라이언트 요청을 처리하는 서버 소프트웨어입니다.
- HTML, CSS, JS 같은 정적인 리소스를 처리하는 웹 서버와는 달리, Java, PHP, Python 같은 서버 사이드 언어로 작성된 로직을 실행하여 결과를 생성하고 응답합니다.

✅ WAS의 주요 역할
1.	클라이언트 요청 처리
- HTTP 요청을 받아 비즈니스 로직을 실행하고, DB 연동을 포함한 처리를 거쳐 HTML, JSON 등으로 응답을 생성합니다.
2.	비즈니스 로직 실행
- JSP, Servlet, Spring 등의 코드를 실행합니다.
3.	DB 연동 및 트랜잭션 관리
- JDBC나 ORM을 통해 DB와 통신하고 트랜잭션을 관리합니다.
4.	세션 및 스레드 관리
- 사용자의 상태(Session) 유지 및 멀티스레딩 기반 요청 처리.
5.	보안, 로깅, 예외 처리 등 다양한 미들웨어 기능 제공

- WAS는 보통 Servlet Container 역할을 하며, Servlet Lifecycle을 관리합니다.
- Spring Boot는 내장 Tomcat을 사용하여 애플리케이션과 WAS를 함께 배포할 수 있습니다.
- WAS가 죽었을 때를 대비해 로드밸런서와 여러 인스턴스를 구성하거나, 무중단 배포 전략(Blue-Green, Canary)을 사용합니다.

</details>

-----------------------

### Web Server란?
<details>
    <summary> 예비 답안 </summary>
    <br />

- 웹 서버(Web Server) 는 클라이언트(보통 웹 브라우저)로부터의 HTTP 요청을 받아, 정적인 리소스(HTML, CSS, JavaScript, 이미지 등)를 응답하는 서버입니다.
- 필요에 따라 동적 요청은 WAS로 전달(proxy)하여 협업합니다.

✅ 주요 역할
1.	정적 콘텐츠 제공
- 웹 페이지, 이미지, JS, CSS 파일 등을 빠르게 응답
2.	요청 라우팅
- URL 또는 경로에 따라 특정 파일이나 백엔드(WAS)로 요청을 전달
3.	리버스 프록시 역할
- WAS 앞단에서 요청을 필터링하고, 보안/부하 분산 기능을 수행
4.	보안 기능 제공
- HTTPS, 인증, 접근 제한 등
5.	로드 밸런싱
- 여러 WAS 인스턴스에 트래픽을 분산

> 예를 들어, Nginx + Spring Boot (내장 Tomcat) 조합에서는 정적 자원은 Nginx가 직접 응답하고,
/api/** 같은 REST 요청은 Nginx가 리버스 프록시 방식으로 WAS에 전달합니다.

> Web Server는 단순히 파일을 전달하는 역할을 넘어서, 리버스 프록시와 보안, 로드밸런싱까지 포함한 프론트 게이트 역할을 수행합니다. 보통 WAS와 함께 연동되어 웹 애플리케이션의 성능과 확장성을 높이기 위한 핵심 구성 요소로 사용됩니다.
</details>

-----------------------

### 톰캣이란 
<details>
    <summary> 예비 답안 </summary>
    <br />

- 아파치 톰캣은 자바 기반의 웹 애플리케이션 서버(WAS) 또는 서블릿 컨테이너입니다.
- 톰캣은 HTTP 요청을 받아 자바 서블릿 및 JSP 를 실행하여 동적인 웹 페이지를 생성하고 반환하는 역할을 합니다.

</details>

-----------------------
