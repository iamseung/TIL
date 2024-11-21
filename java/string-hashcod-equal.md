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
리터럴로 생성된 객체는 `String Constant Pool` 영역에 존재합니다. 리터럴은 내부적으로 String의 `intern()` 메소드를 호출합니다.

intern()은 해당 문자열이 String Constant Pool에서 검색 후 주소값을 반환하거나 생성합니다. new 연산자로 생성된 객체는 Heap 영역에 존재합니다.

## `equals()` 와 `==` 의 차이
`equals()` 는 값을 비교하고 `==` 는 주소값을 비교합니다.

### `equals()`

`boolean equals(Object obj)` 로 정의된 equals 메소드는 기본적으로 2개의 객체가 동일한지 검사하기 위해 사용됩니다. equals 가 구현된 방법은 2개의 객체가 참조하는 것이 동일한지를 확인하는 것이며, 이는 **동일성(Identity)** 을 비교하는 것입니다.

즉, 2개의 객체가 가리키는 곳이 동일한 메모리 주소일 경우에만 동일한 객체가 됨을 의미합니다.

```java
[Object.class]
public boolean equals(Object obj) {
    return (this == obj);
}
```
하지만 프로그래밍을 하다보면 동일한 객체가 메모리 상에 여러 개 띄워져있는 경우가 있습니다. 해당 객체는 서로 다른 메모리에 띄워져있으므로 동일한(Identity) 객체가 아닙니다. 하지만 프로그래밍 상으로는 같은 값을 지니므로 같은 객체로 인식되어야 하는데, 이러한 동등성(Equality)를 위해 우리는 값으로 객체를 비교하도록 equals 메소드를 오버라이딩해주는 것입니다.

예를 들어 아래와 같이 동일한 값을 갖는 문자열을 2개 생성하면 각각은 서로 다른 메모리에 할당되므로 동일하지 않습니다. 대신 같은 값을 지니므로 동등합니다. 하지만 동일성을 비교하는 equals 메소드를 호출해보면 true가 나오는데, 그 이유는 String 클래스에서 equals 메소드를 오버라이드하여 객체가 같은 값을 갖는지 동등성(Equality)을 비교하도록 처리했기 때문입니다.

```java
String s1 = new String("Test");
String s2 = new String("Test");

System.out.println(s1 == s2);			// false
System.out.println(s1.equals(s2));		// true;

[String.class]
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

## HashCode
**HashCode 는 각 객체의 주소 값을 변환하여 생성한 고유의 주소값입니다.** .equals() 를 이용해서 두 객체가 같은지 판단하기 위해서는 `.hashCode` 가 동일한지 비교해야 합니다.

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