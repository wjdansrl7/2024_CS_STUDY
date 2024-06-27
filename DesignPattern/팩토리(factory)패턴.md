# 팩토리 패턴

## 심플 팩토리 패턴

심플 팩토리 패턴은 팩토리 패턴으로 여겨지지는 않지만 `팩토리 메서드 패턴`과 `추상 팩토리 패턴`의 베이스가 되는 패턴이다.

심플 팩토리 패턴은 `객체 생성 클래스`를 따로 두는 것을 의미한다.  
클라이언트에서 객체를 호출할 때, 생성자에 직접 의존하고 있다면 나중에 변경될 때 수정되어야 하는 코드가 많이 발생하게 된다.  
따라서 생성자 호출을 팩토리 클래스에서 담당하고 클라이언트는 팩토리 클래스에만 의존하면 된다.

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

하지만 이러한 구조의 경우 변경에 닫혀 있어야 한다는 **OCP 원칙을 위배**한다.  
새로운 클래스의 생성자가 필요하게 된다면, switch 문에 해당 클래스를 추가하는 과정이 들어가게 된다.  
즉 **Factory 코드를 수정해야하는 문제**가 존재한다.

---

## 팩토리 메서드 패턴

`팩토리 메서드 패턴`은 **객체를 생성할 때 어떤 클래스의 인스턴스를 만들 지 서브 클래스에서 결정**하게 한다.  
즉 부모 추상 클래스는 인터페이스에만 의존하고 객체의 생성을 서브 클래스에게 위임한다.  
이러한 구조는 `심플 팩토리 패턴`과는 달리 새로운 구현 클래스가 추가되어도 **기존 Factory의 수정 없이 새로운 생성 기능을 추가**할 수 있다.

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

```java
public class client {

    public static void main(String[] args) {
        UserFactory userFactory = new NaverUserFactory();
        User user = userFactory.newInstance();
    }
}
```

심플 팩토리 메서드에서는 새로운 객체가 추가된다면, 팩토리 메서드 확장을 위한 코드 수정이 필요했다.  
하지만 팩토리 메서드 패턴을 활용한다면 팩토리 메서드를 수정할 필요가 없다. 즉, 확장에 용이하다는 장점을 가지고 있다.

---

## 추상 팩토리 패턴

`팩토리 메서드 패턴`이 어떤 객체를 생성할 지에 집중한다면 `추상 팩토리 패턴과`는 연관된 객체들을 모아두는 것에 집중한다.

```java
public interface StaffFactory {

    Manager createManager();

    Player createPlayer();
}

public class SoccerStaffFactory implements StaffFactory {

    @Override
    public Manager createManager() {
        return new SoccerManager();
    }

    @Override
    public Player createPlayer() {
        return new SoccerPlayer();
    }
}

public class TennisStaffFactory implements StaffFactory {

    @Override
    public Manager createManager() {
        return new TennisManager();
    }

    @Override
    public Player createPlayer() {
        return new TennisPlayer();
    }
}
```

다른 예시들에서는 StaffFactory 대신에 FactoryOfFactory라는 표현을 쓰기도 한다.  
즉, 연관된 팩토리들 끼리 묶어두는 특징이 존재한다.
