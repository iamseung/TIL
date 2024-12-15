# final 변수와 상수

`final` 키워드는 이름 그대로 끝 이라는 뜻, final 키워드가 붙으면 더는 값을 변경할 수 없다.

```java
public static void main(String[] args) { //final 지역 변수1
			final int data1;
			data1 = 10; //최초 한번만 할당 가능 
			// data1 = 20; //컴파일 오류
			
			// final 지역 변수2
			final int data2 = 10; 
			// data2 = 20; //컴파일 오류
			method(10);
}

```

- `final` 을 지역 변수에 설정할 경우 최초 한번만 할당할 수 있다. 이후에 변수의 값을 변경하려면 컴파일 오류가 발생한다.
- `final` 을 지역 변수 선언시 바로 초기화 한 경우 이미 값이 할당되었기 때문에 값을 할당할 수 없다.
- 매개변수에 `final` 이 붙으면 메서드 내부에서 매개변수의 값을 변경할 수 없다. 따라서 메서드 호출 시점에 사용된 값이 끝까지 사용된다.

`ConstructInit`

```java
package final1;

public class ConstructInit {

    final int value;

    public ConstructInit(int value) {
        this.value = value;
    }
}
```

- `ConstructIni` 과 같이 생성자를 사용해서 `final` 필드를 초기화 하는 경우, 각 인스턴스마다 final 필드에 다른 값을 할당할 수 있다.
- `final` 을 사용했기 때문에 생성 이후에 이 값을 변경하는 것은 불가능하다.

`FieldInit`

```java
package final1;

public class FieldInit {

    static final int CONST_VALUE = 10;
    final int value = 10;

}
```

- `FieldInit` 과 같이 `final` 필드를 필드에서 초기화 하는 경우,  모든 인스턴스가 다음 오른쪽 그림과 같이 같은 값을 가진다.
- 여기서 `FieldInit` 인스턴스의 모든 `value` 값은 10이 된다.
- 생성자 초기화와 다르게 필드 초기화는 필드의 코드에  해당 값이 미리 정해져 있기 때문.
- 모든 인스턴스가 같은  값을 사용하기 때문에 계속 생성되는 것은 개발자가 보기에 명확한 중복이다.
- 이러한 경우에 사용하면 좋은 것이 `static` 이다.

### **`static final`**

- 바뀌지 않는 공용 변수라고 볼 수 있다.
- `FieldInit.MY_VALUE` 는 `static` 영역에 존재한다. 그리고 `final` 키워드를 사용해서 초기화 값이 변하지 않는다.
- `static` 영역은 단 하나만 존재하는 영역이다. `MY_VALUE` 변수는 JVM 상에서 하나만 존재하므로 앞서 설명한 중복과 메로리 비효율 문제를 모두 해결할 수 있다.

⭐️ 이런 이유로 필드에 `final` + 필드 초기화를 사용하는 경우 `static` 을 붙여서 사용하는 것이 효과적이다.

### 상수, Constant

상수느 변하지 않고, 항상 일정한 값을 갖는 수를 의미. 자바에서는 보통 단 하나만 존재하는 변하지 않는 고정된 값을 상수라고 지칭한다. 이러한 이유로 상수는 `static final` 키워드를 사용한다.

**자바 상수 특징**

- `static final` 키워드를 사용한다.
- 대문자를 사용하고 구분은 `_` (언더스코어)로 한다. (관례)
    - 일반적인 변수와 상수를 구분하기 위해 이렇게 한다.
- 필드를 직접 접근해서 사용한다.
    - 상수는 기능이 아니라 고정된 값 자체를 사용하는 것이 목적이다.
    - 상수는 값을 변경할 수 없다. 따라서 필드에 직접 접근해도 데이터가 변하는 문제가 발생하지 않는다.

```java
package final1;

//상수
public class Constant {
    //수학 상수
    public static final double PI = 3.14;
    
    //시간 상수
    public static final int HOURS_IN_DAY = 24;
    public static final int MINUTES_IN_HOUR = 60;
    public static final int SECONDS_IN_MINUTE = 60;
    
    //애플리케이션 설정 상수
    public static final int MAX_USERS = 2000;
}
```

- 애플리케이션 안에는 다양한 상수가 존재할 수 있다. 수학, 시간 등등 실생활에서 사용하는 상수부터, 애플리케이션의 다양한 설정을 위한 상수들도 있다.
- 보통 이런 상수들은 애플리케이션 전반에서 사용되기 때문에 `public` 를 자주 사용한다. 물론 특정 위치에서만사용된다면 다른 접근 제어자를 사용하면 된다.
- 상수는 중앙에서 값을 하나로 관리할 수 있다는 장점도 있다.
- 상수는 런타임에 변경할 수 없다. 상수를 변경하려면 프로그램을 종료하고, 코드를 변경한 다음에 프로그램을 다시 실행해야 한다.

실제 애플리케이션 개발할 때, 중앙에서 관리되어야 하거나 여러 군데서 같이 쓰는

즉, 중앙에서 값을 하나로 관리해야 하는 상황에 상수를 주로 사용한다.

## final  변수와 참조

`final` 은 변수의 값을 변경하지 못하게 막는다. 그런데 여기서 변수의 값이라는 것이 뭘까?

- 변수는 크게 기본형 변수와 참조형 변수가 있다.
- 기본형 변수는 `10`, `20` 같은 값을 보관하고, 참조형 변수는 객체의 참조값을 보관한다.
    - `final` 을 기본형 변수에 사용하면 값을 변경할 수 없다.
    - `final` 을 참조형 변수에 사용하면 **참조값을 변경할 수 없다.**

```java
package final1;

public class Data {
    public int value;
}
```

```java
package final1;

public class FinalRefMain {
    public static void main(String[] args) {
        final Data data = new Data();
        // data 에 참조값이 할당되었기 때문에 더는 참조값을 변경할 수 없다.
        // data = new Data();

        //참조 대상의 값은 변경 가능
        data.value = 10;
        System.out.println(data.value);
        data.value = 20;
        System.out.println(data.value);
    }
}
```

참조형 변수 `data` 에 `final` 이 붙었다. 변수 선언 시점에 참조값을 할당했으므로 더는 참조값을 변경할 수 없다. → 데이터 자신을 바꾸는 것은 안된다.

```java
data.value = 10
data.value = 20
```

그런데 참조 대상의 객체 값은 변경할 수 있다.

- 참조형 변수 `data` 에 `final` 이 붙었다. 이 경우 ⭐️ 참조형 변수에 들어있는 참조값을 다른 값으로 변경하지 못한다.
    - 쉽게 이야기해서 이제 다른 객체를 참조할 수 없다. 그런데 이것의 정확한 뜻을 잘 이해해야 한다.
    - 참조형 변수 에 들어있는 참조값만 변경하지 못한다는 뜻이다. 이 변수 이외에 다른 곳에 영향을 주는 것이 아니다.
- `Data.value` 는 `final` 이 아니다. 따라서 값을 변경할 수 있다.