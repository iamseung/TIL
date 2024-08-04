## Topic 2: 코어 자바

코어 자바는 방대한 주제이며 면접관은 이러한 주제를 반드시 물어봅니다.

코어 자바는 자바 개발자에게 기본적인 것으로 여겨지므로 이 부분에 대해 철저한 답을 공부하세요. 특정한 프레임워크를 모르는 것이 문제가 되지는 않지만, 코어 자바에 대한 지식이 부족하면 문제가 될 수 있습니다.

 
### [토픽]

- String/Hashcode-Equal 메소드

- Immutability

- OOPS 개념

- Serialization

- Collection Framework/concurrent collection

- 예외 처리

- 멀티스레드/스레드풀

- 자바 메모리(메모리 각 영역에 객체, 메소드 및 변수를 저장하는 법)

- 가비지 컬렉션(가비지가 객체를 수집하는 방법, 사용하는 알고리즘)

### [질문]

- ThreadPoolExecutor는 어떻게 동작하나요?

https://leeyh0216.github.io/posts/truth_of_threadpoolexecutor/

- 커스텀 불변 클래스를 어떻게 만드나요? 자바에서 불변 클래스의 예는 무엇인가요?


자바에서 불변 클래스(immutable class)를 만드는 것은 객체가 한 번 생성되면 그 상태가 변경되지 않도록 클래스를 설계하는 것을 말한다. 이러한 클래스는 안전하게 공유할 수 있으며, 멀티스레딩 환경에서 동기화 문제 없이 사용할 수 있는 장점이 있다.

    불변 클래스를 만드는 방법

	1.	클래스 선언을 final로 지정
    : 클래스를 final로 선언하면 상속이 불가능하게 되어, 메소드 오버라이딩을 통한 변경을 막을 수 있다.
	
    2.	모든 필드를 private과 final로 선언
    : 필드가 private이면 외부에서 직접 접근할 수 없고, final로 선언하면 값이 한 번 할당된 후에는 변경할 수 없다.
	
    3.	외부에서 접근 가능한 수정자(mutator) 메소드 제공하지 않기
    : 객체의 상태를 변경할 수 있는 setter와 같은 메소드를 제공하지 않아야 한다.

	4.	생성자를 통해 모든 값을 초기화
    : 모든 필드는 생성자를 통해서만 값을 할당받으며, 객체 생성 후에는 상태가 변경되지 않아야 한다.

	5.	변경 가능한 객체를 필드로 사용하는 경우 방어적 복사(defensive copy) 사용
    : 필드 타입이 변경 가능한 객체(예: Date, 컬렉션 등)일 경우, 해당 필드의 직접 반환을 피하고, 복사본을 반환하여 외부에서 객체의 상태를 변경할 수 없도록 해야 한다.

자바에서의 불변 클래스 예

String 클래스:

	•	String 클래스는 자바에서 가장 널리 알려진 불변 클래스의 예다.
	•	String 객체가 한 번 생성되면 그 내용을 변경할 수 없다.
	•	String 클래스는 내부적으로 문자 배열을 사용하지만, 이 배열은 private이며 final로 선언되어 있어서 외부에서 접근하거나 변경할 수 없다.
	•	문자열을 수정하는 모든 메소드(예: substring, replace)는 새로운 String 객체를 반환한다.

불변 클래스의 실제 구현 예시

    public final class ImmutablePerson {
        private final String name;
        private final int age;

        public ImmutablePerson(String name, int age) {
            this.name = name;
            this.age = age;
        }

        public String getName() {
            return name;
        }

        public int getAge() {
            return age;
        }
    }

위 예시에서 ImmutablePerson 클래스는 불변이다. name과 age 필드는 private과 final로 선언되어 있으며, 이들은 외부에서 접근할 수 없고, 객체 생성 시에만 값이 설정된다. 변경자 메소드가 없으므로 이 객체의 상태는 생성 이후 변경될 수 없다.

불변 클래스를 올바르게 구현하면, 해당 객체들은 안전하게 공유할 수 있으며, 불변성이 필요한 여러 디자인 패턴과 기능에서 유용하게 사용된다.

- hasCode()와 equals()가 무엇인가요? map에서 객체를 키로 사용하면 어떻게 되마요? 올바르게 사용하는 방법은 무엇인가요?

- 깊은 복사와 얕은 복사가 무엇인가요?

- CompletableFuture가 무엇인가요?

- 최신 자바 메모리 모델이 무엇인가요?

- concurrent collection이 무엇인가요?

- HashMap, ArrayList 및 LinkedList의 시간/공간 복잡도를 말해주세요

- Arrays.sort()와 Collections.sort()에서 사용되는 알고리즘은 무엇인가요?

- 자바에서 커스텀 어노테이션을 어떻게 만드나요?

- HashMap과 HashSet은 내부적으로 어떻게 작동되나요?

- String의 join() 메소드의 용도는 무엇인가요?