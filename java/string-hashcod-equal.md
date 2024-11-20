# String 생성 방법

1. 리터럴
```java
String a = "12345;
```

2. new 연산자
```java
String a = new String("12345");
```

## 리터럴과 new 연산자 방식의 차이
리터럴로 생성된 객체는 String Constant Pool 영역에 존재한다. 리터럴은 내부적으로 String의 intern() 메소드를 호출한다.

intern()은 해당 문자열이 String Constant Pool에서 검색 후 주소값을 반환하거나 생성한다.
<br>new 연산자로 생성된 객체는 Heap 영역에 존재한다.

## `equals()` 와 `==` 의 차이
`equals()` 는 값을 비교하고 `==` 는 주소값을 비교한다.

## HashCode
HashCode 는 각 객체의 주소 값을 변환하여 생성한 고유의 주소값이다. .equals() 를 이용해서 두 객체가 같은지 판단하기 위해서는 .hashCode가 동일한지 비교한다.

```java
@Test
@DisplayName("String equals() && hashCode() 테스트")
public void _01_StringTest() {

    String a = "12345";
    String b = new String("12345");

    log.info("a.equals(b): {}", a.equals(b));       // true
    log.info("a == b: {}", a == b);                 // false
    log.info("a.hashCode(): {}", a.hashCode());     // 46792755
    log.info("b.hashCode(): {}", b.hashCode());     // 46792755

}
```