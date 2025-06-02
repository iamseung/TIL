# 어노테이션이란

> 어노테이션이란 메타데이터를 코드에 추가하는 기능입니다. 클래스, 메서드, 변수 등 다양한 코드 요소에 부가 정보를 제공하며, 컴파일러나 프레임워크가 이 정보를 활용해 특정 동작을 수행하거나 자동 처리를 가능하게 합니다.

## 📖 어노테이션의 주요 목적 
1. 컴파일러 지시(컴파일 경고 제거 등)
2. 코드 자동 생성(롬복, 스프링)
3. 런타임 처리(리플렉션 기반 프레임워크 활용)
4. 소스 코드 문서화(javadoc 등)

## 어노테이션 사용 예시
```java
@Override
public String toString() {
    return "Hello";
}
```
- @Override 는 부모 클래스의 메서드를 재정의했다는 것을 명시하여, 잘못 작성 시 컴파일 오류를 발생시켜 준다.


## ✅ 자주 사용하는 기본 어노테이션
|어노테이션 | 설명 |
| -- | -- |
| @Override | 메서드 오버라이드 여부 확인 |
| @Deprecated | 해당 요소가 더 이상 사용되지 않음을 알림 |
| @SuppressWarnings | 컴파일러 경고를 무시할 때 사용 |
| @FunctionalInterface | 하나의 추상 메서드만 가지는 인터페이스임을 보장 (람다식 대상) |
| @SafeVarargs | 제네릭 가변인자 사용 시 경고 제거 |

## ✅ 메타 어노테이션 (어노테이션을 설명하는 어노테이션 )
|어노테이션 | 설명 |
| -- | -- |
| @Target | 어노테이션이 적용될 수 있는 위치 지정 (METHOD, TYPE, FIELD 등) |
| @Retention | 어노테이션이 유지되는 시점 (SOURCE, CLASS, RUNTIME) |
| @Documented | Javadoc 생성 시 어노테이션 포함 여부 |
| @Inherited | 하위 클래스가 상속할 수 있는 어노테이션 |

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    String value();
}
```
