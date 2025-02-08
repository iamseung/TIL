# Static 변수

`static` 키워드는 주로 멤버 변수와 메서드에 사용된다. 특정 클래스에서 공용으로 함께 사용할 수 있는 변수를 만들 수 있다면 편리할 것이다. `static` 키워드를 사용하면 공용으로 함께 사용하는 변수를 만들 수 있다.

멤버 변수에 `static` 을 붙이게 되면 `static` 변수, 정적 변수 또는 클래스 변수라 한다.

```java
public class Data3 {
    public String name;
    public static int count; //static
    public Data3(String name) {
        this.name = name;
        count++;
		} 
}

..
..

public class DataCountMain3 {
    public static void main(String[] args) {
        Data3 data1 = new Data3("A");
        System.out.println("A count=" + Data3.count);
        Data3 data2 = new Data3("B");
        System.out.println("B count=" + Data3.count);
        Data3 data3 = new Data3("C");
        System.out.println("C count=" + Data3.count);
    }
}

------------------------------------------------
------------------------------------------------

A count=1
B count=2
C count=3
```

Data3.count 와 같이 클래스 명에 .(dot) 을 사용한다. 클래스에 직접 접근하는 것처럼 느껴진다.

- `static` 이 붙은 멤버 변수는 메서드 영역에서 관리한다.
    - `static` 이 붙은 멤버 변수 `count` 는 인스턴스 영역에 생성되지 않는다. 대신에 메서드 영역에서 이 변수를 관리한다.
    - `Data3("A")` 인스턴스를 생성하면 생성자가 호출된다
    - 생성자에는 `count++` 코드가 있다. `count` 는 `static` 이 붙은 정적 변수다. **정적 변수는 인스턴스 영역이 아니라 메서드 영역에서 관리한다.** 따라서 이 경우 메서드 영역에 있는 `count` 의 값이 하나 증가된다.

- `Data3("B")` 인스턴스를 생성하면 생성자가 호출된다
- `count++` 코드가 있다. `count` 는 `static` 이 붙은 정적 변수다. 메서드 영역에 있는 `count` 변수의 값이 하나 증가된다.

- `Data3("C")` 인스턴스를 생성하면 생성자가 호출된다
- `count++` 코드가 있다. `count` 는 `static` 이 붙은 정적 변수다. 메서드 영역에 있는 `count` 변수의 값이 하나 증가된다.

최종적으로 메서드 영역에 있는 `count` 변수의 값은 3이 된다. `static` 이 붙은 정적 변수에 접근하려면 Data3.count 와 같이 클래스명 + `.` (dot) + 변수명으로 접근하면 된다.

참고로 Data3 의 생성자와 같이 자신의 클래스에 있는 정적 변수라면 클래스명을 생략할 수 있다.

<aside>
💡

**정리**
`static` 변수는 쉽게 이야기해서 클래스인 붕어빵 틀이 특별히 관리하는 변수이다. 붕어빵 틀은 1개이므로 클래스 변수도 하나만 존재한다. 반면에 인스턴스 변수는 붕어빵인 인스턴스의 수 만큼 존재한다.

`static` 이 붙은 멤버 변수는 메서드 영역에서 관리한다. 인스턴스 영역에 생성되지 않는다.

</aside>

## 용어 정리

```java
public class Data3 {
		public String name;
		public static int count;
}
```

`name`, `count` 는 둘다 멤버 변수이다. 멤버 변수(필드) 는 `static` 이 붙은 것과 아닌 것에 따라 다음과 같이 분리할 수 있다.

### 멤버 변수(필드) 의 종류

- 인스턴스 변수 → static 이 붙지  않은 변수
    - `static` 이 붙지 않은 멤버 변수는 인스턴스를 생성해야 사용할 수 있고, 인스턴스에 소속되어 있다. 따라서 인스턴스 변수라 한다.
    - 인스턴스 변수는 인스턴스를 만들 때 마다 새로 만들어진다.
