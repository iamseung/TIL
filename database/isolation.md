# 트랜잭션 격리 수준

**`READ UNCOMMITTED`**
- 커밋되지 않은 변경 사항도 읽을 수 있습니다.
- JPA에서 읽기 작업 중 Dirty Read(더러운 읽기)가 발생할 수 있음.

**`READ COMMITTED (기본)`**
- 커밋된 데이터만 읽습니다.
- JPA에서 트랜잭션 간 Dirty Read를 방지하지만, Non-Repeatable Read(반복 불가능한 읽기)가 발생할 수 있음.

**`REPEATABLE READ`**
- 트랜잭션이 시작될 때 조회한 데이터를 이후에도 일관되게 유지합니다.
- JPA의 변경 감지(Dirty Checking)와 함께 사용하면 데이터 정합성이 강화됩니다.
    - Dirty Checking → 트랜잭션이 커밋되거나 flush()가 호출될 때, 영속성 컨텍스트 내의 엔티티 상태를 스냅샷과 비교해 변경된 내용을 감지

**`SERIALIZABLE`**
- 가장 높은 격리 수준으로, 트랜잭션 간 충돌을 완전히 방지합니다.
- JPA의 동작은 정확하지만, 동시성 성능이 크게 저하됩니다.