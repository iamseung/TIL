## Filter 와 Interceptor

### Filter
`Filter` 는 HTTP 요청을 가로채어, 특정 작업을 수행할 수 있도록 하는 컴포넌트입니다. 주로 보안/로깅/데이터 처리/요청 수정 등 다양한 작업을 처리할 수 있습니다. Spring Filter 는 Java Servlet API 의 javax.servlet.Filter 인터페이스를 구현하며, Spring Boot 환경에서는 이를 더욱 간편하게 활용할 수 있습니다.

### Intercepter
`Intercepter` 는 Spring 의 HandlerIntercepter 인터페이스를 구현하여 요청 전/후 및 완료 단계에서 처리 로직을 삽입할 수 있다.

### Filter vs Interceptor

특징|Filter|Intercepter
|---|---|----|
위치     | 서블릿 컨테이너 레벨에서 동작 | Spring 의 HandlerMapping 레벨에서 동작
적용 범위 | 모든 요청 및 응답 처리 가능 | 주로 Spring MVC 의 컨트롤러 요청/응답에 처리
기술 스택 | Servlet API 기반 | Spring AOP 기반
용도     | 요청/응답 가로채기,로깅, 보안 처리 등 | 컨트롤러 전/후 처리, 비즈니스 로직 전/후 처리

### 실제 요청 흐름

클라이언트  Filter → DispatcherServlet → Integerceptor (preHandle) → Controller → Integerceptor (postHandler) → Integerceptor(afterCompletion) 응답