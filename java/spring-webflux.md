# Spring WebFlux 자세히 이해하기

`Spring WebFlux`는 `비동기(Asynchronous)`, `논블로킹(Non-blocking)` 프로그래밍을 지원하는 리액티브 웹 프레임워크입니다. 기존 Spring MVC는 블로킹 방식(Synchronous, Thread-per-request) 을 기반으로 하지만, WebFlux는 리액티브 스트림(Reactive Streams)과 프로젝트 리액터(Project Reactor)를 기반으로 한 논블로킹 방식을 제공합니다.

📖 즉, Spring Webflux 는 `Reactive Streams` 표준을 기반으로 동작하며, 내부적으로 `Project Reactor` 라이브러리를 사용하여 비동기 & Non-blocking 방식의 프로그래밍을 지원하며, 이를 통해 스레드 풀의 효율적인 사용과 고성능 처리가 가능합니다.

이제 WebFlux의 개념, 특징, 작동 방식, 장점과 단점을 깊이 있게 알아보겠습니다.

## 1. Spring WebFlux란?
Spring WebFlux는 Spring 5부터 도입된 리액티브 프로그래밍 기반의 웹 프레임워크입니다. 기본적으로 Spring MVC와 유사한 방식으로 웹 애플리케이션을 개발할 수 있지만, 내부적으로 논블로킹 방식의 리액티브 스트림을 활용하여 성능을 극대화합니다.

Spring WebFlux는 다음 두 가지 방식으로 구현될 수 있습니다.

1. 어노테이션 기반(Annotation-based) 방식 → 기존의 @Controller, @RequestMapping을 활용 (Spring MVC와 유사한 방식)
2.  펑셔널(Functional) 방식 → RouterFunction과 HandlerFunction을 활용 (더 가볍고 유연한 방식)

## 2. Spring WebFlux vs Spring MVC 차이점
||Spring WebFlux | Spring MVC| 
|-|---|---|
|처리 방식|	비동기(Asynchronous), 논블로킹(Non-blocking) | 동기(Synchronous), 블로킹(Blocking)|
|스레드 모델|	이벤트 루프(Event Loop) 기반|	하나의 요청당 하나의 스레드(Thread-per-request)|
|사용하는 API|	Reactor (Mono, Flux)|	Servlet API (HttpServletRequest, HttpServletResponse)|
|서버 타입|	Netty, Undertow, Servlet 컨테이너 지원|	Tomcat, Jetty (Servlet 기반)|
|적합한 환경|	대규모 트래픽, 실시간 데이터 처리, 고성능 API|	전통적인 웹 애플리케이션, 적은 요청 처리|

### 이벤트 루프
- 이벤트 루프는 few thread에서 계속 동작합니다.
- 이벤트 루프는 이벤트 큐로부터 이벤트들을 순차적으로 처리하고 알맞은 콜백 함수를 실행합니다.
- 외부 서비스 호출이나 데이터베이스 호출과 같은 동작의 완료에 의해 트리거가 될 수 있습니다.
- 이벤트 루프는 IO작업의 완료에 대한 콜백을 발생하고 그 결과를 완래 호출자에게 결과를 보낼 수 있습니다.

### ✅ 비동기 논블로킹의 장점
- 대규모 트래픽을 효율적으로 처리 가능 (적은 스레드로 많은 요청 처리 가능)
- IO 작업이 많은 시스템에서 성능 향상 (DB, 외부 API 호출 등)
- CPU 사용량 최적화 (비동기 이벤트 루프 활용)

### ❌ 단점
- 개발 난이도가 높음 (리액티브 프로그래밍 개념을 익혀야 함)
- 디버깅이 어려움 (비동기 실행 흐름이 복잡해짐)
- JPA 같은 기존 블로킹 라이브러리와 함께 사용하기 어려움 (대신 R2DBC, MongoDB Driver 등을 사용)

## 3. Spring WebFlux의 핵심 개념
### (1) 리액티브 스트림 (Reactive Streams)
Spring WebFlux는 리액티브 스트림(Reactive Streams) 표준을 준수하는데, 이는 데이터 스트림을 처리하는 논블로킹 방식의 표준입니다.
주요 구성 요소는 다음과 같습니다.

