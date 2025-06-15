### Java 버전별 특징

<details>
    <summary> 예비 답안 </summary>
    <br />

Java 8 : 람다, 인터페이스의 default method, stream api, Null 처리 Optional 등이 추가됨

Java 9 : 인터페이스 내에서 private 메서드 사용이 가능

Java 10 : 타입 추론 변수 var 추가, 병렬 처리 GC, 개별 스레드로 분리된 Stop the world등이 추가됐다.

- 기존에는 Stop-The-World 가 발생하면 GC 를 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춘다. GC 작업을 완료한 이후에야 중단했던 작업을 다시 시작한다. 근데 이게 개별 쓰레드로 분리되어서 Stop-The-World 시간이 개선된것 같다.

Java  11 : 람다 파라미터로 var 사용, String 메서드 추가(strip(), isBlank(), lines(), repeat())

Java  12~14 : 스위치 표현식 개선 (표현식에서 값 반환 가능)

Java  17 : Sealed Class, Pattern Matching for Instanceof, Record 등

</details>

-----------------------

### JVM 구조

<details>
    <summary> 예비 답안 </summary>
    <br />

- `jvm` 은 자바 가상 머신입니다. 
    - 자바와 운영체제 사이에서 중개자 역할을 수행하며 자바가 운영체제에 구애받지 않고 프로그램을 실행할 수 있도록 도와줍니다.
- JVM 의 구조는 
    - JVM 내로 클래스 파일을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈인 `Class Loader`
    - 클래스 로더를 통해 JVM 내의 Runtime Data Area 에 배치된 바이트 코드들을 명령어 단위로 실행하는 `Execution Engine`
    - 힙 메모리 영역에 생성된 객체들 중에서 참조되지 않은 객체들(Unreachable)을 탐색 후, 제거하는 역할을 하는 `Garbage Collector`
    - JVM 의 메모리 영역으로, 자바 애플리케이션을 실행할 때 사용되는 데이터들을 적재하는 영역인 `Runtime Data Area`
    
```
jvm 은 크게 jvm 내로 클래스 파일을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈인 클래스 로더(Class Loader), jvm 내의 Runtime Data Area에 배치된 코드들을 명령어 단위로 실행하는 실행 엔진(Execution Engine), JVM 의 메모리 영역으로, 자바 애플리 케이션을 실행할 때 사용되는 데이터들을 적재하는 런타임 데이터 영역(Runtime Data Area), 힙 메모리 영역에 더 이상 참조되지 않는 객체들을 탐색 후 제거하는 역할을 하는 갈비지 컬렉터(Garbage Collector)으로 구성됩니다.
```
</details>

-----------------------

### JVM 의 메모리 구조

<details>
    <summary> 예비 답안 </summary>
    <br />

- JVM 의 메모리 구조는 모든 쓰레드에서 공유되는 Method 영역과 Heap 영역, 나머지 영역인 PC Register, Stack, Native Method Stack 영역으로 구분할 수 있습니다.

    - `Method 영역` → 모든 스레드가 공유하는 영역으로, 클래스/인터페이스/메소드/필드/static 변수 등의 바이트 코드를 보관합니다.
    - `Heap 영역` → 모든 스레드가 공유하는 영역으로, new 키워드로 생성된 모든 객체와 배열이 저장되는 영역입니다. 또한 메소드 영역에 로드된 클래스만 생성이 가능하고 GC 가 참조되지 않는 메모리를 확인하고 제거하는 영역입니다.
    - `PC Register` → 스레드가 시작될 때 생성되며, 스레드가 어떤 부분을 무슨 명령어로 실행해야 할 지에 대한 기록을 하는 부분으로, JVM 명령의 주소를 가집니다.
    - `Stack` → 메서드 호출 시마다 각각의 스택 프레임이 생성됩니다. 그리고 메서드 안에서 사용되는 값들을 저장하고, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장하며 메서드 수행이 끝나면 프레임별로 됩니다.
    - `Native Method Stack` → 자바 외의 언어로 작성된 네이티브 코드를 위한 스택입니다.

```
JVM 의 메모리 구조는 모든 쓰레드에서 공유되는 Method 영역과 Heap 영역, 나머지 영역인 PC Register, Stack, Native Method Stack 영역으로 구분할 수 있습니다.
클래스, 인터페이스, 메소드, 필드, static 변수 등의 데이터를 보관하는 Method 영역, new 키워드로 생성된 모든 객체와 배열이 저장되는 영역인 Heap 영역, 쓰레드가 어떤 부분을 무슨 명령으로 실행해야할 지에 대한 기록을 하는 부분으로 현재 수행중인 JVM 명령의 주소를 갖는 PC Register 영역, 자바 외 언어로 작성된 네이티브 코드를 위한 메모리 영역인 Native method stack 영역, 메서드 호출 시마다 각각의 스택 프레임이 생성되며 메서드 안에서 사용되는 값들을 저장하고, 호출된 메서드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장하는 Stack 영역이 있습니다.
```

