# 기본형 vs 참조형

사용하는 값을 변수에 직접 넣을 수 있는 기본형, 그리고 이전에 본 `Student student1` 과 같이 객체가 저장된 메모리의 위치를 가리키는 참조값을 넣을 수 있는 참조형으로 분류할 수 있다.

기본형(Primitive Type) →  `int` , `long` , `double` , `boolean` 처럼 변수에 사용할 값을 직접 넣을 수 있는 데이터 타입을 기본형이라 한다.

참조형(Reference Type) → `Student student1` , `int[] students` 와 같이 `데이터에 접근하기 위한 참조(주소)를 저장하는 데이터 타입`을 참조형이라 한다. 참조형은 `객체` 또는 `배열`에 사용된다.

즉, `기본형 변수`에는 10, 20과 같이 실제 사용하는 값을 변수에 담을 수 있다.

`참조형 변수`에는 위치, 참조값이 들어가 있다. 참조형 변수를 통해서 뭔가 하려면 결국 참조값을 통해 해당 위치로 이동해야 한다. 이름 그대로 실제 객제의 위치(참조, 주소)를 저장한다.

기본형은 연산이 가능

```java
int a = 10, b = 20;
int sum = a + b;
```

참조형은 연산이 불가능

```java
Student s1 = new Student();
Student s2 = new Student(); 
s1 + s2 //오류 발생
```

- 기본형을 제외한 나머지는 모두 참조형이다.
    - 기본형은 모두 소문자로 시작한다.
    - 기본형은 자바가 기본으로 제공하는 데이터 타입이다.
- 클래스는 대문자로 시작하며 모두 참조형이다.

하지만 `String` 은 사실 클래스이며, 참조형이다. 그런데 기본형처럼 문자 값을 바로 대입할 수 있다.

문자는 매우 자주 다루기 때문에 자바에서 특별하게 편의 기능을 제공한다.

## 변수 대입

### 기본형 대입

```java
int a = 10;
int b = a;
```

기본형은 변수에 값을 대입하더라도 실제 사용하는 값이 변수에 바로 들어있기 때문에 해당 값만 복사해서 대입한다고 생각하면 된다.

즉, a 와 b 는 서로 다른 10 을 의미하며 a 의 값을 수정해도 b 의 10이라는 값은 변하지 않는다.

### 참조형 대입

```java
Student s1 = new Student();
Student s2 = s1;
```

`참조형의 경우 실제 사용하는 객체가 아니라 객체의 위치를 카르키는 참조 값만 복사`된다. 죽, 실제 건물이 복사되는 것이 아니라 건물의 위치인 주소만 복사되는 것이다.

## 메서드 호출

기본형이면 변수에 들어 있는 실제 사용하는 값을 복사해서 대입하고, 참조형이면 변수에 들어 있는 참조값을 복사해서 대입한다.

메서드 호출도 마찬가지다. 메서드를 호출할 때 사용하는 매개변수(파라미터)도 결국 변수일 뿐이다.

자바에서 메서드의 매개변수(파라미터)는 항상 값에 의해 전달된다. 그러나 이 값이 실제 값이냐, 참조(메모리 주소)값이냐에 따라 동작이 달라진다.

- **`기본형`**: 메서드로 기본형 데이터를 전달하면, `해당 값이 복사되어 전달`된다. 
**이 경우, 메서드 내부에서 매개변수(파라미터)의 값을 변경해도, 호출자의 변수 값에는 영향이 없다.**
- **`참조형`**: 메서드로 참조형 데이터를 전달하면, `참조값이 복사되어 전달`된다. 
**이 경우, 메서드 내부에서 매개변수(파라미터)로 전달된 객체의 멤버 변수를 변경하면, 호출자의 객체도 변경된다.**

## 변수와 초기화

### 변수의 종류

- 멤버 변수(필드) → 클래스에 선언
- 지역 변수 → 메서드에 선언, 매개변수도 지역 변수의 한 종류

멤버 변수, 필드 예시

```java
 public class Student {
      String name;
			int age;
			int grade; 
}
```

지역 변수 예시

```java
public class ClassStart3 {
    public static void main(String[] args) {
        Student student1;
        student1 = new Student();
        Student student2 = new Student();
		} 
}
```

```java

public class MethodChange1 {
    public static void main(String[] args) {
        int a = 10;
				System.out.println("메서드 호출 전: a = " + a); 
				changePrimitive(a); 
				System.out.println("메서드 호출 후: a = " + a);
		}
		
    public static void changePrimitive(int x) {
        x = 20;

		} 
}
```

a, x(매개변수) 는 지역 변수이다. 지역 변수는 이름 그대로 특정 지역에서만 사용되는 변수라는 뜻이다.  예를 들어서 변수 `x` 는 `changePrimitive()` 메서드의 블록에서만 사용된다. `changePrimitive()` 메서드가 끝나면 제거된다. `a` 변수도 마찬가지이다.

### 변수의 값 초기화

- 멤버 변수: 자동 초기화
    - 인스턴스의 멤버 변수는 인스턴스를 생성할 때 자동으로 초기화된다.
    - 숫자(`int`)= 0 , `boolean` = `false` , 참조형 = `null` (`null` 값은 참조할 대상이 없다는 뜻으로 사용된다.)
    - 개발자가 초기값을 직접 지정할 수 있다.
- 지역 변수: 수동 초기화
    - 지역 변수는 항상 직접 초기화해야 한다.

```java
[InitData]
package ref;

public class InitData {
	int value1; //초기화 하지 않음 
	int value2 = 10; //10으로 초기화
}

[InitMain]
package ref;
  public class InitMain {
      public static void main(String[] args) {
          InitData data = new InitData();
          System.out.println("value1 = " + data.value1); // 0
          System.out.println("value2 = " + data.value2); // 10
	} 
}

```

`value1` 은 초기값을 지정하지 않았지만 멤버 변수는 자동으로 초기화 된다. 숫자는 `0` 으로 초기화된다. `value2` 는 `10` 으로 초기값을 지정해두었기 때문에 객체를 생성할 때 `10` 으로 초기화된다.