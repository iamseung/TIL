## this


```java
package construct;
public class MemberInit {
    String name;
    int age;
    int grade;
    
		//추가
		void initMember(String name, int age, int grade) {
		          this.name = name;
		          this.age = age;
		          this.grade = grade;
		} 
}
```

```java
void initMember(String name, int age, int grade) {
```

`Member` 의 코드를 다시 보자

`initMember(String name...)` 의 코드를 보면 메서드의 매개변수에 정의한 String name 과 Member 의 멤버 변수의 이름이 `String name` 으로 둘다 똑같다. 나머지 변수 이름도 name , age , grade 로 모두 같다.

멤버 변수와 메서드의 매개변수의 이름이 같으면 둘을 어떻게 구분해야 할까? 이 경우 멤버 변수보다 매개변수가 코드 블럭의 더 안쪽에 있기 때문에 **`매개변수가 우선순위`**를 가진다. 따라서`initMember(String name,...)` 메서드 안에서 `name` 이라고 적으면 매개변수에 접근하게 된다.

```java
this.name = name; //1. 오른쪽의 name은 매개변수에 접근
this.name = "user"; //2. name 매개변수의 값 사용
x001.name = "user"; //3. this.은 인스턴스 자신의 참조값을 뜻함, 따라서 인스턴스의 멤버 변수에 접근
```

멤버 변수에 접근하려면 앞에 `this.` 이라고 해주면 된다. 여기서 `this` 는 인스턴스 자신의 참조값을 가리킨다.

## **생성자 호출**

생성자는 인스턴스를 생성하고 나서 즉시 호출된다. 생성자를 호출하는 방법은 다음 코드와 같이 `new` 명령어 다음에 생성자 이름과 매개변수에 맞추어 인수를 전달하면 된다.

```java
new 생성자이름(생성자에 맞는 인수 목록)
new 클래스이름(생성자에 맞는 인수 목록)
```

참고로 생성자이름이 클래스 이름이기 때문에 둘다 맞는 표현이다.

```java
new MemberConstruct("user1", 15, 90)
```

이렇게 하면 인스턴스를 생성하고 즉시 해당 생성자를 호출한다. 여기서는 `Member` 인스턴스를 생성하고 바로 `MemberConstruct(String name, int age, int grade)` 생성자를 호출한다.

참고로 `new` 키워드를 사용해서 객체를 생성할 때 마지막에 괄호 `()` 도 포함해야 하는 이유가 바로 생성자 때문이다. 객 체를 생성하면서 동시에 생성자를 호출한다는 의미를 포함한다.

## **생성자 장점**

### **중복 호출 제거**

생성자가 없던 시절에는 생성 직후에 어떤 작업을 수행하기 위해 다음과 같이 메서드를 직접 한번 더 호출해야 했다. 생성자 덕분에 객체를 생성하면서 동시에 생성 직후에 필요한 작업을 한번에 처리할 수 있게 되었다.

```java
//생성자 등장 전
MemberInit member = new MemberInit(); member.initMember("user1", 15, 90);
//생성자 등장 후
MemberConstruct member = new MemberConstruct("user1", 15, 90);
```

### **제약 - 생성자 호출 필수**

방금 코드에서 생성자 등장 전 코드를 보자. 이 경우 `initMember(...)` 를 실수로 호출하지 않으면 어떻게 될까? 

이 메서드를 실수로 호출하지 않아도 프로그램은 작동한다. 하지만 회원의 이름과 나이, 성적 데이터가 없는 상태로 프로그 램이 동작하게 된다. 만약에 이 값들을 필수로 반드시 입력해야 한다면, 시스템에 큰 문제가 발생할 수 있다. 결국 아무 정보가 없는 유령 회원이 시스템 내부에 등장하게 된다.

생성자의 진짜 장점은 객체를 생성할 때 직접 정의한 생성자가 있다면 `직접 정의한 생성자를 반드시 호출해야 한다는 점`이다. 참고로 생성자를 메서드 오버로딩 처럼 여러개 정의할 수 있는데, 이 경우에는 하나만 호출하면 된다.

`MemberConstruct` 클래스의 경우 다음 생성자를 직접 정의했기 때문에 직접 정의한 생성자를 반드시 호출해야 한다.

```java
MemberConstruct(String name, int age, int grade) {...}
```

다음과 같이 직접 정의한 생성자를 호출하지 않으면 컴파일 오류가 발생한다.

 `no suitable constructor found for MemberConstruct(no arguments)`

```java
MemberConstruct member3 = new MemberConstruct(); //컴파일 오류 발생 
member3.name = "user1";
```

컴파일 오류는 IDE에서 즉시 확인할 수 있는 좋은 오류이다. 이 경우 개발자는 객체를 생성할 때, 직접 정의한 생성자를 필수로 호출해야 한다는 것을 바로 알 수 있다. 그래서 필요한 생성자를 찾아서 다음과 같이 호출할 것이다. → 생**성자를 사용하면 `필수값 입력을 보장`할 수 있다.**

## 기본 생성자

매개변수가 없는 생성자 → 기본 생성자

클래스에 생성자가 하나도 없으면 자바 컴파일러는 매개변수가 없고, 작동하는 코드가 없는 기본 생성자를 자동으로 만들어준다.

`생성자가 하나라도 있으면 자바는 기본 생성자를 만들지 않는다.`

### **기본 생성자를 왜 자동으로 만들어줄까?**

만약 자바에서 기본 생성자를 만들어주지 않는다면 생성자 기능이 필요하지 않은 경우에도 모든 클래스에 개발자가 직접 기본 생성자를 정의해야 한다. 생성자 기능을 사용하지 않는 경우도 많기 때문에 이런 편의 기능을 제공한다.

**정리**

- 생성자는 반드시 호출되어야 한다.
- 생성자가 없으면 기본 생성자가 제공된다.
- **생성자가 하나라도 있으면 기본 생성자가 제공되지 않는다.**이 경우 개발자가 정의한 생성자를 직접 호출해야 한다.

## 생성자 → 오버로딩, `this()`

생성자도 메서드 오버로딩처럼 매개변수만 다르게 해서 여러 생성자를 제공할 수 있다.

```java
package construct;

public class MemberConstruct {
    String name;
    int age;
    int grade;
    
		//추가
		MemberConstruct(String name, int age) {
        this.name = name;
        this.age = age;
        this.grade = 50;
		}
		
		MemberConstruct(String name, int age, int grade) { System.out.println("생성자 호출 name=" + name + ",age=" + age + ",grade="
		  + grade);
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
}
```

### this()

```java

package construct;

public class MemberConstruct {
    String name;
    int age;
    int grade;
    
		MemberConstruct(String name, int age) {
				this(name, age, 50); //변경
		}
		
		MemberConstruct(String name, int age, int grade) { System.out.println("생성자 호출 name=" + name + ",age=" + age + ",grade="
		+ grade);
        this.name = name;
        this.age = age;
        this.grade = grade;
    }
}
```

`this()` 라는 기능을 사용하면 생성자 내부에서 자신의 생성자를 호출할 수 있다. 참고로 `this` 는 인스턴스 자신의 참조값을 가리킨다. 그래서 자신의 생성자를 호출한다고 생각하면 된다.
	

`this()` 는 생성자 코드의 첫줄에만 작성할 수 있다.