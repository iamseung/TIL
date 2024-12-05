## 패키지란

매우 많은 클래스가 등장하면서 관련 있는 기능들을 분류해서 관리하고 싶을 것이다. 컴퓨터는 보통 파일을 분류하기 위해 폴더, `디렉토리`라는 개념을 제공한다. 자바도 이런 개념을 제공하는데, 이것이 바
로 `패키지`이다.

### 패키지 사용

```java
package pack;

public class Data {
    public Data() {
		System.out.println("패키지 pack Data 생성"); }
}

```

- **패키지를 사용하는 경우 항상 코드 첫줄에** `package pack` **과 같이 패키지 이름을 적어주어야 한다.**
- 여기서는 `pack` 패키지에 `Data` 클래스를 만들었다.
- 이후에 `Data` 인스턴스가 생성되면 생성자를 통해 정보를 출력한다.

**`사용자와 같은 위치` →** `PackageMain1` 과 `Data` 는 같은 `pack` 이라는 패키지에 소속되어 있다. 이렇게 같은 패키지에 있는 경우에는 패키지 경로를 생략해도 된다.

**`사용자와 다른 위치` →**  `PackageMain1` 과 `User` 는 서로 다른 패키지다. 이렇게 패키지가 다르면 `pack.a.User` 와 같이 패키지 전체 경로를 포함해서 클래스를 적어주어야 한다.

## import

코드에서 첫줄에는 `package` 를 사용하고, 다음 줄에는 `import` 를 사용할 수 있다.
`import` 를 사용하면 다른 패키지에 있는 클래스를 가져와서 사용할 수 있다.
`import` 를 사용한 덕분에 코드에서는 패키지 명을 생략하고 클래스 이름만 적을 수 있다.

`pack.a` 라는 class 를 여러개 사용할 경우엔, `import pack.a.*` 로 표기해서 사용할 수 있다.

### 중복된 패키지 명

```java
package pack;

import pack.a.User;

public class PackageMain3 {
    public static void main(String[] args) {
        User userA = new User();
        pack.b.User userB = new pack.b.User();
		} 
}
```

같은 이름의 클래스가 있다면 `import` 는 둘중 하나만 선택할 수 있다. 이때는 자주 사용하는 클래스를 `import` 하고 나머지를 패키지를 포함한 전체 경로를 적어주면 된다. 물론 둘다 전체 경로를 적어준다면 `import` 를 사용하지 않아도 된다.

### 패키지 규칙

- 패키지의 이름과 위치는 폴더(디렉토리) 위치와 같아야 한다. (필수)
- 패키지 이름은 `모두 소문자`를 사용한다. (관례)
- 패키지 이름의 앞 부분에는 일반적으로 회사의 도메인 이름을 거꾸로 사용한다. 예를 들어,  `com.company.myapp` 과 같이 사용한다. (관례)
    - 이 부분은 필수는 아니다. 하지만 수 많은 외부 라이브러리가 함께 사용되면 같은 패키지에 같은 클래스 이름이 존재할 수도 있다. 이렇게 도메인 이름을 거꾸로 사용하면 이런 문제를 방지할 수 있다.
    - 내가 오픈소스나 라이브러리를 만들어서 외부에 제공한다면 꼭 지키는 것이 좋다.
    - 내가 만든 애플리케이션을 다른 곳에 공유하지 않고, 직접 배포한다면 보통 문제가 되지 않는다.