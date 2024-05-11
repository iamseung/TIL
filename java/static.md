정적(static)이란 무엇인가요?

static은 공용으로 사용되는 변수 또는 메서드를 나타냅니다. 이는 여러 객체 간에 공유되며, 주로 중복되는 기능이나 데이터를 객체를 생성할 때마다 반복적으로 할당하는 것이 아니라 
클래스 단위로 관리하여 메모리 효율성을 높이기 위해 사용됩니다. 
예를 들어, 사람 객체를 만들 때 잠자기(), 먹기()와 같은 메서드들이 중복으로 들어가는 경우, 이를 클래스 내의 static으로 선언하여 객체를 생성할 때마다 반복적으로 메모리를 차지하지 않고 한 번만 할당하여 사용할 수 있습니다. 
이로써 코드의 효율성을 높일 수 있습니다.

Q1. 클래스 내의 static 메서드에서는 어떤 메모리에 속한 변수에 접근할 수 있을까요?
: static 메서드는 static 변수만 사용 할 수 있습니다. 그 이유는 static은 이미 메모리가 할당이 되어 있기 때문에 메모리 할당이 안되어 있는 클래스 내의 일반 변수 들은 사용 할 수 없습니다. 만약 static안에서 사용 하고 싶다면 파라미터로 변수를 받아서 사용해야 합니다. 

Q2. static의 장단점에 대해 말해주세요.
: Static 메모리 영역에 저장되어 고정된 메모리 영역을 사용하기 때문에 매번 인스턴스를 생성하며 낭비되는 메모리를 줄일 수 있다.(메모리 측면에서 효율적)
객체를 생성하지 않고 사용 가능하기 때문에 속도가 빠르다.

프로그램 종료시까지 메모리에 할당된 채로 존재한다. 우리가 만든 Class 는 프로그램 실행 시, Static 영역에 생성된다.
Garbage Collector 를 통해 수시로 관리받는 Heap 영역과 다르게 관리를 받지 않는다. 만약 프로그램에서 많은 Static 을 사용하게 되면 종료시까지 메모리가 할당된 채로 존재하므로 프로그램 퍼포먼스에 악영향을 주게 된다.

URL : https://seung-seok.tistory.com/entry/Java-%EC%A0%95%EC%A0%81static-%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80