- 클래스 변수 → static 이 붙은 변수
    - 클래스 변수, 정적 변수, `static` 변수등으로 부른다.
    - `static` 이 붙은 멤버 변수는 인스턴스와 무관하게 클래스에 바로 접근해서 사용할 수 있고, 클래스 자체에 소속되어 있다. 따라서 클래스 변수라 한다.
    - 클래스 변수는 자바 프로그램을 시작할 때 딱 1개가 만들어진다. 인스턴스와는 다르게 보통 여러곳에서 공 유하는 목적으로 사용된다.

### **변수와 생명주기**

**지역 변수(매개변수 포함)**

지역 변수는 `스택 영역`에 있는 `스택 프레임 안에 보관`된다. 메서드가 종료되면 스택 프레임도 제거 되는데 이때 해당 스택 프레임에 포함된 지역 변수도 함께 제거된다. 따라서 지역 변수는 생존 주기가
짧다.

**인스턴스 변수**

`인스턴스에 있는 멤버 변수`를 `인스턴스 변수`라 한다. 인스턴스 변수는 힙 영역을 사용한다. 힙 영
역은 GC(가비지 컬렉션)가 발생하기 전까지는 생존하기 때문에 보통 지역 변수보다 생존 주기가 길다.

**클래스 변수**

클래스 변수는 `메서드 영역의 static 영역에 보관되는 변수`이다. `메서드 영역은 프로그램 전체에서 사용하는 공용 공간`이다. 클래스 변수는 해당 클래스가 JVM에 로딩 되는 순간 생성된다. 그리고 JVM이 종료될 때 까지 생명주기가 이어진다. 따라서 가장 긴 생명주기를 가진다.

⭐️ `static` 이 정적이라는 이유는 바로 여기에 있다. 힙 영역에 생성되는 인스턴스 변수는 동적으로 생성되고, 제거된다. 반면에 `static` 인 정적 변수는 거의 프로그램 실행 시점에 딱 만들어지고, 프로그램 종료 시점에 제거된다. 정적 변수 는 이름 그대로 정적이다.

### 정적 변수 접근 법

```java
//추가
//인스턴스를 통한 접근
Data3 data4 = new Data3("D"); 
System.out.println(data4.count);

//클래스를 통합 접근 
System.out.println(Data3.count);
```

둘의 차이는 없다. 둘다 결과적으로 정적 변수에 접근한다.

**인스턴스를 통한 접근** `data4.count`

정적 변수의 경우 인스턴스를 통한 접근은 추천하지 않는다. 왜냐하면 코드를 읽을 때 마치 인스턴스 변수에 접근하는 것 처럼 오해할 수 있기 때문이다.

**클래스를 통한 접근** `Data3.count`

정적 변수는 클래스에서 공용으로 관리하기 때문에 클래스를 통해서 접근하는 것이 더 명확하다. 따라서 정적 변수에 접
근할 때는 클래스를 통해서 접근하자.

## Static 메서드

```java
package static2;
public class DecoUtil1 {
    public String deco(String str) {
        String result = "*" + str + "*";
        return result;
		} 
}

```

앞서 개발한 `deco()` 메서드를 호출하기 위해서는 `DecoUtil1` 의 인스턴스를 먼저 생성해야 한다. 그런데 `deco()` 라는 기능은 멤버 변수도 없고, 단순히 기능만 제공할 뿐이다. 

인스턴스가 필요한 이유는 멤버 변수(인스턴스 변수)등을 사용하는 목적이 큰데, 이 메서드는 사용하는 인스턴스 변수도 없고 단순히 기능만 제공한다.

`DecoUtil2.deco(s)` 코드를 보자.

`static` 이붙은정적메서드는객체생성없이클래스명+ `.` (dot)+메서드명으로 바로 호출할 수 있다.

⭐️ 정적 메서드 덕분에 **불필요한 객체 생성 없이 편리하게 메서드를 사용**했다. → 정적 메서드를 사용하는 이유

**클래스 메서드**

