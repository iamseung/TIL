# NoSQL 데이터 모델 

NoSQL 데이터베이스는 크게 `Key-Value`, `Wide-Column`, `Document`, `Graph` 4가지 유형으로 분류됩니다. 각각의 데이터 모델은 특정한 사용 사례에 맞게 최적화되어 있습니다.

## 주요 차이점
| 데이터 모델 | 저장 방식 | 특징 | 대표적인 데이터베이스 | 주요 활용 사례 |
|------------|---------|------|-------------------|--------------|
| **Key-Value** | Key와 Value의 쌍으로 저장 | 단순한 구조, 빠른 읽기/쓰기 성능 | Redis, DynamoDB, Riak | 캐싱, 세션 저장소, 로그 |
| **Wide-Column** | 행(Row) 단위로 여러 개의 컬럼을 가짐 | 컬럼 패밀리 기반, 수평 확장 용이 | Cassandra, HBase, ScyllaDB | 실시간 분석, 로그 저장, IoT |
| **Document** | JSON, BSON, XML 등의 문서 형식으로 저장 | 반정형 데이터, 스키마 유연함 | MongoDB, CouchDB, Firebase | 웹 애플리케이션, 콘텐츠 관리 시스템 |
| **Graph** | 노드(Node)와 엣지(Edge)로 관계를 표현 | 관계형 데이터 모델보다 복잡한 연관 관계 표현 가능 | Neo4j, ArangoDB, Amazon Neptune | 추천 시스템, 소셜 네트워크 분석 |



## `Key-Value`

Key-Value 모델은 빠른 조회 속도와 단순한 데이터 구조가 필요한 경우에 가장 적합합니다.

1️⃣ 캐싱 시스템
- ex) Redis를 활용한 웹 페이지 캐싱
- API 요청의 결과를 캐싱하여 서버 부하를 줄이고 응답 속도를 향상.

2️⃣ 세션 관리
- ex) DynamoDB 또는 Redis를 활용한 사용자 로그인 세션 저장
- 세션 데이터를 Key-Value 형태로 저장하여, 빠르게 조회 및 갱신 가능.

3️⃣ 로그 및 이벤트 저장소
- ex) ELK Stack에서 Redis를 Buffer 역할로 활용
- 이벤트 데이터를 Key 기반으로 빠르게 저장하고 실시간 분석 가능.

### 구조
- Key와 Value의 단순한 쌍으로 구성됨.
- Value에는 문자열, JSON, 바이너리 데이터 등 다양한 형식이 저장 가능.

### 장점
- 매우 빠른 읽기/쓰기 성능.
- 단순한 데이터 저장 및 조회에 최적화됨.

### 단점
- 관계형 데이터 표현이 어려움.
- 특정 Key를 모르면 원하는 데이터를 검색하기 어려움.

### 예제 (Redis)

```
"user:1001" -> "{name: 'Alice', age: 25, city: 'Seoul'}"
"user:1002" -> "{name: 'Bob', age: 30, city: 'Busan'}"
```

## `Wide-Column`

Wide-Column 모델은 금융 거래 기록과 같은 시계열 데이터, 로그, 분석에 적합합니다.

### 구조
- Keyspace → Column Family(Table) → Row(Primary Key 기반의 데이터 저장)
- RDBMS의 테이블과 비슷하지만, 행마다 다른 컬럼을 가질 수 있음.

### 장점
- 대량의 데이터 저장에 최적화됨.
- 수평 확장이 쉬움.

### 단점
- 복잡한 쿼리 및 관계형 연산이 어렵고 JOIN이 불가능.

### 예제 (Cassandra)
```
CREATE TABLE users (
    id UUID PRIMARY KEY,
    name text,
    age int,
    city text
);
```

```
| id           | name  | age | city  |
|--------------|-------|-----|-------|
| 1001         | Alice | 25  | Seoul |
| 1002         | Bob   | 30  | Busan |
```

