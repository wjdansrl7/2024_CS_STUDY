### 싱글톤(Singleton) 패턴

1. 싱글톤 패턴 사용시 얻을 수 있는 장점을 설명해주세요(사용하는 이유).
2. 싱글톤 패턴 사용시 발생할 수 있는 문제점을 설명해주세요.

### 어댑터(Adapter) 패턴

1. 어댑터 패턴이 무엇인지 설명해주세요.
2. 어댑터 패턴이 쓰인 예제를 아무거나 하나 알려주세요.

### 상태(State) 패턴

1. 상태 패턴이 무엇인지 설명해주세요.
2. 상태 패턴을 적용할 수 있는 상황을 아무거나 하나 말해주세요.

### 복합(Compound) 패턴

1. 복합 패턴이 무엇인지 설명해주세요.
2. MVC패턴에서 Model, View, Contoller 각각에 사용된 패턴을 작성해주세요.

### 컴포지트 패턴

1. 컴포지트 패턴은 OO - XX 계층 구조를 쉽게 구현할 수 있다. (OO와 XX에 들어갈 단어를 쓰세요)
2. 컴포지트 패턴이란 무엇인가? 

### 템플릿 메소드 패턴

1. 템플릿 메소드 패턴이란 무엇인가?
2. 템플릿 메소드의 단점이 아닌것은?
    1. 중복코드가 생길 수 있다.
    2. 추상 메소드가 많아지면서 클래스 관리가 복잡해진다.
    3. 클래스간의 관계와 코드가 꼬일 수도 있다.

### 퍼사드 패턴

1. 퍼사드 패턴이란 무엇인가?
2. 퍼사드패턴의 장점 하나를 쓰세요

### 데코레이터 패턴

1. 데코레이터 패턴은 객체의 결합을 통해 OO을 동적으로 유연하게 확장할 수 있게 해주는 패턴입니다. 여기서 OO는 무엇일까요?
2. 데코레이터 패턴의 장점이 아닌 것은?
    1. 객체를 여러 데코레이터로 래핑하여 여러 동작을 결합할 수 있다.
    2. 컴파일 타임이 아닌 런타임에 동적으로 기능을 변경할 수 있다.
    3. 클라이언트 코드 수정 없이 기능 확장이 필요하면 장식자 클래스를 추가하면 되니 개방 폐쇄 원칙을 준수한다.
    4. 만일 장식자 일부를 제거하고 싶다면, Wrapper 스택에서 특정 wrapper만 제거하면 되므로 쉽게 제거가 가능하다.

### 반복자 패턴

1. 일련의 데이터 집합에 대하여 순차적인 접근(순회)을 지원하는 패턴은?
2. 반복자 패턴의 단점은?

### 전략 패턴
1. 전략 패턴이란 무엇인가?
2. 전략 패턴의 적절한 사용 시기 중에 알고리즘 코드가 노출되어서는 안되는 데이터에 액세스 하거나 데이터를 활용할 때 사용하면 적절하다고 하는데, 해당 내용은 객체 지향 프로그래밍의 특징 중 무엇에 해당하는가?

### 커맨드 패턴
1. 커맨드 패턴은 실행될 기능을 캡슐화함으로써 주어진 여러 기능을 실행할 수 있는 OOOO이 높은 클래스로 설계하는 패턴이며, 객체의 행위(OOO)를 클래스로 만들어 캡슐화하는 패턴인데, 해당 빈칸에 해당하는 단어들을 작성해주세요.
2. 커맨드 패턴의 장점을 얘기해주세요.

### 팩토리 패턴

Q1. 다음 코드는 객체지향의 어떤 원칙을 위배하고 있나요?

```java
public interface Pet {

    enum Type {
        Cat, Dog
    }
}

public class PetFactory {

    public Pet generatePet(Pet.Type petType) {
        return switch (petType) {
            case CAT -> new Cat();
            case DOG -> new Dog();
            default -> throw new IllegalArgumentException();
        };
    }
}
```




Q2. NaverUser를 팩토리 패턴을 이용해서 만들고 싶을때, client 에서는 어떤 식으로 만들 수 있나요. 코드 작성!

```java
public interface User {

    void signup();
}

public class NaverUser implements User {

    @Override
    public void signup() {
        System.out.println("네이버 아이디로 가입");
    }
}

public abstract class UserFactory {

    public User newInstance() {
        User user = createUser();
        user.signup();
        return user;
    }

    protected abstract User createUser();
}

public class NaverUserFactory extends UserFactory {

    @Override
    protected User createUser() {
        return new NaverUser();
    }
}
```

``` java
public class client {

    public static void main(String[] args) {
        // hint : 팩토리 패턴을 이용한다. 팩토리를 만저 만들고 해당 팩토리에서 객체를 만들면 됨.
        // TODO
    }
}
```