## Topic 1: 프로젝트 플로우와 아키텍처

프로젝트의 기능, 흐름 및 아키텍처에 대해 문의합니다. 또한 기술 스택과 운영 환경에 어떻게 배포되는지, 지원자가 기여한 부분에 대해서도 질문합니다.

### [질문]

- 진행한 프로젝트와 아키텍처에 대해서 알려주세요.

- 그림으로 아키텍처, 프레임워크, 사용한 기술에 대해 설명해주세요.


프로젝트에 대해 아는 모든 것을 어딘가에 적어두세요. 당신만이 프로젝트에 대해 모든 것을 알고 있다는 것을 기억하고, 자신감을 가지세요.


## Topic 2: 코어 자바

코어 자바는 방대한 주제이며 면접관은 이러한 주제를 반드시 물어봅니다.

코어 자바는 자바 개발자에게 기본적인 것으로 여겨지므로 이 부분에 대해 철저한 답을 공부하세요. 특정한 프레임워크를 모르는 것이 문제가 되지는 않지만, 코어 자바에 대한 지식이 부족하면 문제가 될 수 있습니다.

 
### [토픽]

- String/Hashcode-Equal 메소드

- Immutability

- OOPS 개념

- Serialization

- Collection Framework/concurrent collection

- 예외 처리

- 멀티스레드/스레드풀

- 자바 메모리(메모리 각 영역에 객체, 메소드 및 변수를 저장하는 법)

- 가비지 컬렉션(가비지가 객체를 수집하는 방법, 사용하는 알고리즘)

### [질문]

- ThreadPoolExecutor는 어떻게 동작하나요?

- 커스텀 불변 클래스를 어떻게 만드나요? 자바에서 불변 클래스의 예는 무엇인가요?

- hasCode()와 equals()가 무엇인가요? map에서 객체를 키로 사용하면 어떻게 되마요? 올바르게 사용하는 방법은 무엇인가요?

- 깊은 복사와 얕은 복사가 무엇인가요?

- CompletableFuture가 무엇인가요?

- 최신 자바 메모리 모델이 무엇인가요?

- concurrent collection이 무엇인가요?

- HashMap, ArrayList 및 LinkedList의 시간/공간 복잡도를 말해주세요

- Arrays.sort()와 Collections.sort()에서 사용되는 알고리즘은 무엇인가요?

- 자바에서 커스텀 어노테이션을 어떻게 만드나요?

- HashMap과 HashSet은 내부적으로 어떻게 작동되나요?

- String의 join() 메소드의 용도는 무엇인가요?

## Topic 3: 자바 8/자바 11/자바 17

새로 추가된 자바 API와 관련된 기능들을 알아야 합니다. 

### [토픽]

- 자바8 기능

- default/static 메소드

- 람다 표현식

- functional interface

- optional API

- stream API

- 패턴 매칭

- text block

- 모듈

### [질문]

- 자바 8/자바11/자바17의 새로운 기능은 무엇인가요?

- 자바에서 병렬 스트림이란 무엇이며 어떻게 작동하나요?

- 자바 메모리 모델의 새로운 개선점이 무엇인가요? 자바8 hashmap의 개선점은 무엇인가요?

 
### Topic 4: 스프링 프레임워크, 스프링 부트, 마이크로서비스, REST API

기본적인 반복 질문을 공부해야 합니다. 이 주제에 대해 면접관을 만족시키지 못하면 탈락할 수 있습니다.

### [토픽]

- 의존성 주입/IOC, 스프링 MVC

- configuration, 어노테이션, CRUD 

- Bean, Scope, Profiles, Bean 라이프사이클

- App context/Bean context

- AOP, Exception Handler, Control Advice

- Security(JWT, Oauth)

- Actuators

- 웹플럭스와 Mono Framework

- HTTP method

- Microservice 개념

- Spring Cloud

- JPA

### [질문]

- 이 어노테이션의 용도는 무엇인가요? - @RequestMapping @RestController @Service @Repository @Entity

- Actuator가 무엇이고 어디에 쓰이나요?

- 애플리케이션의 복원력을 높이는 방법은 무엇인가요?

- distributed tracing이 무엇인가요? traceId와 spanId는 무엇인가요?

- 스프링 부트에서 WebFlux 및 Mono Framework란 무엇인가요?

- 스프링이 주기적으로 의존하는 것은 무엇이며, 어떻게 예방하나요?

- REST API를 보호하는 방법은?

- 스프링 부트에서 auto-configuration을 비활성화하는 방법은 무엇인가요?