# 📚 자바 컬렉션 프레임워크 메서드 시간 복잡도 정리

## ✅ List 계열

| 메서드        | ArrayList | LinkedList |
|---------------|-----------|------------|
| `add()`       | O(1)      | O(1)       |
| `remove()`    | O(n)      | O(1)       |
| `get()`       | O(1)      | O(n)       |
| `contains()`  | O(n)      | O(n)       |

> 📝 *리스트는 `remove()`와 `get()`에서 ArrayList와 LinkedList 간에 큰 차이가 있으며,  
`contains()`는 내부적으로 순차 탐색이기 때문에 둘 다 O(n)입니다.*

# 📚 ArrayList vs LinkedList 차이 정리

Java의 대표적인 List 구현체인 `ArrayList`와 `LinkedList`는 내부 구조와 성능 특성이 다릅니다. 상황에 맞게 선택하지 않으면 성능 저하로 이어질 수 있습니다.

## ✅ 핵심 비교 표

| 항목               | ArrayList                          | LinkedList                          |
|--------------------|-------------------------------------|--------------------------------------|
| 내부 구조           | 동적 배열 (Object[])                | 이중 연결 리스트 (Doubly Linked List) |
| 인덱스 접근 (`get`) | O(1)                                | O(n) (앞에서부터 순회)                |
| 중간 삽입/삭제      | O(n) (요소 이동 필요)               | O(1)* (노드 참조 시)                  |
| 맨 앞/뒤 삽입/삭제  | O(n) (앞: 느림 / 뒤: 빠름)          | O(1)                                  |
| 메모리 사용량        | 낮음 (데이터만 저장)                | 높음 (노드마다 포인터 2개)            |
| 순차 탐색 성능       | 높음 (연속된 메모리 구조)           | 낮음 (비연속 메모리)                  |
| 캐시 적중률         | 높음                                | 낮음                                  |

> \* `LinkedList`는 노드 참조를 알고 있을 때 중간 삽입/삭제가 O(1)입니다.  
인덱스로 접근하면 여전히 O(n)입니다.

---

## ✅ ArrayList 특징
- ArrayList는 배열 기반의 동적 리스트로, 인덱스로 접근이 빠릅니다.
- ArrayList는 주로 읽기 중심의 데이터 처리, 랜덤 접근이 필요한 경우에 적합합니다.
- 내부적으로 `Object[]` 배열 사용
- 자동 크기 확장 (용량 초과 시 배열 복사 발생)
- **빠른 랜덤 접근**
- 중간 삽입/삭제 시 전체를 밀어야 하므로 **느림**

### 예시
```java
List<String> list = new ArrayList<>();
list.add("A");
list.add("B");
System.out.println(list.get(1)); // O(1)
```

## ✅ LinkedList 특징
- LinkedList는 이중 연결 리스트로, 각 요소가 이전/다음 노드를 가리킵니다.
- LinkedList는 삽입/삭제가 빈번하거나, Queue/Deque 구조로 사용할 때 적합합니다.
- 내부적으로 이중 연결 리스트로 구성
- 각 노드는 이전/다음 포인터를 가짐
- 삽입/삭제가 빈번한 작업에 유리
- 랜덤 접근이 느림

### 예시
```java
List<String> list = new LinkedList<>();
list.add("A");
list.add("B");
list.remove(0); // O(1) (맨 앞 삭제)
```

### ✅ Tip
- 일반적인 경우에는 대부분 ArrayList가 성능 면에서 더 유리합니다.
- LinkedList는 주로 Queue, Deque 용도로 쓰는 것이 일반적입니다.
- 데이터 양이 많고, 중간 삽입이 많은 특수한 경우에만 LinkedList를 고려하세요.

---

## ✅ Set 계열

| 메서드       | HashSet | LinkedHashSet | TreeSet |
|--------------|---------|----------------|---------|
| `add()`      | O(1)    | O(1)           | O(log n)|
| `contains()` | O(1)    | O(1)           | O(log n)|
| `next()`     | O(h/n)  | O(1)           | O(log n)|

> 📝 *`HashSet`은 내부적으로 해시 기반 자료구조(HashMap)를 사용하기 때문에 탐색 속도가 빠르고, contains() 연산이 평균 O(1)의 시간 복잡도로 동작하기 때문입니다.
`TreeSet`은 내부적으로 정렬된 이진 탐색 트리 구조로 동작합니다 (O(log n)).*

# 🔍 HashSet vs LinkedHashSet vs TreeSet 정리

자바의 `Set` 인터페이스 구현체인 `HashSet`, `LinkedHashSet`, `TreeSet`은  
모두 중복을 허용하지 않는 컬렉션이지만, 내부 구조와 정렬 방식, 성능에 차이가 있습니다.

