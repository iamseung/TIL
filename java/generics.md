# Java의 제네릭(Generics)

## 제네릭(Generics) 이란

`제네릭(Generics)` 은 클래스나 메서드에서 사용할 데이터 타입을 파라미터화하는 기능입니다.
즉, 컴파일 시 타입을 지정할 수 있도록 하여 코드의 재사용성을 높이고, 타입 안정성을 보장할 수 있습니다.

## 1. 제네릭을 사용하는 이유
1. 컴파일 타임 타입 체크 → 실행 전에 오류를 잡을 수 있습니다.
2. 형 변환, Casting 제거 → 불필요한 타입 변환을 줄여 코드 가독성을 증가시킬 수 있습니다.
3. 코드 재사용성 증가 → 여러 타입을 처리하는 일반화된 코드 작성이 가능합니다.


### 제네릭이 없을 때 (Object 사용)

```java
import java.util.ArrayList;

public class NonGenericExample {
    public static void main(String[] args) {
        ArrayList list = new ArrayList(); // 모든 타입을 받을 수 있음
        list.add("Hello");
        list.add(100); // 문자열과 숫자가 혼합됨

        String str = (String) list.get(0); // 형 변환 필요
        System.out.println(str);
    }
}
```
### 문제점
- `list.add(100)` 처럼 실수로 다른 타입을 넣어도 컴파일 오류가 발생하지 않습니다.
- `(String) list.get(0)` 처럼 형 변환이 필요하고, 타입 오류가 발생할 가능성이 높습니다.

## 2. 제네릭 기본 문법

제네릭을 사용하면 특정 타입만 지정할 수 있도록 제한이 가능합니다.

```java
import java.util.ArrayList;

public class GenericExample {
    public static void main(String[] args) {
        // 제네릭 사용
        ArrayList<String> list = new ArrayList<>();
        list.add("Hello");
        // ❌ 컴파일 에러 (String 타입만 허용)
        // list.add(100);

        // 형 변환 필요 없음
        String str = list.get(0);
        System.out.println(str);
    }
}
```

### 제네릭을 사용하면
- 컴파일 시 타입 체크 가능 → `list.add(100)`; 시 컴파일 오류가 발생합니다.
- 형 변환 불필요 → `(String) list.get(p)`; 대신 `list.get(0)` 으로 사용이 가능합니다.

## 3. 제네릭 클래스(Generic Class)

제네릭 클래스를 사용하여 여러 타입을 처리하는 일반적인 코드 작성이 가능합니다.
```java
class Box<T> {  // T는 타입 매개변수
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}
```
```java
public class Main {
    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.set("Hello");
        System.out.println(stringBox.get()); // "Hello"

        Box<Integer> intBox = new Box<>();
        intBox.set(123);
        System.out.println(intBox.get()); // 123
    }
}
```

> 제네릭을 사용하면 Box<T> 를 여러 타입(String, Integer) 으로 재사용 가능합니다.

## 4. 제네릭 메서드

메서드 레벨에서 제네릭을 사용할 수 있습니다.

```java
class Util {
    public static <T> void print(T data) { // 제네릭 메서드
        System.out.println(data);
    }
}

public class Main {
    public static void main(String[] args) {
        Util.print("Hello");  // "Hello"
        Util.print(123);      // 123
        Util.print(3.14);     // 3.14
    }
}
```

### 제네릭 메서드의 특징
- <T> 를 사용하여 타입을 일반화할 수 있습니다.
- 매개변수의 타입을 유연하게 처리 가능합니다.

## 5. 제네릭 타입 제한, Bounded Type 

### 제네릭에 특정 타입만 허용하는 방법
```java
class NumberBox<T extends Number> {  // Number 또는 그 하위 타입만 가능
    private T value;

    public void set(T value) {
        this.value = value;
    }

    public T get() {
        return value;
    }
}
```

### 제네릭 타입 제한 사용 예제
```java
public class Main {
    public static void main(String[] args) {
        NumberBox<Integer> intBox = new NumberBox<>();
        intBox.set(100);
        System.out.println(intBox.get()); // 100

        NumberBox<Double> doubleBox = new NumberBox<>();
        doubleBox.set(3.14);
        System.out.println(doubleBox.get()); // 3.14

        // NumberBox<String> stringBox = new NumberBox<>(); // ❌ 컴파일 에러 (Number가 아님)
    }
}
```

> 제한을 두면 `String` 과 같은 타입이 들어오지 않도록 강제가 가능합니다.

## 6. 와일드카드(?)를 사용한 제네릭

제네릭 타입을 완전히 특정하지 않고, 범위를 설정할 수 있습니다.

```java
class Printer {
    public static void printBox(Box<?> box) { // ? 는 어떤 타입이든 받을 수 있음
        System.out.println(box.get());
    }
}
```
```java
public class Main {
    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.set("Hello");

        Box<Integer> intBox = new Box<>();
        intBox.set(100);

        Printer.printBox(stringBox); // "Hello"
        Printer.printBox(intBox);    // 100
    }
}
```

## 7. 제네릭과 배열의 차이점

제네릭 배열은 직접 생성할 수 없습니다. 

```java
class GenericArray<T> {
    private T[] array;

    public GenericArray(int size) {
        // array = new T[size]; // ❌ 컴파일 에러!
        array = (T[]) new Object[size]; // ✅ 타입 변환 사용
    }

    public void set(int index, T value) {
        array[index] = value;
    }

    public T get(int index) {
        return array[index];
    }
}
```
제네릭 배열을 사용하려면 `Object[]` 배열로 생성한 후, 형 변환을 해야 합니다.

## Java 제네릭(Generics) 장점 정리

| 장점 | 설명 |
|------|------|
| **컴파일 타임 타입 체크** | 타입 오류를 컴파일 시점에서 발견 가능 ✅ |
| **형 변환 제거** | 불필요한 `(Type)` 형 변환을 없앨 수 있음 ✅ |
| **코드 재사용성 증가** | 여러 타입을 처리할 수 있는 일반적인 코드 작성 가능 ✅ |
| **타입 안정성 증가** | 잘못된 타입 입력 방지 ✅ |

## ✅ 결론, 제네릭을 사용하는 이유
    제네릭을 사용하면 코드 재사용성이 증가하고, 타입 안정성이 보장됩니다.
    List<T>, Map<K, V> 등의 컬렉션을 다룰 때 필수적으로 사용됩니다.
    형 변환을 줄이고, 유지보수가 쉬운 코드를 작성할 수 있습니다.