# 데코레이터 패턴(decorator)

데코레이터 패턴(decorator)

- 객체의 결합을 통해 기능을 동적으로 유연하게 확장 할 수 있게 해주는 패턴

데코레이터 패턴 구조

![Untitled](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/e5cc57e8-9f2a-4a53-a234-28a1433bb743)

역할

- Component
    - 기본 기능을 뜻하는 ConcreteComponent와 추가 기능을 뜻하는 Decorator의 공통 기능을 정의
    - 즉, 클라이언트는 Component를 통해 실제 객체를 사용함
- ConcreteComponent
    - 기본 기능을 구현하는 클래스
- Decorator
    - 많은 수가 존재하는 구체적인 Decorator의 공통 기능을 제공
- ConcreteDecoratorA, ConcreteDecoratorB
    - Decorator의 하위 클래스로 기본 기능에 추가되는 개별적인 기능을 뜻함
    - ConcreteDecorator 클래스는 ConcreteComponent 객체에 대한 참조가 필요한데, 이는 Decorator 클래스에서 Component 클래스로의 ‘합성(composition) 관계’를 통해 표현됨

데코레이터 패턴 특징

패턴 사용 시기

- 객체 책임과 행동이 동적으로 상황에 따라 다양한 기능이 빈번하게 추가/삭제되는 경우
- 객체의 결합을 통해 기능이 생성될 수 있는 경우
- 객체를 사용하는 코드를 손상시키지 않고 런타임에 객체에 추가 동작을 할당할 수 있어야 하는 경우
- 상속을 통해 서브클래싱으로 객체의 동작을 확장하는 것이 어색하거나 불가능 할 때

패턴 장점

- 데코레이터를 사용하면 서브클래스를 만들때보다 훨씬 더 유연하게 기능을 확장할 수 있다.
- 객체를 여러 데코레이터로 래핑하여 여러 동작을 결합할 수 있다.
- 컴파일 타임이 아닌 런타임에 동적으로 기능을 변경할 수 있다.
- 각 장식자 클래스마다 고유의 책임을 가져 단일 책임 원칙(SRP)
- 클라이언트 코드 수정없이 기능 확장이 필요하면 장식자 클래스를 추가하면 되니 개방 폐쇄 원칙(OCP)
- 구현체가 아닌 인터페이스를 바라봄으로써의존 역전 원칙(DIP)

패턴 단점

- 만일 장식자 일부를 제거하고 싶다면, Wrapper 스택에서 특정 wrapper를 제거하는 것은 어렵다.
- 데코레이터를 조합하는 초기 생성코드가 보기 안좋을 수 있다. ex) new A(new B(new C(new D())))
- 어느 장식자를 먼저 데코레이팅 하느냐에 따라 데코레이터 스택 순서가 결정지게 되는데, 만일 순서에 의존하지 않는 방식으로 데코레이터를 구현하기는 어렵다.

예시) 도로 표시 방법 조합 (데코레이터 사용 (X))

![Untitled 1](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/05e9e29b-5a05-4f9b-a102-ccb1b519b9ad)

```java
// 기본 도로 표시 클래스
public class RoadDisplay {
    public void draw() { System.out.println("기본 도로 표시"); }
}
```

```java
// 기본 도로 표시 + 차선 표시 클래스
public class RoadDisplayWithLane extends RoadDisplay {
  public void draw() {
      super.draw(); // 상위 클래스, 즉 RoadDisplay 클래스의 draw 메서드를 호출해서 기본 도로 표시
      drawLane(); // 추가적으로 차선 표시
  }
  private void drawLane() { System.out.println("차선 표시"); }

```

```java
public class Client {
  public static void main(String[] args) {
      RoadDisplay road = new RoadDisplay();
      road.draw(); // 기본 도로만 표시

      RoadDisplay roadWithLane = new RoadDisplayWithLane();
      roadWithLane.draw(); // 기본 도로 표시 + 차선 표시
  }

```

문제점

1. 또 다른 도로 표시 기능을 추가로 구현하는 경우

![Untitled 2](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/960ee1ff-d349-458c-be79-17408f5830bb)

```java
// 기본 도로 표시 + 교통량 표시 클래스
public class RoadDisplayWithTraffic extends RoadDisplay {
   public void draw() {
     super.draw(); // 상위 클래스, 즉 RoadDisplay 클래스의 draw 메서드를 호출해서 기본 도로 표시
     drawTraffic(); // 추가적으로 교통량 표시
   }
   private void drawTraffic() { System.out.println("교통량 표시"); }
}
```

1. 여러 가지 추가 기능을 조합해야 하는 경우

![Untitled 3](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/9e630336-3139-4d0b-9219-c64695322fca)

![Untitled 4](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/78dc7599-aeab-4ee6-92f5-e127862f4d33)

⇒ 상속을 통한 기능의 확장은 각 기능별로 클래스를 추가해야 한다.

해결책(데코레이터 패턴 O)

- 각 추가 기능별로 개별적인 클래스를 설계하고 기능을 조합할 때 각 클래스의 객체 조합을 이용

![Untitled 5](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/4e762c61-55cd-4ba5-bf3b-962b9d3eda16)

```java
public abstract class Display { public abstract void draw(); }
```

```java
/* 기본 도로 표시 클래스 */
public class RoadDisplay extends Display {
  @Override
  public void draw() { System.out.println("기본 도로 표시"); }

```

```java
/* 다양한 추가 기능에 대한 공통 클래스 */
public abstract class DisplayDecorator extends Display {
  private Display decoratedDisplay;
  // '합성(composition) 관계'를 통해 RoadDisplay 객체에 대한 참조
  public DisplayDecorator(Display decoratedDisplay) {
      this.decoratedDisplay = decoratedDisplay;
  }
  @Override
  public void draw() { decoratedDisplay.draw(); }
```

```java
/* 차선 표시를 추가하는 클래스 */
public class LaneDecorator extends DisplayDecorator {
  // 기존 표시 클래스의 설정
  public LaneDecorator(Display decoratedDisplay) { super(decoratedDisplay); }
  @Override
  public void draw() {
      super.draw(); // 설정된 기존 표시 기능을 수행
      drawLane(); // 추가적으로 차선을 표시
  }
  // 차선 표시 기능만 직접 제공
  private void drawLane() { System.out.println("\t차선 표시"); }
}
/* 교통량 표시를 추가하는 클래스 */
public class TrafficDecorator extends DisplayDecorator {
  // 기존 표시 클래스의 설정
  public TrafficDecorator(Display decoratedDisplay) { super(decoratedDisplay); }
  @Override
  public void draw() {
      super.draw(); // 설정된 기존 표시 기능을 수행
      drawTraffic(); // 추가적으로 교통량을 표시
  }
  // 교통량 표시 기능만 직접 제공
  private void drawTraffic() { System.out.println("\t교통량 표시"); }
}
```

```java
public class Client {
  public static void main(String[] args) {
      Display road = new RoadDisplay();
      road.draw(); // 기본 도로 표시
      Display roadWithLane = new LaneDecorator(new RoadDisplay());
      roadWithLane.draw(); // 기본 도로 표시 + 차선 표시
      Display roadWithTraffic = new TrafficDecorator(new RoadDisplay());
      roadWithTraffic.draw(); // 기본 도로 표시 + 교통량 표시
  }
}
```

![Untitled 6](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/fc1886f2-7bb3-4c8a-8cd7-e99404d9c40a)

참고자료

[https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html](https://gmlwjd9405.github.io/2018/07/09/decorator-pattern.html)

[https://inpa.tistory.com/entry/GOF-💠-데코레이터Decorator-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0Decorator-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)