메서드 앞에도 `static` 을 붙일 수 있다. 이것을 **정적 메서드** 또는 **클래스 메서드**라 한다. 정적 메서드라는 용어는 `static` 이 정적이라는 뜻이기 때문이고, 클래스 메서드라는 용어는 인스턴스 생성 없이 마치 클래스에 있는 메서드를 바로 호출하는 것 처럼 느껴지기 때문이다.

**인스턴스 메서드**

`static` 이 붙지 않은 메서드는 인스턴스를 생성해야 호출할 수 있다. 이것을 **인스턴스 메서드**라 한다.

### 정적 메서드 사용법

정적 메서드는 객체 생성없이 클래스에 있는 메서드를 바로 호출할 수 있다는 장점이 있다.
하지만 정적 메서드는 언제나 사용할 수 있는 것이 아니다.

```java
package static2;

public class DecoData {

    private int instanceValue;
    private static int staticValue;

    public static void staticCall() {
        // 인스턴스 변수 접근, compile error
        //instanceValue++; 
        // 인스턴스 메서드 접근, compile error
        //instanceMethod(); 

        staticValue++; //정적 변수 접근
        staticMethod(); //정적 메서드 접근
    }

    public void instanceCall() {
        instanceValue++; //인스턴스 변수 접근
        instanceMethod(); //인스턴스 메서드 접근

        staticValue++; //정적 변수 접근
        staticMethod(); //정적 메서드 접근
    }

    private void instanceMethod() {
        System.out.println("instanceValue=" + instanceValue);
    }

    private static void staticMethod() {
        System.out.println("staticValue=" + staticValue);
    }
}
```

- `static` 메서드는 `static` 만 사용할 수 있다.
    - 클래스 내부의 기능을 사용할 때, 정적 메서드는 `static` 이 붙은 ****정적 메서드나 정적 변수만 사용할 수 있다.**
    - 클래스 내부의 기능을 사용할 때, 정적 메서드는 인스턴스 변수나, 인스턴스 메서드를 사용할 수 없다.
- 반대로 모든 곳에서 `static` 을 호출할 수 있다.
    - 정적 메서드는 공용 기능이다. 따라서 접근 제어자만 허락한다면 클래스를 통해 모든 곳에서 `static` 을 호출할 수 있다.

- `instanceValue` 는 인스턴스 변수이다.
- `staticValue` 는 정적 변수(클래스 변수)이다.
- `instanceMethod()` 는 인스턴스 메서드이다.
- `staticMethod()` 는 정적 메서드(클래스 메서드)이다.

- `static void staticCall` 에서 `instaceValue` 는 인스턴스가 생성되고, 힙 영영에 생성되어야지 접근이 가능하다.
- `static void instanceCall` 에서는 모두 접근 가능하다.

```java
public static void staticCall() {
    //instanceValue++; //인스턴스 변수 접근, compile error
    //instanceMethod(); //인스턴스 메서드 접근, compile error

    staticValue++; //정적 변수 접근
    staticMethod(); //정적 메서드 접근
}
```

`staticCall()` , 이 메서드는 정적 메서드이다.

따라서 `static` 만 사용할 수 있다. 정적 변수, 정적 메서드에는 접근할 수 있지만, `static` 이 없는 인스턴스 변수나 인스턴스 메서드에 접근하면 **컴파일 오류가 발생**한다.

	

```java
public void instanceCall() {
    instanceValue++; //인스턴스 변수 접근
    instanceMethod(); //인스턴스 메서드 접근

    staticValue++; //정적 변수 접근
    staticMethod(); //정적 메서드 접근
}
```

`instanceCall()` , 이 메서드는 인스턴스 메서드이다. 

모든 곳에서 공용인 `static` 을 호출할 수 있다. 따라서 정적 변수, 정적 메서드에
접근할 수 있다. 물론 인스턴스 변수, 인스턴스 메서드에도 접근할 수 있다.

<aside>
💡

**정적 메서드가 인스턴스의 기능을 사용할 수 없는 이유**