---

## ✅ 핵심 비교 표

| 항목             | HashSet              | LinkedHashSet            | TreeSet                  |
|------------------|----------------------|---------------------------|---------------------------|
| 내부 구조         | 해시 테이블 (HashMap 기반) | 해시 테이블 + 이중 연결 리스트 | 이진 탐색 트리 (Red-Black Tree) |
| 정렬 여부         | ❌ 순서 없음           | ✅ 삽입 순서 유지            | ✅ 자동 정렬 (정렬 순 유지)     |
| 중복 허용         | ❌                   | ❌                         | ❌                         |
| `add()` 성능      | O(1) 평균            | O(1) 평균                  | O(log n)                  |
| `contains()` 성능 | O(1) 평균            | O(1) 평균                  | O(log n)                  |
| `remove()` 성능   | O(1) 평균            | O(1) 평균                  | O(log n)                  |
| 정렬 기준         | 없음                | 삽입 순서                  | `Comparable` 또는 `Comparator` |
| 널(null) 허용     | 1개 허용              | 1개 허용                   | 첫 원소로 `null` 불가         |

---

## ✅ HashSet

- **중복 허용 X, 순서 보장 X**
- 가장 빠른 Set – 평균 시간 복잡도 O(1)
- 내부적으로 `HashMap`을 사용하여 구현
- 가장 많이 사용됨

### 예시
```java
Set<String> set = new HashSet<>();
set.add("A"); set.add("B"); set.add("C");
System.out.println(set); // 순서 예측 불가
```

## ✅ LinkedHashSet

- **중복 허용 X, 순서 보장 O(삽입 순서 유지)**
- HashSet 보다 약간 느리지만, 순서를 보장해야 할 때 사용
- 내부적으로 해시 테이블 + 이중 연결 리스트 구조

### 예시
```java
Set<String> set = new LinkedHashSet<>();
set.add("A"); set.add("C"); set.add("B");
System.out.println(set); // [A, C, B]
```

## ✅ TreeSet

- **중복 허용 X, 순서 보장 O(자동 정렬됨)**
- 내부적으로 Red-Black Tree(이진 탐색 트리) 기반
    - TreeSet은 중복 없는 정렬된 데이터 집합을 유지해야 하기 때문에 → 정렬 + 삽입/삭제 시에도 균형 유지가 필수입니다.
    - 그래서 TreeSet은 내부적으로 레드-블랙 트리를 사용하여 `O(log n)` 성능을 보장합니다.
- 요소는 반드시 Comparable 을 구현하거나 Comparator 이 있어야 함.
- HashSet 보다 느리지만 정렬된 결과가 필요할 때 적합

### 예시
```java
Set<String> set = new TreeSet<>();
set.add("C"); set.add("A"); set.add("B");
System.out.println(set); // [A, B, C]
```

---

## ✅ Stack

| 메서드     | 시간복잡도 |
|------------|-------------|
| `push()`   | O(1)        |
| `pop()`    | O(1)        |
| `peek()`   | O(1)        |
| `search()` | O(n)        |

> 📝 *Stack은 LIFO 구조로 `push`, `pop`, `peek`은 빠르지만  
`search()`는 순차 탐색이므로 O(n)입니다.*

---

## ✅ Queue (PriorityQueue 기준)

| 메서드     | 시간복잡도 |
|------------|-------------|
| `offer()`  | O(log n)    |
| `peek()`   | O(1)        |
| `poll()`   | O(log n)    |
| `size()`   | O(1)        |

> 📝 *PriorityQueue는 내부적으로 힙 구조로 구현되어 있어 삽입과 삭제는 log n,  
`peek()`은 O(1)로 가장 빠른 우선순위 값을 확인할 수 있습니다.*

---

## ✅ Map 계열

| 메서드       | HashMap | LinkedHashMap | TreeMap |
|--------------|---------|----------------|---------|
| `get()`      | O(1)    | O(1)           | O(log n)|
| `contains()` | O(1)    | O(1)           | O(log n)|

> 📝 *`HashMap`과 `LinkedHashMap`은 평균적으로 O(1)의 성능을 보이며,  
`TreeMap`은 정렬된 키-값 구조를 유지하기 때문에 O(log n)의 성능을 가집니다.*

# 📌 HashMap vs LinkedHashMap vs TreeMap 정리

Java의 `Map` 인터페이스를 구현한 대표적인 클래스들인 `HashMap`, `LinkedHashMap`, `TreeMap`은  
모두 키-값 쌍을 저장하지만, **내부 구조와 순서 처리 방식, 성능**에서 차이가 있습니다.

---

