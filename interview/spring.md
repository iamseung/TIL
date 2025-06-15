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

### 스프링의 의존 자동 주입 방법

<details>
    <summary> 예비 답안 </summary>
    <br />

- @Autowired - 필드, 생성자, 세터 메서드에 해당 어노테이션을 붙여주면 스프링은 타입이 일치하는 빈 객체를 찾아서 주입을 해준다.
- 생성자 주입
    - 호출 시점에 1회 호출된다는 보장이 있다. 주입을 받는 객체들이 변하지 않는다는 보장이 되고 필드에 final을 붙일 수 있다
    - 생성자가 1개만 있을 경우 @Autowired 어노테이션이 생략 가능하다.
- Setter주입
    - Setter 주입 방법은 주로 주입 받는 객체가 변경될 가능성이 있는 경우에 사용을 한다.
    - 개발자가 실수로 의존 객체를 올바르게 주입하지 않을 경우, 사용 시점에 NullPointerException이 발생할 수 있다는 단점이 있다.
    - Setter를 열어둬야 한다.
- 필드 주입
    - 필드에 @Autowired를 붙여 바로 의존 관계를 주입하는 방법이다.
    - 필드 주입 방법은 코드가 간결해져 과거에는 많이 사용하였지만 외부에서 접근이 불가능하여 테스트의 어려움이 있다.
    - 스프링과 같은 DI를 제공하는 프레임워크에서만 동작하여 프레임워크의 변경이 있을 시 많은 문제를 초래할 수 있다.
> 📌 필드 주입은 빈의 생성자가 실행된 바로 직후에 실행이 되게 된다.
    
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

### @Controller, @RestController 차이

<details>
    <summary> 예비 답안 </summary>
    <br />

- @Controller 는 기본 반환 방식이 View 이름(String) 이며, HTML 페이지 반환 등 템플릿 기반 응답에 사용됩니다.

```java
@Controller
public class PageController {

    @GetMapping("/hello")
    public String hello(Model model) {
        model.addAttribute("message", "Hello!");
        return "hello";  // templates/hello.html 렌더링
    }
}
```

- @RestController 는 기본 반환 방식이 JSON, XML(객체 직렬화) 이며, REST API 응답에 사용됩니다.(주로 JSON 반환)

```java
@RestController
public class ApiController {

    @GetMapping("/api/hello")
    public Map<String, String> hello() {
        return Map.of("message", "Hello!");
        // JSON: { "message": "Hello!" }
    }
}
```
- @Controller + @ResponseBody 의 조합
- 반환값을 HTTP 응답 본문(body) 에 바로 JSON/XML 등으로 전송
- RESTful API 개발에 최적화
    
</details>

-----------------------

### Spring 의 3대 요소

<details>
    <summary> 예비 답안 </summary>
    <br />

- AOP, DI, PSA Spring이 위의 3가지 요소를 통해 어떤 것을 추구할까?
    - POJO를 통해 비즈니스 로직에 집중할 수 있도록 하는 것이다.

> `PSA (Portable Service Abstraction)`
> - 추상화 계층을 사용하여 어떤 기술을 내부에 숨기고 개발자에게 편의성을 제공해주는 것이 서비스 추상화(Service Abstraction)
    
</details>

-----------------------

### DI, IoC는 무엇일까요?

<details>
    <summary> 예비 답안 </summary>
    <br />

- DI(dependency injection, 의존성 주입)는 외부에서 의존 관계를 주입해 결합도를 낮추고 유연성을 높여주는 기술이다.
- 스프링의 DI는 스프링 컨테이너에 필요한 객체(Bean)들을 싱클턴으로 생성하고 생성한 객체에 의존을 주입하며 제공한다.
- DI는 스프링에서 IoC(Inversion of Control)를 구현하는데 사용하는 패턴이다.
- IoC는 객체 또는 프로그램의 제어 권한을 프레임워크에 넘기는 기술이다. 스프링에서 빈을 생성, 소멸의 작업들을 수행하고 의존 주입의 대상까지 스프링이 해주는데 이것이 바로 스프링의 IoC이다.

#### IoC의 장점
- 유연성 증가 → 다른 구현체로 변경하기 쉽다.
- 객체간 결합도 감소 → 프로그램을 모듈들로 나누기 쉽다.
- 작업의 실행과 구현을 분리할 수 있다.
- 컴포넌트를 격리하거나 의존성을 mocking하는 등의 작업을 통해 테스트를 하기 쉽다.
    
</details>

-----------------------

### AOP란 ?

<details>
    <summary> 예비 답안 </summary>
    <br />

- AOP(Aspect Oriented Programming)은 관점 지향 프로그래밍으로 로직을 기준으로 핵심적인 관점, 부가적인 관점으로 나누어 보고 그 관점을 기준으로 모듈화하는 기술을 의미한다.
- 흩어진 관심사를 Aspect로 모듈화하고 핵심적인 비즈니스 로직에서 분리하여 재사용하겠다는 것이 AOP의 취지이다.

#### 스프링 AOP의 특징

- 프록시 패턴 기반의 AOP 구현체, 프록시 객체를 쓰는 이유는 접근 제어 및 부가기능을 추가하기 위해서임
- 스프링 빈에만 AOP를 적용 가능
- 모든 AOP 기능을 제공하는 것이 아닌 스프링 IoC와 연동하여 엔터프라이즈 애플리케이션에서 가장 흔한 문제(중복코드, 프록시 클래스 작성의 번거로움, 객체들 간 관계 복잡도 증가 ...)에 대한 해결책을 지원하는 것이 목적

#### 대표적인 예시
- 스프링의 Transaction
    
</details>

-----------------------