</details>

-----------------------

### Garbage Collector 이란 무엇인지

<details>
    <summary> 예비 답안 </summary>
    <br />

- GC, 가비지 컬렉션은 JVM의 Heap 영역에서 더 이상 참조되지 않는 객체를 일정 주기로 찾아내고 메모리를 회수하는 기능입니다.
- 프로그램이 사용하지 않는 메모리를 주기적으로 해제함으로써 애플리케이션의 안전성과 지속 가능성을 유지합니다.
- 힙 영역은 Young 과 Old Generation 영역으로 나뉘는데, 이 영역은 Minor GC 와 Full GC 를 판가름하는 대상입니다.
    - `Young Generation` 영역은 짧게 살아남는 메모리들이 존재하는 공간입니다. 모든 객체는 처음에는 Young Generation 에 생성되며, Young Generation 의 공간은 Old Generation 에 비해 상대적으로 적기 때문에 메모리 상의 객체를 찾아 제거하는데 적은 시간이 걸립니다. (Minor GC)
    - 새롭게 생성되는 객체는 Young Generation 영역 중 `Eden` 에서 생성되며, Eden 공간이 가득차면 MinorGC가 동작하여 생존한 객체가 증가된 age-bit과 함께 `Survivor0` 영역으로 넘어가게 됩니다.
    - 위의 과정을 통해 Young Generation 의 마지막 영역인 `Survivor1` 영역의 GC에서도 살아남는다면 Old Generation 영역으로 넘어가게 됩니다.
    - `Old Generation` 은 길게 살아남는 메모리들이 존재하는 공간입니다. Old Generation의 객체들은 처음에는 Young Generation 에 의해 시작되었으나, GC 과정 중에 제거되지 않은 경우 Old Generation로 이동합니다. (Major GC)
    - Old Generation 에서 발생하는 Major GC 는 매우 큰 공간이기 때문에 데이터를 지우는데 많은 시간이 소요되며, Major GC 가 발생하면 Thread 가 멈추고(Stop The World) Mark and Sweep 작업을 해야 해서 CPU에 부하를 줄 수 있습니다.

### Mark And Sweep
![poster](../image/jvm/ms.png)

Mark-Sweep 이란 다양한 GC에서 사용되는 객체를 솎아내는 내부 알고리즘입니다. 가비지 컬렉션이 동작하는 기초적인 청소 과정이라고 생각하면 됩니다.

원리는 가비지 컬렉션이 될 대상 객체를 `식별(Mark)`하고 `제거(Sweep)`하며 객체가 제거되어 파편화된 메모리 영역을 앞에서부터 `채워나가는 작업(Compaction)`을 수행하게 됩니다.

- Mark 과정 : 먼저 `Root Space` 로부터 그래프 순회를 통해 연결된 객체들을 찾아내어 각각 어떤 객체를 참조하고 있는지 찾아서 마킹합니다.
- Sweep 과정 : 참조하고 있지 않은 객체, 즉 Unreachable 객체들을 Heap 에서 제거합니다.
- Compact 과정 : Sweep 후에 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 압축합니다. (가비지 컬렉터 종류에 따라 하지 않는 경우도 존재합니다.)

⭐️ Mark And Sweep 방식은 루트로 부터 해당 객체에 접근이 가능한지가 해제의 기준이 됩니다. JVM GC에서의 Root Space는 `Heap 메모리 영역을 참조`하는 method area, static 변수, stack, native method stack 이 있습니다.

</details>

### Java Interface's default Method

<details>
    <summary> 예비 답안 </summary>
    <br />

- java 8 이 등장하면서 인터페이스 개념에 디폴트 메서드(default Method) 를 사용할 수 있게 되었습니다. 원래 기존의 인터페이스는 추상 메서드만 존재할 수 있었고 이를 상속받는 구현체에서 직접 해당 추상 메서드를 구현해야만 하는 상황이였습니다.

 ClassA, ClassB, ClassC 총 3개의 클래스가 InterfaceA를 구현하고 있습니다. 이때, 요구사항이 추가되면서 InterfaceA에 특정 추상 메서드 methodA를 추가해야된다고 생각해봅시다.