## ✅ 핵심 비교 표

| 항목             | HashMap              | LinkedHashMap            | TreeMap                        |
|------------------|----------------------|---------------------------|---------------------------------|
| 내부 구조         | 해시 테이블           | 해시 테이블 + 이중 연결 리스트 | 레드-블랙 트리 (이진 탐색 트리)     |
| 정렬 여부         | ❌ 순서 없음          | ✅ 삽입 순서 유지            | ✅ 키 정렬 (자연 순서 또는 Comparator) |
| Null 허용 여부     | 키 1개, 값 무제한 허용 | 키 1개, 값 무제한 허용         | 키는 ❌ (null 불가), 값은 허용       |
| `put()` 성능      | O(1) 평균            | O(1) 평균                  | O(log n)                        |
| `get()` 성능      | O(1) 평균            | O(1) 평균                  | O(log n)                        |
| `containsKey()` 성능 | O(1) 평균          | O(1) 평균                  | O(log n)                        |
| 사용 목적         | 가장 일반적인 Map     | 순서 유지가 필요한 Map         | 정렬된 키로 탐색/범위 조회가 필요한 경우 |

---

## ✅ HashMap

- 가장 빠르고 가볍게 사용할 수 있는 일반적인 Map
- 내부적으로 **해시 테이블** 사용
- 순서 유지 X, 키 중복 X, null 키 1개 가능
- 평균적으로 모든 연산 O(1) → 해시 충돌 시 최악 O(n)

### 예시
```java
Map<String, Integer> map = new HashMap<>();
map.put("apple", 1);
map.put("banana", 2);
System.out.println(map); // 순서 보장 안 됨
```

## ✅ LinkedHashMap

- HashMap의 확장 → 삽입 순서를 유지
- 내부적으로 해시 테이블 + 이중 연결 리스트
- 순서가 중요한 캐시 구현 등에 사용
- accessOrder = true 설정 시 LRU 순서로도 정렬 가능

### 예시
```java
Map<String, Integer> map = new LinkedHashMap<>();
map.put("c", 3);
map.put("a", 1);
map.put("b", 2);
System.out.println(map); // [c=3, a=1, b=2]
```

## ✅ TreeMap
- 이진 탐색 트리 (Red-Black Tree) 기반 Map
- 키는 자동 정렬됨 (기본: Comparable / 사용자 정의 Comparator 가능)
- 범위 기반 탐색 (subMap, tailMap, headMap) 가능
- null 키 ❌ (예외 발생), 값은 허용

### 예시
```java
Map<String, Integer> map = new TreeMap<>();
map.put("c", 3);
map.put("a", 1);
map.put("b", 2);
System.out.println(map); // [a=1, b=2, c=3]
```

## 🌲 Tree 계열

## ✅ 트리 종류별 시간 복잡도 비교표

| 트리 종류                         | 탐색(Search) | 삽입(Insert) | 삭제(Delete) | 특징 요약                             |
|----------------------------------|--------------|--------------|--------------|----------------------------------------|
| **일반 이진 트리**               | O(n)         | O(n)         | O(n)         | 균형이 없는 경우 리스트처럼 편향될 수 있음 |
| **이진 탐색 트리 (BST)**         | O(h)         | O(h)         | O(h)         | 왼쪽 < 루트 < 오른쪽, h = 높이          |
| **균형 이진 탐색 트리 (AVL, RBT)** | O(log n)     | O(log n)     | O(log n)     | 자가 균형 유지, 항상 log n 보장         |
| **힙 (Heap)**                    | O(n)         | O(log n)     | O(log n)     | 최대/최소 우선순위만 빠르게 꺼냄         |
| **B-트리 / B+트리**              | O(log n)     | O(log n)     | O(log n)     | 디스크 I/O 최소화, DB에 자주 사용        |
| **트라이 (Trie)**                | O(k)         | O(k)         | O(k)         | 문자열 길이 기반 탐색, 자동완성 최적     |

---

## 🌳 트리별 구조 및 특징 설명

### 1. **일반 이진 트리 (Binary Tree)**

- 각 노드가 최대 2개의 자식을 가짐
- 특별한 정렬 없음 → 탐색 효율성 없음
- **완전 편향되면 선형 리스트처럼 작동**

```text
      A
     / \
    B   C
```

### 2. **이진 탐색 트리 (BST)**
- 왼쪽 < 부모 < 오른쪽 규칙을 따르는 트리
- 삽입 순서에 따라 트리 높이가 O(n) 까지 증가할 수 있음 <- 균형 유지 X

```text
      8
     / \
    3   10
   / \    \
  1   6   14
```

