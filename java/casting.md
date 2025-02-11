# 형 변환, Casting

## Casting
형변환, Casting 이란 데이터 타입을 다른 타입으로 변환하는 과정을 의미합니다.

Java 에서는 `기본형(Primitive Type)` 과 `참조형(Reference Type)` 에 대해 형 변환이 가능합니다.

## 1. 기본형 형 변환

### 1. 묵시적 형 변환 (`Implicit Casting`, 자동 형 변환)
- `작은 크기의 타입 → 큰 크기의 타입 변환 (안전한 변환이므로 자동으로 처리됨)`
- 데이터 손실이 발생하지 않습니다.

```java
int num = 10;
double d = num;  // int → double 자동 변환
System.out.println(d); // 10.0
```

### 자동 변환이 가능한 경우
|작은 타입|→|큰 타입|
|---|-|---|
|byte	|→|	short, int, long, float, double|
|short	|→|	int, long, float, double|
|int	|→|	long, float, double|
|long	|→|	float, double|
|float	|→|	double|

### 2. 명시적 형 변환 (Explicit Casting, 강제 형 변환)
- `큰 크기의 타입 → 작은 크기의 타입 변환`
- `데이터 손실`이 발생할 수 있으므로 명시적으로 형 변환해야 합니다.
- (타입)을 사용하여 변환합니다.

```java
double d = 10.99;
int num = (int) d;  // double → int 강제 변환
System.out.println(num); // 10 (소수점 손실)
```
```java
int largeNumber = 130;
byte smallNumber = (byte) largeNumber;
System.out.println(smallNumber); // -126 (데이터 손실 발생!)
```
> byte의 범위는 -128 ~ 127 이므로 130을 변환할 경우 오버플로우가 발생합니다.

## 2. 참조형(Reference Type) 형 변환
참조형(객체 타입) 간 형 변환은 상속 관계가 있어야 가능하며, `업캐스팅(Upcasting)`과 `다운캐스팅(Downcasting)`으로 나뉩니다.

### 1. 업캐스팅(Upcasting) - 자식 타입 → 부모 타입 (자동 변환)
- 자식 클래스 객체를 부모 클래스 타입으로 변환
- 자동 변환 가능 (명시적인 형 변환 필요 없음)
- 부모 클래스에 정의된 메서드만 호출 가능합니다.

```java
class Animal {
    void sound() {
        System.out.println("동물 소리");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println("멍멍!");
    }
}

public class Main {
    public static void main(String[] args) {
        // 자동 업캐스팅 (Animal 타입으로 Dog 객체 저장)
        Animal animal = new Dog();
        // "동물 소리" (부모 클래스 메서드 실행)
        animal.sound(); 
        // ❌ 컴파일 오류 (부모 타입에서는 자식의 메서드 사용 불가능)
        // animal.bark();
    }
}
```

- Dog 객체를 Animal 타입으로 변환 (자동 업캐스팅)
- 부모 클래스의 메서드만 호출 가능 (자식 클래스의 bark() 메서드는 접근 불가)
> 💡`즉, 업캐스팅을 하면 자식 클래스의 오버라이딩된 메서드는 실행되지만, 자식 클래스에서 새로 추가된 메서드는 호출할 수 없습니다.`

### 2. 다운캐스팅(Upcasting) - 부모 타입 → 자식 타입 (강제 변환)
- 부모 클래스 타입을 자식 클래스 타입으로 변환 (명시적 형 변환 필요)
- 런타임 오류(ClassCastException) 발생 가능

```java
public class Main {
    public static void main(String[] args) {
        Animal animal = new Dog(); // 업캐스팅
        Dog dog = (Dog) animal; // 다운캐스팅 (명시적 변환)
        dog.bark(); // "멍멍!" (자식 클래스 메서드 호출 가능)
    }
}
```
```java
Animal animal = new Animal();
Dog dog = (Dog) animal; // ❌ ClassCastException 발생
```

### 주의할점
- `Animal` 객체는 `Dog` 로 변환될 수 없습니다.
- 다운캐스팅 할 때는 `instanceof` 연산자로 타입을 확인해야 합니다.

```java
if (animal instanceof Dog) {
    Dog dog = (Dog) animal;
    dog.bark();
} else {
    System.out.println("변환할 수 없음");
}
```

### 업캐스팅과 다운캐스팅 정리

| 개념 | 설명 | 예제 |
|------|------|------|
| **업캐스팅(Upcasting)** | 부모 타입으로 변환 (자동 가능) | `Animal animal = new Dog();` |
| **오버라이딩된 메서드 호출** | 부모에 존재하는 메서드는 자식의 오버라이딩된 메서드 실행 | `animal.sound(); // "개짖는 소리"` |
| **자식에서 추가된 메서드는 호출 불가** | 부모 타입에는 없는 메서드는 호출 불가능 | `animal.bark(); // ❌ 컴파일 오류` |
| **다운캐스팅(Downcasting)** | 다시 자식 타입으로 변환해야 호출 가능 | `((Dog) animal).bark(); // "멍멍!"` |

## 3. 제네릭과 형 변환

제네릭(Generics)을 사용하면 형 변환을 줄일 수 있습니다.

### 제네릭이 없을 때

```java
ArrayList list = new ArrayList();
list.add("Hello");
String str = (String) list.get(0); // 형 변환 필요
```

### 제네릭을 사용하면 형 변환 불필요
```java
ArrayList<String> list = new ArrayList<>();
list.add("Hello");
String str = list.get(0); // 형 변환 필요 없음 ✅
```

> 제네릭을 사용하면 불필요한 (String) 형 변환 없이 타입 안정성을 보장합니다.

## Java 형 변환(Casting) 정리

| 유형 | 설명 | 예제 |
|------|------|------|
| **묵시적 형 변환** | 작은 타입 → 큰 타입 자동 변환 | `int → double` |
| **명시적 형 변환** | 큰 타입 → 작은 타입 강제 변환 (데이터 손실 가능) | `double → int` |
| **업캐스팅** | 자식 → 부모 변환 (자동 가능) | `Dog → Animal` |
| **다운캐스팅** | 부모 → 자식 변환 (명시적 변환 필요, 위험) | `(Dog) animal` |
| **제네릭과 형 변환** | 제네릭을 사용하면 형 변환 불필요 | `List<String>` |

## ✅ 결론, 형 변환을 사용하는 이유
    형 변환은 기본형과 참조형에서 사용되며, 타입 간 변환을 가능하게 합니다.
    업캐스팅은 자동 변환되지만, 다운캐스팅은 `instanceof`로 확인해야 합니다.
    제네릭을 사용하면 불필요한 형 변환을 줄일 수 있어 코드 안정성이 증가합니다.

    즉, 형 변환은 변수의 데이터 타입을 다른 타입으로 변환하는 과정입니다. 이를 사용하는 이유는 다양한 타입을 일관되게 처리하고, 메모리를 효율적으로 사용하며, 다형성을 활용하기 위해서 입니다.