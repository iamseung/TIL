## REST

→ `자원을 이름으로 구분`하여 `해당 자원의 상태를 주고 받는 모든 것을 의미`

### REST 의 구성 요소

1. 자원(Resource) → `HTTP URI`
2. 자원에 대한 행위(Verb) → `HTTP Method`
3. 자원에 대한 행위의 내용(Representations) → `HTTP Message Payload`

### URI vs URL

`URI` → Uniform Resource Identifier

    통합 자원 식별자
    인터넷 상의 리소스 자원 자체를 식별하는 고유한 문자열 시퀀스
    ex) google.com

`URL` → Uniform Resource Locator

    네트워크 상에서 통합 자원의 “**위치**”를 나타내기 위한 규약
    웹사이트 주소 + 컴퓨터 네트워크 상의 자원
    ex) https://www.google.com (URI + 프로토콜)

### REST 의 특징

    Server-Client (서버-클라이언트 구조) → 클라이언트와 서버는 서로 독립적이며, 클라이언트는 오직 URI 만 알아야 한다.
    Stateless → 클라이언트의 요청에는 해당 요청을 이해할 수 있는 모든 정보가 포함되어 있어야 한다
    Cacheable → 클라이언트는 API 요청을 캐싱하고 있어 동일한 요청이 발생하는 경우에는 서버로 요청을 발생시키지 않음

## Restful

→ REST 의 특징을 준수하는 인터페이스, RESTful 은 REST 아키텍처를 통해 설계되었을 뿐만 아니라 그 원칙을 모두 충실히 따르는 API 를 의미.

멱등성 보장 X → `POST`, `PATCH`
<br>
(멱등성 → 여러 번 동일한 요청을 보냈을 때, 영향이 동일한가)

`PUT` → 리소스가 있을 경우 대체, 없을 경우 생성
<br>
`PATCH` → 리소스 부분 변경