### 3. **균형 이진 탐색 트리 (AVL, Red-Black Tree)**
- BST의 한계(편향)를 극복하기 위해 스스로 균형 유지
- 대표적으로:
    - AVL 트리: 균형을 엄격히 유지 → 검색 빠름
    - Red-Black 트리: 약간 느슨한 균형 → 삽입/삭제 유리
- 자바의 TreeMap, TreeSet이 레드-블랙 트리 기반

```text
      8
     / \
    3   10
   / \    \
  1   6   14
```

### 4. **힙 (Heap)**
- 완전 이진 트리 구조
- 부모 ≥ 자식 (MaxHeap) 또는 부모 ≤ 자식 (MinHeap)
- 우선순위 큐로 사용 → 전체 정렬은 불가
- Java: PriorityQueue 로 제공

```text
    100
   /   \
  50    30
 / \
20 10
```

### 5. **B-트리 / B+트리**
- 다수의 자식을 가질 수 있는 트리
- 디스크 기반 저장 구조에 최적화됨
- 데이터베이스, 파일 시스템에 많이 사용됨
- B+트리는 모든 키를 리프 노드에 저장 + 순차 연결

```text
[B-트리]
      [10 | 20]
     /    |    \
  <10   10~20  >20
```

### 6. **트라이 (Trie)**
- 문자열의 문자 단위로 노드를 구성
- 루트 → 각 문자 → 단어 끝 표시 노드
- 문자 길이(k)에 비례한 탐색 시간
- 자동완성, 사전, 검색엔진에서 자주 사용

```text
Trie 저장 예: "cat", "car", "dog"

        (root)
        /    \
      c       d
     /         \
    a           o
   /             \
  t (✓)          g (✓)
  |
  r (✓)
```

###  🟢 Linked 의 의미
```
Hash Table:
+-------+        +-------+        +-------+
|  hash | -----> | Entry | -----> | Entry |
+-------+        +-------+        +-------+

이중 연결 리스트:
head <==> entry1 <==> entry2 <==> entry3 <==> tail
```
- Linked가 붙은 자바 자료구조는 데이터 순서 유지를 위해 내부적으로 이중 연결 리스트를 사용합니다.
- Double Linked List, 이중 연결 리스트는 `각 노드가 자신의 이전 노드와 다음 노드를 모두 알고 있는 연결 리스트입니다`.
- 이 구조 덕분에 양방향 순회가 가능하고, 중간 삽입/삭제가 효율적입니다.

###  🟢 Map 과 Set 의 차이
1. Map은 키(key) 로 값을 찾기 위한 자료구조이고, Set은 내부적으로 Map 을 사용하여 Key 만 저장한 자료구조입니다.
2. Map 의 키는 중복을 허용하지 않지만 값에는 중복을 허용하고, Set 은 중복 없는 유일한 값들의 집합입니다.
3. Map 은 Key 로 빠르게 값을 찾는데 최적화되어 있으며, Set은 빠른 검색이나 유일성 검증에 유리합니다.

## 🟢 Hash 충돌이란 ?
서로 다른 객체인데도 hashCode() 값이 같은 경우, 두 객체가 해시 테이블에서 같은 버킷(bucket) 에 들어가 충돌이 발생하는 것
```java
Object A → hashCode() = 123
Object B → hashCode() = 123  ← 충돌 발생
```

### 🚫 자바의 해시 충돌 처리 방식

### 1. 해시 -> 버킷 인덱스 계산
```java
int hash = key.hashCode();
int index = hash % table.length;
```
- 해시값을 배열 크기로 나눠서 버킷 인덱스를 결정

### 2. 같은 인덱스에 여러 값이 들어오면 ? 
자바는 버킷 충돌 처리를 위해 아래 두 단계를 사용합니다.

#### ✅ 1. 체이닝 방식 (Chaining) — 기본 방식
- 충돌한 요소들을 LinkedList로 연결하여 버킷 하나에 여러 항목 저장

```java
index[5] → [Entry A] → [Entry B] → [Entry C]
```

- 새로운 값 삽입 시: 해당 리스트의 각 노드에 대해 equals() 비교
- get() 시에도 equals() 로 비교하며 선형 탐색

#### ✅ 2. 트리화(Treeification) — Java 8부터 도입
- 충돌이 너무 많아 리스트가 길어지면 → 성능이 O(n) → O(log n) 으로 개선
- 기준:
    - 하나의 버킷에 8개 이상의 노드가 쌓이고
    - 전체 해시 테이블 크기가 64 이상일 경우
```java
index[5] → Red-Black Tree (Entry A, B, C, ...)
```
→ 해당 버킷을 Red-Black Tree로 변환


- https://hahahoho5915.tistory.com/35
- https://ironmask43.tistory.com/100