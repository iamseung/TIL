# 인터페이스(Interface)

## 인터페이스란

- 추상 메서드만 가지는 클래스의 일종입니다.
- 구현 클래스에서 반드시 메서드를 오버라이딩(Override) 해야 합니다.
- 다중 상속을 지원(여러 개의 인터페이스를 구현 가능) 합니다.
- 상수(public, static, final) 만 포함이 가능합니다.

### 인터페이스 선언 예제

```java
interface Animal {
    void makeSound(); // 추상 메서드 (구현 X)
}
```

### 인터페이스 구현 예제
```java
class Dog implements Animal {
    @Override
    public void makeSound() {
        System.out.println("멍멍!");
    }
}
```
```java
class Cat implements Animal {
    @Override
    public void makeSound() {
        System.out.println("야옹!");
    }
}
```

### 다형성을 활용한 코드

```java
public class Main {
    public static void main(String[] args) {
        Animal myDog = new Dog();
        Animal myCat = new Cat();
        
        myDog.makeSound(); // "멍멍!"
        myCat.makeSound(); // "야옹!"
    }
}
```
- Animal 인터페이스를 구현한 Dog 와 Cat 은 공통된 동작인 makeSound()를 반드시 구현해야 합니다.
- Animal 타입(인터페이스)을 통해 Dog 와 객체를 다룰 수 있습니다. → ⭐️ 다형성 활용

## 인터페이스를 사용하는 이유

### 1. 다형성(Polymorphism) 제공 → 유지 보수성 증가

인터페이스를 사용하면 구현체를 쉽게 변경할 수 있습니다.

```java
class Duck implements Animal {
    @Override
    public void makeSound() {
        System.out.println("꽥꽥!");
    }
}
```
- Animal 을 구현한 새로운 클래스 `Duck` 을 추가하더라도 기존 코드 변경없이 적용이 가능합니다.
- Dog, Cat, Duck 이 같은 Animal 타입을 따르므로 확장성이 증가합니다.

> `다형성`이란, 객체 지향 프로그래밍에서 같은 이름의 메서드나 연산자가 다른 클래스에서 다르게 동작하는 것을 의미합니다.

⭐️ 따라서 코드 변경 없이도 쉽게 새로운 기능을 추가할 수 있습니다. (OCP, 개방-폐쇄 원칙)

### 2. 결합도(Dependency) 낮추기 → 유지보수성 향상

✅ 인터페이스를 사용하면 구현체에 의존하지 않습니다.

```java
class AnimalService {
    private Animal animal;

    public AnimalService(Animal animal) { // 인터페이스 타입으로 주입
        this.animal = animal;
    }

    public void perform() {
        animal.makeSound();
    }
}
```

`AnimalService` 에서 `Animal` 이라는 인터페이스 타입으로 주입합니다.

✅ 구현 클래스 변경이 가능해집니다.

```java
public class Main {
    public static void main(String[] args) {
        AnimalService service1 = new AnimalService(new Dog());
        service1.perform(); // "멍멍!"

        AnimalService service2 = new AnimalService(new Cat());
        service2.perform(); // "야옹!"
    }
}
```

- AniamlService는 Dog 나 Cat이 아닌 Animal 인터페이스에만 의존합니다. ⭐️
- Dog 를 Cat 으로 바꿔도 AnimalService 코드는 변경할 필요가 없습니다.
- 즉, 결합도를 낮출 수 있으며 유지보수성이 증가하는 효과를 얻을 수 있습니다.

### 3. 다중 구현(Multiple Implementaion) 가능 → 유연성 증가

자바에서는 클래스는 단일 상속만 가능하지만, 인터페이스는 여러개 구현이 가능합니다.

```java
interface Flyable {
    void fly();
}

class Bird implements Animal, Flyable {
    @Override
    public void makeSound() {
        System.out.println("짹짹!");
    }

    @Override
    public void fly() {
        System.out.println("새가 날아갑니다!");
    }
}
```
- Bird 클래스는 Animal 과 Flyable 을 동시에 구현하여 소리도 내고 (fly()), 날 수도 있습니다.(makeSound())
- 다중 구현을 통해 기능 확장에 용이합니다.

### 4. 특정 동작 강제 (Contract) → 코드 안정성 증가

인터페이스를 구현하면 반드시 메서드를 오버라이딩 해야합니다.

```java
interface Payment {
    void pay(int amount);
}
```
```java
class CreditCardPayment implements Payment {
    @Override
    public void pay(int amount) {
        System.out.println("신용카드로 " + amount + "원 결제");
    }
}

class PayPalPayment implements Payment {
    @Override
    public void pay(int amount) {
        System.out.println("PayPal로 " + amount + "원 결제");
    }
}
```

인터페이스 타입으로 객체를 다룰 수 있습니다.

```java
public class Main {
    public static void main(String[] args) {
        Payment payment = new CreditCardPayment();
        payment.pay(5000); // "신용카드로 5000원 결제"

        payment = new PayPalPayment();
        payment.pay(7000); // "PayPal로 7000원 결제"
    }
}
```
- 모든 Payment 구현체는 pay() 메서드를 반드시 구현해야 합니다.
- 따라서 pay() 를 호출할 때 오류없이 동일한 동작을 보장할 수 있습니다.

## 인터페이스 vs 추상 클래스 비교

| 비교 항목 | 인터페이스 (`interface`) | 추상 클래스 (`abstract class`) |
|----------|-----------------|-----------------|
| **공통점** | 추상 메서드를 가질 수 있음 | 추상 메서드를 가질 수 있음 |
| **목적** | 다중 상속 및 특정 동작 강제 | 공통된 기본 동작 제공 |
| **다중 상속 가능 여부** | ✅ 다중 구현 가능 | ❌ 단일 상속만 가능 |
| **멤버 변수** | `static final`(상수)만 가능 | 일반 필드 선언 가능 |
| **메서드** | `default` 및 `static` 메서드 추가 가능 (Java 8 이상) | 일반 메서드 포함 가능 |
| **생성자** | ❌ 생성자 없음 | ✅ 생성자 있음 |
| **사용 예시** | `Animal`, `Payment`, `Flyable` 등 | `BaseController`, `AbstractClass` 등 |

> 여느 클래스들과 마찬가지로 인스턴스 변수와 인스턴스 메서드를 갖지만, 이를 상속하는 하위 클래스에 의해서 구현되어야 할 메서드가 하나 이상있는 경우 이를 `추상 클래스`라고 합니다.
<br>반면, `인터페이스`는 메소드의 본체가 비어있는 추상 메서드로만 구성되어 있습니다. <br>따라서 인스턴스 생성이 불가능하며 다른 클래스에 의해 구현되어야 합니다. 인터페이스는 표준화/캡슐화, 개발 속도 향상을 위해 사용됩니다.

## 결론

### ✅ 인터페이스를 사용하는 이유
    1.	다형성(Polymorphism) 제공 → 구현체 교체가 쉽고 유지보수 용이.
	2.	결합도 낮추기(Loose Coupling) → 코드 변경 최소화, 유연한 설계 가능.
	3.	다중 구현(Multiple Implementation) 가능 → 유연한 설계.
	4.	특정 동작을 강제(Contract) → 안정적인 코드 작성 가능.

➡ 즉, 인터페이스는 “설계 규약”을 정의하고, 구현체 간 결합도를 낮춰 유지보수를 쉽게 만들어주는 강력한 도구입니다. 