- Publisher: 데이터를 생성하는 주체 (예: Flux, Mono)
- Subscriber: 데이터를 소비하는 주체
- Subscription: Publisher와 Subscriber 사이의 연결
- Processor: Publisher와 Subscriber 사이에서 데이터 변환 및 처리
### (2) Reactor 라이브러리 (Mono와 Flux)
Spring WebFlux에서는 Reactor 라이브러리의 Mono와 Flux를 활용하여 비동기 데이터를 처리합니다.

#### ✅ Mono<T> (0~1개의 데이터 처리)
```java
Mono<String> mono = Mono.just("Hello WebFlux");
mono.subscribe(System.out::println);
```
→ 하나의 데이터를 포함하는 Mono는 비동기적으로 단일 값을 반환합니다.

####  ✅ Flux<T> (0~N개의 데이터 처리)
```java

Flux<String> flux = Flux.just("A", "B", "C");
flux.subscribe(System.out::println);
→ 여러 개의 데이터를 포함하는 Flux는 비동기적으로 여러 개의 값을 반환합니다.
```

### (3) Netty 기반의 Event Loop(기본 설정)
- Tomcat, Jetty, Undertow 등과 다르게 기본적으로 Netty 를 사용하여 Event Loop 기반의 Non-Blocking I/O 처리

## 4. Spring WebFlux의 주요 기능
### (1) 어노테이션 기반 (Spring MVC 스타일)
기존 Spring MVC와 유사하게 @RestController, @RequestMapping을 사용할 수 있습니다.

```java
@RestController
@RequestMapping("/users")
public class UserController {

    @GetMapping("/{id}")
    public Mono<User> getUser(@PathVariable String id) {
        return userService.findById(id);
    }

    @GetMapping
    public Flux<User> getAllUsers() {
        return userService.findAll();
    }
}
```

### (2) 함수형 라우팅 (Functional Style)
WebFlux는 더 가볍고 유연한 방식인 함수형 라우팅을 지원합니다.

```java
@Configuration
public class RouterConfig {

    @Bean
    public RouterFunction<ServerResponse> routes(UserHandler handler) {
        return RouterFunctions.route(GET("/users/{id}"), handler::getUser)
                             .andRoute(GET("/users"), handler::getAllUsers);
    }
}
이 방식은 @RestController를 사용하지 않고, 요청을 HandlerFunction으로 매핑하는 방식입니다.
```

## 5. Spring WebFlux와 데이터베이스
WebFlux는 기존의 블로킹 방식의 JPA/Hibernate 대신 R2DBC (Reactive Relational Database Connectivity) 를 사용하여 비동기 방식으로 관계형 DB에 접근할 수 있습니다.

### ✅ R2DBC를 활용한 비동기 DB 연동

```java
@Repository
public interface UserRepository extends ReactiveCrudRepository<User, String> {
}
```
→ ReactiveCrudRepository를 사용하면, WebFlux의 Mono와 Flux를 반환하는 비동기 방식으로 데이터를 처리할 수 있습니다.

## 6. Spring WebFlux가 적합한 경우
### ✅ 사용하기 좋은 환경
- 대규모 트래픽을 처리해야 하는 API 서버
    - 예: SNS 서비스, 검색 엔진 API, 실시간 알림 서비스
- IO 작업이 많은 환경   
    - 예: 비동기적으로 DB, 외부 API를 호출해야 하는 경우
- 스트리밍 서비스
    - 예: 실시간 데이터 스트리밍, WebSocket 기반의 채팅 서비스
- 마이크로서비스 아키텍처 (MSA)
    - WebFlux는 경량 프레임워크이므로, MSA 환경에서 가볍게 사용할 수 있음

### ❌ 사용하기 어려운 환경
- 전통적인 MVC 기반의 웹 애플리케이션
- 서버 렌더링을 많이 사용하는 프로젝트에서는 MVC가 더 적합
- JPA와 같이 블로킹 라이브러리를 많이 사용하는 경우
- WebFlux는 블로킹 라이브러리와 함께 사용하기 어렵기 때문에, 기존 프로젝트를 WebFlux로 전환하려면 많은 수정이 필요

## 7. Spring WebFlux 의 동작 원리

### (1) 요청 처리 흐름
HTTP 요청이 들어오면 WebFlux는 다음과 같은 방식으로 요청을 처리한다.

