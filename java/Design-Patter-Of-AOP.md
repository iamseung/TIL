# ✅ AOP와 관련 깊은 디자인 패턴 3가지

1. 프록시 패턴
2. 데코레이터 패턴
3. 체인 오브 리스폰서빌리티 패턴

## 1. 프록시 패턴 (Proxy Pattern) – AOP의 핵심 기반
- 정의 : 어떤 객체에 대한 접근을 제어하기 위해 대리 객체를 사용하는 패턴
- AOP와의 연관성
    - AOP는 프록시 객체를 사용하여 실제 메서드 호출 전후에 부가 기능(로깅, 트랜잭션, 보안 등)을 삽입
        - → Spring AOP는 JDK Dynamic Proxy 또는 CGLIB Proxy로 이를 구현함

```java
public class ServiceProxy implements Service {
    private Service target;

    public void doSomething() {
        System.out.println("Before"); // 부가 기능
        target.doSomething();        // 핵심 기능
        System.out.println("After"); // 부가 기능
    }
}
```

# 2. 데코레이터 패턴 (Decorator Pattern)
- 정의 : 기존 객체에 새로운 기능을 추가할 때 사용하는 패턴 (상속 대신 위임)
- AOP와의 연관성
    - 데코레이터처럼 핵심 기능에 영향을 주지 않으면서 부가 기능을 덧붙이는 구조
    - 단, AOP는 더 선언적이고 프레임워크 기반으로 적용됨

### ex) 텍스트 출력 기능에 꾸미기 기능을 데코레이팅
```java
// 컴포넌트 인터페이스
public interface Notifier {
    void send(String message);
}

// 기본 구현체
public class BaseNotifier implements Notifier {
    public void send(String message) {
        System.out.println("기본 알림: " + message);
    }
}

// 데코레이터 추상 클래스
public abstract class NotifierDecorator implements Notifier {
    protected Notifier delegate;

    public NotifierDecorator(Notifier delegate) {
        this.delegate = delegate;
    }

    public void send(String message) {
        delegate.send(message);
    }
}

// 이메일 알림 추가
public class EmailNotifier extends NotifierDecorator {
    public EmailNotifier(Notifier delegate) {
        super(delegate);
    }

    public void send(String message) {
        super.send(message);
        System.out.println("이메일 전송: " + message);
    }
}

// 카카오 알림 추가
public class KakaoNotifier extends NotifierDecorator {
    public KakaoNotifier(Notifier delegate) {
        super(delegate);
    }

    public void send(String message) {
        super.send(message);
        System.out.println("카카오톡 전송: " + message);
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Notifier notifier = new KakaoNotifier(
                                new EmailNotifier(
                                    new BaseNotifier()));
        notifier.send("주문이 완료되었습니다.");
    }
}

..
..

기본 알림: 주문이 완료되었습니다.
이메일 전송: 주문이 완료되었습니다.
카카오톡 전송: 주문이 완료되었습니다.
```

# 3. 체인 오브 리스폰서빌리티 패턴 (Chain of Responsibility Pattern)
- 정의 : 요청을 처리하는 객체를 체인 형태로 연결하고, 그 중 하나가 요청을 처리하도록 위임
- AOP와의 연관성
    - 여러 Advice들이 순차적으로 실행되는 AOP의 동작 방식이 이 패턴과 유사
        - (예: @Around → @Before → target → @After → @AfterReturning)


### ex) 요청에 따라 책임을 넘겨 처리

```java
// 요청 클래스
public class Request {
    public String content;
    public Request(String content) { this.content = content; }
}

// 핸들러 인터페이스
public abstract class Handler {
    protected Handler next;

    public Handler setNext(Handler next) {
        this.next = next;
        return next;
    }

    public void handle(Request request) {
        if (next != null) {
            next.handle(request);
        }
    }
}

// 각 책임자 구현
public class AuthHandler extends Handler {
    public void handle(Request request) {
        if (request.content.contains("auth")) {
            System.out.println("Auth 처리됨");
        } else {
            super.handle(request);
        }
    }
}

public class LogHandler extends Handler {
    public void handle(Request request) {
        if (request.content.contains("log")) {
            System.out.println("Log 처리됨");
        } else {
            super.handle(request);
        }
    }
}

public class DataHandler extends Handler {
    public void handle(Request request) {
        if (request.content.contains("data")) {
            System.out.println("Data 처리됨");
        } else {
            super.handle(request);
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args) {
        Handler chain = new AuthHandler();
        chain.setNext(new LogHandler())
             .setNext(new DataHandler());

        Request r1 = new Request("auth 요청입니다.");
        Request r2 = new Request("log 요청입니다.");
        Request r3 = new Request("data 요청입니다.");
        Request r4 = new Request("기타 요청입니다.");

        chain.handle(r1);
        chain.handle(r2);
        chain.handle(r3);
        chain.handle(r4);
    }
}

..
..

Auth 처리됨
Log 처리됨
Data 처리됨
```


| 디자인 패턴 | AOP에서의 역할/연관 | 
| -------- | --------------- |
| Proxy | 핵심 구조 기반. 실제 메서드 호출 전후에 Advice 삽입 |
| Decorator | 핵심 기능에 영향을 주지 않고 부가 기능을 덧붙임 |
| Chain of Responsibility | 여러 Advice가 순차적으로 실행되는 흐름과 유사 |