## `Document`

Document 모델은 웹 애플리케이션, 콘텐츠 관리(CMS, Content Management System)에 적합합니다.

### 구조
- JSON, BSON, XML 형식의 문서를 저장.
- 컬렉션(Collection) 안에 여러 개의 문서(Document)가 포함됨.

### 장점
- 스키마가 유연하여 동적 데이터 모델링 가능.
- JSON 기반이므로 REST API와 잘 호환됨.

### 단점
- 복잡한 관계형 데이터 모델에는 적합하지 않음.
- 중복 데이터 저장 가능성이 높음.

### 예제 (MongoDB)
```
{
    "_id": "1001",
    "name": "Alice",
    "age": 25,
    "city": "Seoul"
}
{
    "_id": "1002",
    "name": "Bob",
    "age": 30,
    "city": "Busan"
}
```

## `Graph`

### 구조
- 노드(Node)와 엣지(Edge)로 관계를 표현.
- 엣지에는 방향성이 있을 수도 있고 없을 수도 있음.

### 장점
- 복잡한 관계형 데이터 저장에 강점.
- 소셜 네트워크, 추천 시스템 등에 최적화됨.

### 단점
- 대규모 데이터 처리 시 성능 문제가 발생할 수 있음.
- 일반적인 트랜잭션 처리에는 적합하지 않음.

### 예제 (Neo4j)
```
CREATE (Alice:Person {name: "Alice", age: 25, city: "Seoul"})
CREATE (Bob:Person {name: "Bob", age: 30, city: "Busan"})
CREATE (Alice)-[:FRIEND]->(Bob)

(Alice) ---[FRIEND]---> (Bob)
```

## 🔹 RDBMS vs Wide-Column의 차이점

### ✅ RDBMS (MySQL, PostgreSQL)
- 테이블을 만들 때 고정된 스키마가 필요.
- 모든 행(Row)이 동일한 컬럼(Column) 개수와 타입을 가져야 함.

```
CREATE TABLE users (
    id INT PRIMARY KEY,
    name TEXT,
    age INT
);
```

### ✅ Wide-Column 모델 (Cassandra, HBase)
- 테이블(Column Family)을 만들 때 컬럼을 미리 정하지 않아도 됨.
- 각 행마다 컬럼을 다르게 설정 가능 (필요한 데이터만 저장 가능).
- 일부 행은 특정 컬럼이 없고, 다른 행은 추가적인 컬럼을 가질 수 있음.

| id       | name  | age | email         | phone   |
|---------|------|----|-------------|--------|
| 1       | Alice | 25 | alice@mail.com | - |
| 2       | Bob   | -  | -             | 010-1234-5678 |

	1번 행은 name, age, email 컬럼을 가짐.
	2번 행은 name, phone 컬럼만 존재 (age, email 컬럼 없음).
	즉, 각 행이 필요한 데이터만 저장할 수 있음 (컬럼 개수가 고정되지 않음).

### 🔹 실제 Cassandra 예제

```
-- users 테이블 생성
CREATE TABLE users (
    id UUID PRIMARY KEY
);
```

```
-- Alice 데이터 삽입 (name, age, email 포함)
INSERT INTO users (id, name, age, email)
VALUES (uuid(), 'Alice', 25, 'alice@mail.com');

-- Bob 데이터 삽입 (name, phone만 포함)
INSERT INTO users (id, name, phone)
VALUES (uuid(), 'Bob', '010-1234-5678');
```

### 💡 Wide-Column 모델이 행마다 다른 컬럼을 가질 수 있는 이유

1. 컬럼을 동적으로 추가 가능 → 미리 정의된 스키마가 필요 없음.
2. 필요한 데이터만 저장 가능 → 불필요한 Null 값 없이 효율적인 저장 가능.
3. 수평 확장성 → 데이터가 분산 저장되며, 새로운 컬럼을 추가해도 테이블 변경이 필요 없음.