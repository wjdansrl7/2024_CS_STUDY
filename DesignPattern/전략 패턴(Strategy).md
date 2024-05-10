# 전략 패턴(Strategy)

# 전략 패턴(Strategy pattern)

- 객체들이 할 수 있는 행위 각각에 대해 전략 클래스를 생성하고, 유사한 행위들을 캡슐화 하는 인터페이스를 정의하여, 객체의 행위를 동적으로 바꾸고 싶은 경우 직접 행위를 수정하지 않고 전략을 바꿔주기만 함으로써 행위를 유연하게 확장하는 방법
- ⇒ 객체가 할 수 있는 행위들 각각을 전략으로 만들어 놓고, 동적으로 행위의 수정이 필요한 경우, 전략을 바꾸는 것만으로 행위의 수정이 가능하도록 만든 패턴
- 알고리즘 변형이 빈번하게 필요한 경우에 적합한 패턴


![Untitled](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/719bed0f-dfd3-42c9-8be5-64ee798e0011)

![Untitled 1](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/a8200318-a16d-47de-8329-8d981fe10dd9)

![Untitled 2](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/78a89d4f-3b77-49c9-93e9-b152d751c5fc)

⇒ OOP 기술들의 총 집합 버전

# 해당 방식의 문제점

- OCP 위배
- 시스템이 커져서 확장이 될 경우 메서드의 중복 문제 발생

# 전략 패턴 사용 시기

- 전략 알고리즘의 여러 버전 또는 변형이 필요할 때 클래스화를 통해 관리
- 알고리즘 코드가 노출되어서는 안 되는 데이터에 액세스 하거나 데이터를 활용할 때 (캡슐화)
- 알고리즘의 동작이 런타임에 실시간으로 교체 되어야 할때

# 전략 패턴 주의점

- 알고리즘이 많아질수록 관리해야할 객체의 수가 늘어난다는 단점이 있다.
- 만일 어플리케이션 특성이 알고리즘이 많지 않고 자주 변경되지 않는다면, 새로운 클래스와 인터페이스를 만들어 프로그램을 복잡하게 만들 이유가 없다.
- 개발자는 적절한 전략을 선택하기 위해 전략 간의 차이점을 파악하고 있어야 한다. (복잡도 ↑)

## 전략 패턴 사용 X

![Untitled 3](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/1ed3dd83-7921-4542-a36d-0bcba31acf5f)

```java
public interface Movable {
    public void move();
}
```

```java
public class Train implements Movable{
    public void move(){
        System.out.println("선로를 통해 이동");
    }
}
```

```java
public class Bus implements Movable{
    public void move(){
        System.out.println("도로를 통해 이동");
    }
}
```

```java
public class Client {
    public static void main(String args[]){
        Movable train = new Train();
        Movable bus = new Bus();

        train.move();
        bus.move();
    }
}
```

⇒ 만약 버스의 move() 메서드를 변경해야한다면?

```java
public void move(){
    System.out.println("선로를 따라 이동");
}
```

⇒ **SOLID의 원칙 중 OCP( Open-Closed Principle )에 위배**

⇒ 시스템이 확장이 되었을 때 유지보수를 어렵게 함

## 전략 패턴 사용 O

⇒ 이동 방식을 직접 메서드로 구현하지 않고, 어떻게 움직일 것인지에 대한 전략을 설정하여, 그 전략의 움직임 방식을 사용하여 움직이도록 함.

![Untitled 4](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/1e2e80d1-3cb5-42bb-8e60-2dc7bec174b8)

```java
public interface MovableStrategy {
    public void move();
}
```

```java
public class RailLoadStrategy implements MovableStrategy{
    public void move(){
        System.out.println("선로를 통해 이동");
    }
}
```

```java
public class LoadStrategy implements MovableStrategy{
    public void move() {
        System.out.println("도로를 통해 이동");
    }
}
```

![Untitled 5](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/68eb7e24-722b-4d22-8870-75ec93a71865)

```java
public class Moving {
    private MovableStrategy movableStrategy;

    public void move(){
        movableStrategy.move();
    }

    public void setMovableStrategy(MovableStrategy movableStrategy){
        this.movableStrategy = movableStrategy;
    }
}
```

```java
public class Bus extends Moving{

}
```

```java
public class Train extends Moving{

}
```

```java
public class Client {
    public static void main(String args[]){
        Moving train = new Train();
        Moving bus = new Bus();

        /*
            기존의 기차와 버스의 이동 방식
            1) 기차 - 선로
            2) 버스 - 도로
         */
        train.setMovableStrategy(new RailLoadStrategy());
        bus.setMovableStrategy(new LoadStrategy());

        train.move();
        bus.move();

        /*
            선로를 따라 움직이는 버스가 개발
         */
        bus.setMovableStrategy(new RailLoadStrategy());
        bus.move();
    }
}
```

참고자료

[https://victorydntmd.tistory.com/292](https://victorydntmd.tistory.com/292)

[https://inpa.tistory.com/entry/GOF-💠-전략Strategy-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%A0%84%EB%9E%B5Strategy-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)