정적 메서드는 클래스의 이름을 통해 바로 호출할 수 있다. 그래서 인스턴스처럼 참조값의 개념이 없다.
특정 인스턴스의 기능을 사용하려면 참조값을 알아야 하는데, 정적 메서드는 참조값 없이 호출한다. 따라서 정적 메서드 내부에서 인스턴스 변수나 인스턴스 메서드를 사용할 수 없다.

</aside>

## 멤버 메서드 종류

인스턴스 메서드 → `static` 이 붙지 않은 메서드

클래스 메서드 → `static` 이 붙은 메서드

클래스 메서드, 정적 메서드, `static` 메서드 등으로 부른다.

`static` 이 붙지 않은 멤버 메서드는 인스턴스를 생성해야 사용할 수 있고, 인스턴스에 소속되어 있다. 따라서 인스턴 스 메서드라 한다. `static` 이 붙은 멤버 메서드는 인스턴스와 무관하게 클래스에 바로 접근해서 사용할 수 있고, 클래스 자체에 소속되어 있다. 따라스 클래스 메서드라 한다.

### 정적 메서드 활용

정적 메서드는 `객체 생성이 필요 없이 메서드의 호출만으로 필요한 기능을 수행할 때 주로 사용`한다.
예를 들어 간단한 메서드 하나로 끝나는 유틸리티성 메서드에 자주 사용한다. 수학의 여러가지 기능을 담은 클래스를 만들 수 있는데, 이 경우 `인스턴스 변수 없이 입력한 값을 계산하고 반환하는 것이 대부분`이다. 이럴 때 정적 메서드를 사용해서 유틸리티성 메서드를 만들면 좋다.

```java
//인스턴스를 통한 접근
DecoData data3 = new DecoData();
data3.staticCall();

//클래스를 통한 접근
DecoData.staticCall();
```

**인스턴스를 통한 접근** `data3.staticCall()`

정적 메서드의 경우 인스턴스를 통한 접근은 추천하지 않는다. 왜냐하면 코드를 읽을 때 마치 인스턴스 메서드에 접근하 는 것 처럼 오해할 수 있기 때문이다.

**클래스를 통한 접근**  `DecoData.staticCall()`

정적 메서드는 클래스에서 공용으로 관리하기 때문에 클래스를 통해서 접근하는 것이 더 명확하다. 따라서 정적 메서드 에 접근할 때는 클래스를 통해서 접근하자.

### static import

정적 메서드를 사용할 때 해당 메서드를 다음과 같이 자주 호출해야 한다면 `static import` 기능을 고려하자. 

특정 클래스의 정적 메서드 하나만 적용하려면 다음과 같이 생략할 메서드 명을 적어주면 된다.

`import static static2.DecoData.staticCall;`

특정 클래스의 모든 정적 메서드에 적용하려면 다음과 같이 `*` 을 사용하면 된다. 

`import static static2.DecoData.*;`

참고로 `import static` 은 정적 메서드 뿐만 아니라 정적 변수에도 사용할 수 있다.

AS-IS

```java
 DecoData.staticCall();
```

TO-BE

```java
  import static static2.DecoData.staticCall;
  
  ..
  ..
  
  // 클래스 명을 적지 않아도 사용 가능.
  staticCall();
```

## main() **메서드는 정적 메서드**

인스턴스 생성 없이 실행하는 가장 대표적인 메서드가 바로 `main()` 메서드이다. `main()` 메서드는 프로그램을 시작하는 시작점이 되는데, 생각해보면 객체를 생성하지 않아도 `main()` 메서드가 작 동했다. 이것은 `main()` 메서드가 `static` 이기 때문이다.

**정적 메서드는 정적 메서드만 호출할 수 있다.** 따라서 정적 메서드인 `main()` 이 호출하는 메서드에는 정적 메서드를 사용했다. 

물론 더 정확히 말하자면 정적 메서드는 같은 클래스 내부에서 정적 메서드만 호출할 수 있다. 따라서 정적 메서드인 main() 메서드가 같은 클래스에서 호출하는 메서드도 정적 메서드로 선언해서 사용했다.