그러면 인터페이스 원칙에 의해 ClassA, ClassB, ClassC에 모두 methodA 를 구현 해야할 것입니다. 현재는 3개밖에 없지만 InterfaceA 를 상속받는 클래스가 10개가 넘어가는 상황에는 모두 구현해야 합니다.


```java
public interface Interface {
   // 추상 메서드 
    void abstractMethodA();
    void abstractMethodB();
    void abstractMethodC();

	// default 메서드
    default int defaultMethodA(){
    	...
    }
}
```
기존의 추상 메서드와 다른 점은
- 메서드 앞에 `default` 예약어를 붙여야 합니다.
- 구현부 `{}` 가 있어야 합니다.

# default Method 예시

```java
public interface PaymentProcessor {
    void process();
}

public class KakaoPayProcessor implements PaymentProcessor {

    @Override
    public void process() {
        System.out.println("Processing with KakaoPay");
    }
}

public class NaverPayProcessor implements PaymentProcessor {

    @Override
    public void process() {
        System.out.println("Processing with NaverPay");
    }
}

```
공용 결제 처리를 하는 PaymentProcessor 를 네이버, 카카오가 상속받은 코드입니다. 새로운 요구사항으로 메서드를 추가해야 하는 상황을 예시로 들어보겠습니다. 

```java
public interface PaymentProcessor {
    void process();
    String getPaymentMethodName(); // 새로운 추상 메서드 추가
}
```
예를 들어, 결제 방식에 대한 설명을 추가하기 위해 getPaymentMethodName() 메서드를 추가한다고 해봅시다. 그러면 기존의 모든 구현 클래스인 KakaoPayProcessor, NaverPayProcessor 등 모두 컴파일 오류가 발생합니다. 기존 클래스들의 변경이 불가피한 상태죠.

```java
public interface PaymentProcessor {
    void process();

    default String getPaymentMethodName() {
        return "Unknown Payment Method";
    }
}

public class KakaoPayProcessor implements PaymentProcessor {

    @Override
    public void process() {
        System.out.println("Processing with KakaoPay");
    }

    @Override
    public String getPaymentMethodName() {
        return "KakaoPayProcessor";
    }
}
```

이렇게 하면 새로운 구현체에서는 필요하면 오버라이딩하고, 기존 구현체는 변경하지 않아도 컴파일 오류 없이 동작하게 됩니다.

### default Method 의 장점
인터페이스에 추상 메서드를 추가하게 되면 모든 구현체에 구현을 해야 합니다. 이를 default method 를 사용하여 추가 변경을 막을 수 있습니다.
이로써 OCP 에서 확장에 개방되어 있고, 변경에 닫힌 코드를 설계할 수 있습니다.

### default Method 간의 충돌
default method를 사용하면 크게 2가지 충돌 상황이 발생할 수 있습니다.

1. 여러 인터페이스의 디폴트 메서드 간의 충돌
2. 디폴트 메서드와 상위 클래스의 메서드 간의 충돌

default method는 인터페이스를 구현한 클래스에서 코드를 구현할 필요가 없을 뿐이지, 구현을 할 수 없는 것이 아닙니다.

즉, 인터페이스를 구현하는 클래스에서 default method를 재정의할 수 있습니다.

따라서, 위와 같은 충돌 상황이 일어나는 클래스에서 defalt method를 재정의하면 충돌 상황을 해결할 수 있습니다.
    
</details>

-----------------------

### java load, unload

<details>
    <summary> 예비 답안 </summary>
    <br />

- JVM Load 는 클래스가 필요한 시점에 동적으로 클래스의 바이트 코드를 읽어 메모리에 할당하는 과정
- JVM Unload 는 클래스가 더 이상 사용되지 않아 메모리에서 클래스를 해제하는 과정
    
</details>

-----------------------

### new String() 을 사용한 문자열 선언

<details>
    <summary> 예비 답안 </summary>
    <br />

```java
String string1 = "abc";
String string2 = new String("abc");
```

위의 코드는 String class를 만드는 두가지 방법을 나타낸다. 두가지 방법은 보기에는 같은 결과가 나온다고 생각할 수 있지만 내부적으로는 다른 결과를 낸다. string1과 string2는 스트링 풀(String pool)에 있는 같은 객체를 바라보게 된다. 
<br> 반면에 new String()을 통해 생성한 string3 의 경우는 힙 메모리에 새로운 String 인스턴스를 만들어 관리를 하게 된다. 예시 코드를 작성하여 수행해보면 다음과 같은 결과가 나온다.