1. 클라이언트가 HTTP 요청을 보냄.
2. Netty Event Loop(Thread) 가 요청을 비동기적으로 수신.
3. 요청이 Dispatcher Handler로 전달됨.
4. RouterFunction 또는 @RestController 기반의 Handler Function이 실행됨.
5. Handler가 Mono 또는 Flux를 반환하면, Reactor가 이를 구독(subscribe)하여 처리.
6. 데이터가 준비되면 Netty Event Loop Thread가 응답을 비동기적으로 클라이언트에 반환.

### (2) Non-blocking의 핵심: Netty 기반 Event Loop
- Spring WebFlux는 Netty의 Event Loop 모델을 사용하여 적은 수의 스레드로 많은 요청을 처리.
- 기본적으로 EventLoopGroup(스레드 그룹) 내의 스레드가 Non-blocking 방식으로 요청을 처리.
- 데이터베이스 호출, API 호출 같은 I/O 작업도 Reactor의 Schedulers를 사용하여 비동기 실행.


## 8. 결론
Spring WebFlux는 비동기, 논블로킹 기반의 고성능 웹 애플리케이션을 개발할 수 있도록 도와주는 프레임워크입니다.

- Mono와 Flux를 사용하여 비동기 방식으로 데이터를 처리
- 기존 Spring MVC와 달리 이벤트 루프 기반의 논블로킹 모델을 사용하여 성능 최적화
- R2DBC와 같은 비동기 DB 접근 방식을 활용할 수 있음
- 실시간 데이터 스트리밍, 마이크로서비스 아키텍처 등에 적합
- 하지만, 기존의 블로킹 기반 시스템과 함께 사용하기 어려운 단점이 있으며, 리액티브 프로그래밍을 익히는 데 학습 곡선이 가파를 수 있음.

> 👉 WebFlux를 선택하기 전에, 프로젝트의 요구 사항과 적합성을 신중히 고려해야 함. 🚀

# 비동기 (Asynchronous) 와 논블로킹 (Non-blocking)

## 1. 비동기(Asynchronous)란?
비동기 프로그래밍이란, 작업이 끝날 때까지 기다리지 않고 다른 작업을 수행할 수 있는 방식을 의미합니다.

### ✅ 비동기의 특징
- 하나의 작업이 완료될 때까지 기다리지 않고 다른 작업을 병렬로 실행 가능
- 콜백(Callback) 또는 Future, Promise, CompletableFuture 같은 메커니즘을 통해 응답을 받을 수 있음
- 자원을 효율적으로 사용하여 높은 동시성을 지원할 수 있음

### ✅ 비동기 동작 방식
- 작업을 요청함 (예: 데이터베이스 조회, API 호출)
- 즉시 다음 코드 실행을 진행 (응답을 기다리지 않음)
- 작업이 완료되면 콜백(Callback) 또는 Future를 통해 결과를 전달받음

### ✅ 비동기 코드 예제 (Java CompletableFuture)

```java
import java.util.concurrent.CompletableFuture;

public class AsyncExample {
    public static void main(String[] args) {
        System.out.println("작업 시작");

        CompletableFuture.supplyAsync(() -> {
            try { 
                Thread.sleep(2000); 
            } catch (InterruptedException e) {

            }

            return "비동기 작업 완료!";
        }).thenAccept(result -> System.out.println(result));

        System.out.println("다른 작업 수행 중...");
    }
}
```

```java
[실행 결과]
작업 시작
다른 작업 수행 중...
(2초 후)
비동기 작업 완료!
```

> ✅ 비동기 코드에서는 메인 스레드가 API 요청을 기다리지 않고 다른 작업을 먼저 수행한 후, 결과를 받는다.

## 2. 논블로킹(Non-blocking)란?
논블로킹 프로그래밍이란, 작업이 진행되는 동안 현재 실행 중인 스레드가 멈추지 않고 계속해서 다른 작업을 수행할 수 있는 방식입니다.

### ✅ 논블로킹의 특징
- 작업이 완료될 때까지 기다리지 않고 바로 다음 코드 실행
- CPU와 스레드를 효율적으로 사용하여 높은 성능을 제공
- 주로 이벤트 루프(Event Loop) 기반으로 동작

### ✅ 논블로킹 동작 방식
- 특정 작업을 요청하면 해당 요청이 바로 반환됨 (즉시 응답)
- 실제 작업이 완료될 때까지 현재 실행 중인 스레드는 멈추지 않고 다른 작업을 수행함
- 작업이 완료되면 결과를 비동기적으로 처리