```java
public class StringTest {

	public static void main(String[] args) {
		String string1 = new String("abc");
		String string2 = new String("abc");

		System.out.println(string1 == string2); // false

		String string3 = "abc";
		String string4 = "abc";

		System.out.println(string3 == string4); // true
	}
}
```

위의 코드의 경우 new String 을 사용하여 새로운 인스턴스를 생성한 string1, string2의 경우는 서로 다른 주소값을 가르켜 false라는 결과를 반환한다. 반면에 스트링 풀의 주소만을 가르키며 생성한 string3, string4의 경우는 값이 같다는 결과가 나오게 된다.

| 구분 | 저장 위치 | 인스턴스 생성 여부 | 비교 결과 (==) |
| -- | -- | -- | -- |
| `new String("abc")` | Heap + (내부적으로 String Pool 참조) | 새 인스턴스 생성 | false |
| `"abc" 리터럴` | String Pool | Pool에 이미 있으면 재사용 |  true |

    
</details>

-----------------------

### 동일성과 동등성

<details>
    <summary> 예비 답안 </summary>
    <br />

- 동일성(==)은 객체가 참조하고 있는 주소 값을 비교하는 것입니다.
- 동등성(equals)는 equals를 통해 정의된 값에 따라 비교를 하는 것입니다.
- 객체들의 최상위 클래스 Object는 기본적으로 equals가 주소 값을 비교하는 동일성 체크와 동일하며 우리는 객체의 equals 재정의를 통해 내부 값이 같으면 두 객체가 동등하다고 판단할 수록 할 수 있다.
    
</details>

-----------------------

### Equals & HashCode는 왜 재정의를 해야할까?

<details>
    <summary> 예비 답안 </summary>
    <br />

객체들의 최상위 클래스 Object는 기본적으로 equals가 주소 값을 비교하는 동일성 체크와 동일하며 우리는 객체의 equals 재정의를 통해 내부 값이 같으면 두 객체가 논리적으로 동등하다고 판단할 수 있다.

그렇다면 HashCode는 왜 재정의를 해야할까?

`"Object의 명세서에는 equals(Object)가 두 객체를 같다고 판단했다면, 두 객체의 hashCode는 똑같은 값을 반환해야 한다."` 라는 조항이 존재합니다. 이를 위해 우리는 equals를 재정의할 때는 hashCode도 반드시 재정의해야 한다.
    
</details>

-----------------------

### StringBuilder, StringBuffer의 특징과 차이점에 대해 설명해주세요

<details>
    <summary> 예비 답안 </summary>
    <br />

- 둘 다 내부적으로 가변적인 char[]를 멤버 변수로 가집니다.
- 새로운 인스턴스를 생성하지 않고 char[]를 변경할 수 있어서 문자열을 여러번 연결하거나 변경할 때 사용하면 유용합니다.
- 출력은 나중에 toString() 메서드로 String반환을 해주면 됩니다.
- StringBuilder와 StringBuffer는 char[] (character buffer)를 갖는 공통점이 있으나 `StringBuffer는 multi-thread환경에서 동기화(synchronization)가 보장됩니다`.
- `그래서 single thread 프로그래밍의 경우는 StringBuilder사용을 권장하며 multi-thread환경에서는 StringBuffer를 사용을 권장한다.`
    
</details>

-----------------------

### String, Integer와 같은 클래스는 import 없이 어떻게 사용이 가능할까?

<details>
    <summary> 예비 답안 </summary>
    <br />

> 자바는 빌드를 하며 빌트인 패키지를 자동으로 import한다. String, Integer, System과 같은 클래스가 속해있는 java.lang은 해당 클래스에 해당하여 자동으로 import한다.
    
</details>

-----------------------

### Generic, 제너릭에 대해 설명해주세요.

<details>
    <summary> 예비 답안 </summary>
    <br />

- 제네릭은 클래스나 메소드에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법입니다.
- List와 같이 다양한 종류의 데이터를 관리하는 경우 데이터의 타입을 특정 타입으로 고정할 수 있다.

### Generic의 장점

- 제네릭을 사용하면 잘못된 타입이 들어올 수 있는 것을 컴파일 단계에서 방지할 수 있다.
- 특정 타입으로 제한함으로써 타입 안정성을 제공한다.
- 타입 체크와 형변환을 생략할 수 있으므로 코드가 간결해 진다.
    - 클래스 외부에서 타입을 지정해주기 때문에 따로 타입을 체크하고 변환해줄 필요가 없다. 즉, 관리하기가 편하다.
- 비슷한 기능을 지원하는 경우 코드의 재사용성이 높아진다.
    
</details>

-----------------------