## 3. 비동기 vs 논블로킹 차이점
| |비동기(Asynchronous)|논블로킹(Non-blocking)|
|-|----|----|
|개념|	요청을 보낸 후 기다리지 않고 다른 작업을 수행 가능|	실행 중인 스레드가 특정 작업을 기다리며 멈추지 않음|
|동작 방식|	응답을 기다리는 동안 다른 작업을 수행|	작업이 완료될 때까지 현재 실행 중인 스레드가 멈추지 않음|
|예제|	CompletableFuture, Future, Callback	| NIO (Non-blocking I/O), Reactor, WebFlux|
|장점|	높은 동시성, 빠른 응답 가능	| 스레드 자원을 효율적으로 사용 가능|
|단점|	콜백 지옥(Callback Hell), 디버깅 어려움|	기존 블로킹 방식의 코드와 혼용 시 복잡해질 수 있음|

## 🔹 한마디로 정리
- 비동기(Asynchronous) = 요청을 보내고 기다리지 않음
- 논블로킹(Non-blocking) = 특정 작업을 기다리지 않고 다른 작업 수행 가능

✅ 비동기이면서 논블로킹일 수도 있지만,
✅ 비동기이지만 블로킹일 수도 있고,
✅ 동기이면서 논블로킹일 수도 있음.

## 4. 비동기 & 논블로킹이 WebFlux에서 어떻게 사용될까?

Spring WebFlux는 비동기(Asynchronous) + 논블로킹(Non-blocking) 모델을 함께 사용하여 성능을 최적화합니다.

### Spring MVC (블로킹, 동기)

Spring MVC는 Tomcat 같은 블로킹 I/O(Blocking I/O) 모델을 사용합니다.

```java
@GetMapping("/blocking")
public String getData() {
    return slowDatabaseQuery();  // 동기적으로 실행 (스레드가 멈춤)
}
```

#### ⛔ 이 방식의 문제점?

하나의 요청이 실행되는 동안 스레드가 대기(blocking)하고 있음 → 성능 저하 발생
Spring WebFlux (비동기, 논블로킹)
WebFlux는 Reactor(Flux, Mono) + Netty 같은 논블로킹 서버를 사용합니다.

```java
@GetMapping("/non-blocking")
public Mono<String> getData() {
    return asyncDatabaseQuery();  // 비동기적으로 실행
}
```

### ✅ 장점?

- 하나의 스레드가 여러 요청을 처리할 수 있음 → 고성능 처리 가능
- Mono와 Flux를 사용하여 비동기 방식으로 데이터를 처리

## 5. WebFlux에서 비동기 & 논블로킹 적용

### ✅ Spring MVC (블로킹)
```java
@RestController
public class BlockingController {
    @GetMapping("/blocking")
    public String blocking() {
        try { Thread.sleep(2000); } catch (InterruptedException e) {}
        return "동기 블로킹 응답 완료";
    }
}
```

#### 🔹 동작 방식
- 요청이 들어오면 해당 스레드가 2초 동안 멈춤 (Blocking)
- 새로운 요청이 오면 새로운 스레드가 필요함 → 동시 요청이 많으면 성능 저하 발생

### ✅ Spring WebFlux (비동기 & 논블로킹)

```java
@RestController
public class NonBlockingController {
    @GetMapping("/non-blocking")
    public Mono<String> nonBlocking() {
        return Mono.fromSupplier(() -> {
            try { Thread.sleep(2000); } catch (InterruptedException e) {}
            return "비동기 논블로킹 응답 완료";
        });
    }
}
```

#### 🔹 동작 방식
- Mono.fromSupplier()를 사용하여 비동기적으로 데이터를 반환
- 요청이 들어와도 스레드는 멈추지 않고 다른 요청을 처리 가능
- 같은 스레드로 여러 요청을 처리 가능 → 고성능 처리 가능

## 6. 결론
Spring WebFlux에서 비동기 & 논블로킹을 활용하면,
- 스레드를 효율적으로 사용하여 높은 성능과 동시성을 제공
- 서버 리소스를 최적화하여 대규모 트래픽을 처리 가능
- 블로킹 방식보다 빠른 응답 시간 제공

하지만,
- 리액티브 프로그래밍이 복잡하여 학습 곡선이 가파름
- 기존 블로킹 API와 함께 사용할 때 변환 과정이 필요

> 👉 WebFlux를 사용할 때는 블로킹 API 사용을 최소화하고, 리액티브 방식으로 처리하는 것이 중요!
