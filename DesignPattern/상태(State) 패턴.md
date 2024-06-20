# 상태(State) 패턴

객체가 특정 상태에 따라 행위를 달리하는 상황에서, 상태를 조건문으로 검사해서 행위를 달리하는 것이 아닌, 상태를 객체화하여 상태가 행동할 수 있도록 위임하는 패턴

상태를 클래스로 표현 → 클래스를 교체해서 상태의 변화 표현 → 객체 내부 상태 변경에 따라 객체의 행동을 상태에 특화된 행동들로 분리 → 새로운 행동 추가해도 다른 행동에 영향을 주지 않음

- 객체의 특정 상태 = 클래스
- 상태에 따른 행위 = 클래스 내 메서드
- 상태 클래스를 인터페이스로 캡슐화

## 구조

![[https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%83%81%ED%83%9CState-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)](images/state1.png)

[https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%83%81%ED%83%9CState-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)

1. State 인터페이스 : 상태를 추상화한 고수준 모듈
2. ConcreteState : 구체적인 각각의 상태를 클래스로 표현, State 인터페이스의 구체적 구현, 다음 상태 결정 시 Context에 상태 변경을 요청하는 역할도 겸한다.
3. Context : State를 이용하는 시스템, 시스템 상태를 나타내는 State 객체를 합성(Composition)하여 가진다. 클라이언트로부터 요청받으면 State객체에 행위 실행을 위임한다.

## 패턴 흐름

![[https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%83%81%ED%83%9CState-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)](images/state2.png)

[https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%83%81%ED%83%9CState-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)

```java
interface AbstractState {
    void requestHandle(Context cxt);
}

class ConcreteStateA implements AbstractState {
    @Override
    public void requestHandle(Context cxt) {}
}

class ConcreteStateB implements AbstractState {
    @Override
    public void requestHandle(Context cxt) {
        // 상태에서 동작을 실행한 후 바로 다른 상태로 바꾸기도 함
        // 예를 들어 전원 on 상태에서 끄기 동작을 실행한후 객체 상태를 전원 off로 변경 하듯이
        cxt.setState(ConcreteStateC.getInstance());
    }
}

class ConcreteStateC implements AbstractState {
    @Override
    public void requestHandle(Context cxt) {}
}

출처: https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자 [Inpa Dev 👨‍💻:티스토리]
```

```java
class Context {
    AbstractState state; // composition

    void setState(AbstractState state) {
        this.state = state;
    }

    // 상태에 의존한 처리 메소드로서 state 객체에 처리를 위임함
    void request() {
        state.requestHandle(this);
    }
}
출처: https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자 [Inpa Dev 👨‍💻:티스토리]
```

```java
class Client {
    public static void main(String[] args) {
        Context context = new Context();

        // 1. StateA 상태 설정
        context.setState(new ConcreteStateA());

        // 2. 현재 StateA 상태에 맞는 메소드 실행
        context.request();

        // 3. StateB 상태 설정
        context.setState(new ConcreteStateB());

        // 4. StateB 상태에서 또다른 StateC 상태로 변경
        context.request();

        // 5. StateC 상태에 맞는 메소드 실행
        context.request();
    }
}
출처: https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자 [Inpa Dev 👨‍💻:티스토리]
```

## 패턴 특징

- 객체의 행동이 상태(State)에 따라 각기 다른 동작을 할 때
- 상태 및 전환에 걸쳐 대규모 조건 분기 코드와 중복 코드가 많을 때

```java
// 전원 버튼 클릭
void powerButtonPush() {
    if (powerState == Laptop.OFF) {
        System.out.println("전원 on");
        changeState(Laptop.ON);
    } else if (powerState == Laptop.ON) {
        System.out.println("전원 off");
        changeState(Laptop.OFF);
    } else if (powerState == Laptop.SAVING) {
        System.out.println("전원 on");
        changeState(Laptop.ON);
    }
}
```

- 런타임시 객체의 상태를 유동적으로 변경해야 할 때
- 싱글톤 클래스로 구성, 객체의 현 상태를 나타내기에 대부분의 상황에서 유일하게 존재

### 장점

- 상태(State)에 따른 동작을 개별 클래스로 옮겨서 관리 → 모든 동작을 상태 클래스에 분산시킴으로써, 코드 복잡도를 줄일 수 있다.
- SRP 준수 (특정 상태와 관련된 코드를 별도의 클래스로 구성)
- OCP 준수 (기존 State 클래스나 컨텍스트를 변경하지 않고 새 State를 도입할 수 있다)

### 단점

- 상태 별 클래스 생성 → 관리해야 할 클래스 수 증가
- 상태 클래스 개수가 많고 상태 규칙이 자주 변경된다면, Context의 상태 변경 코드가 복잡해질 수 있다.
- 객체에 적용할 상태가 몇 가지 밖에 없거나 거의 상태 변경이 이루어지지 않는 경우 패턴을 적용하는 것이 과도할 수 있다.

## 코드 예제

### 노트북 전원 상태에 따른 동작 설계

1. 노트북 전원 ON 상태에서 전원 버튼을 누르면 전원 OFF 상태로 변경
2. 노트북 전원 OFF 상태에서 전원 버튼을 누르면 전원 ON 상태로 변경
3. 노트북 전원 절전 모드 상태에서 전원 버튼을 누르면 전원 ON 상태로 변경

```java
interface PowerState {
    void powerButtonPush(LaptopContext cxt);

    void typebuttonPush();
}

class OnState implements PowerState {
    @Override
    public void powerButtonPush(LaptopContext cxt) {
        System.out.println("노트북 전원 OFF");
        cxt.changeState(new OffState());
    }

    @Override
    public void typebuttonPush() {
        System.out.println("키 입력");
    }

    @Override
    public String toString() {
        return "노트북이 전원 ON 인 상태 입니다.";
    }
}

class OffState implements PowerState {
    @Override
    public void powerButtonPush(LaptopContext cxt) {
        System.out.println("노트북 전원 ON");
        cxt.changeState(new OnState());
    }

    @Override
    public void typebuttonPush() {
        throw new IllegalStateException("노트북이 OFF 인 상태");
    }

    @Override
    public String toString() {
        return "노트북이 전원 OFF 인 상태 입니다.";
    }
}

class SavingState implements PowerState {
    @Override
    public void powerButtonPush(LaptopContext cxt) {
        System.out.println("노트북 전원 on");
        cxt.changeState(new OnState());
    }

    @Override
    public void typebuttonPush() {
        throw new IllegalStateException("노트북이 절전 모드인 상태");
    }

    @Override
    public String toString() {
        return "노트북이 절전 모드 인 상태 입니다.";
    }
}
출처: https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자 [Inpa Dev 👨‍💻:티스토리]
```

```java
class LaptopContext {
    PowerState powerState;

    LaptopContext() {
        this.powerState = new OffState();
    }

    void changeState(PowerState state) {
        this.powerState = state;
    }

    void setSavingState() {
        System.out.println("노트북 절전 모드");
        changeState(new SavingState());
    }

    void powerButtonPush() {
        powerState.powerButtonPush(this);
    }

    void typebuttonPush() {
        powerState.typebuttonPush();
    }

    void currentStatePrint() {
        System.out.println(powerState.toString());
    }
}
출처: https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자 [Inpa Dev 👨‍💻:티스토리]
```

```java
class Client {
    public static void main(String[] args) {
        LaptopContext laptop = new LaptopContext();
        laptop.currentStatePrint();

        // 노트북 켜기 : OffState -> OnState
        laptop.powerButtonPush();
        laptop.currentStatePrint();
        laptop.typebuttonPush();

        // 노트북 절전하기 : OnState -> SavingState
        laptop.setSavingState();
        laptop.currentStatePrint();

        // 노트북 다시 켜기 : SavingState -> OnState
        laptop.powerButtonPush();
        laptop.currentStatePrint();

        // 노트북 끄기 : OnState -> OffState
        laptop.powerButtonPush();
        laptop.currentStatePrint();
    }
}
출처: https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자 [Inpa Dev 👨‍💻:티스토리]
```

### 싱글톤 적용

```java
class OnState implements PowerState {

	// Thread-Safe 한 싱글톤 객체화
    private OnState() {}
    private static class SingleInstanceHolder {
        private static final OnState INSTANCE = new OnState();
    }
    public static OnState getInstance() {
        return SingleInstanceHolder.INSTANCE;
    }

    @Override
    public void powerButtonPush(LaptopContext cxt) {
        System.out.println("노트북 전원 OFF");
        cxt.changeState(OffState.getInstance()); // 싱글톤 객체 얻기
    }

    @Override
    public void typebuttonPush() {
        System.out.println("키 입력");
    }

    @Override
    public String toString() {
        return "노트북이 전원 ON 인 상태 입니다.";
    }
}

// ...
출처: https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자 [Inpa Dev 👨‍💻:티스토리]
```

## (상태)State vs (전략)Strategy

### 유사점

- 클래스 다이어그램과 코드 사용법이 비슷하다.
- 둘 다 난잡한 조건 분기를 극복하기 위해 전략 / 상태를 객체화
- 둘 다 합성(Composition)을 통해 상속의 한계 극복
- 객체 지향 원칙 준수
- State는 Strategy의 확장으로 간주될 수 있다.

### 차이점

- 전략 패턴은 알고리즘을 객체화하여 클라이언트에서 유연적으로 전략을 제공 / 교체
- 상태패턴은 객체의 상태를 객체화하여 클라이언트와 상태 클래스 내부에서 다른 상태로 교체
- 전략패턴의 전략객체는 그 전략만의 알고리즘 동작을 정의 및 수행

```java
// 전략(추상화된 알고리즘)
interface IStrategy {
    void doSomething();
}

// 전략 알고리즘 A
class ConcreteStrategyA implements IStrategy {
    public void doSomething() {}
}

// 전략 알고리즘 B
class ConcreteStrategyB implements IStrategy {
    public void doSomething() {}
}
출처: https://inpa.tistory.com/entry/GOF-💠-전략Strategy-패턴-제대로-배워보자 [Inpa Dev 👨‍💻:티스토리]
```

- 상태패턴의 상태객체는 상태가 적용되는 대상 객체가 할 수 있는 모든 행동들을 정의 및 수행

```java
interface AbstractState {
    void requestHandle(Context cxt);
}

class ConcreteStateA implements AbstractState {
    @Override
    public void requestHandle(Context cxt) {}
}

class ConcreteStateB implements AbstractState {
    @Override
    public void requestHandle(Context cxt) {
        // 상태에서 동작을 실행한 후 바로 다른 상태로 바꾸기도 함
        // 예를 들어 전원 on 상태에서 끄기 동작을 실행한후 객체 상태를 전원 off로 변경 하듯이
        cxt.setState(ConcreteStateC.getInstance());
    }
}

class ConcreteStateC implements AbstractState {
    @Override
    public void requestHandle(Context cxt) {}
}
출처: https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자 [Inpa Dev 👨‍💻:티스토리]
```

- 전략패턴의 전략객체는 입력값에 따라 전략 형태가 다양하게 될 수 있으니 인스턴스로 구성
- 상태패턴의 상태객체는 정의된 상태를 서로 스위칭하기에 메모리 절약을 위해 싱글톤으로 구성
- 전략패턴 → 상속의 제한 대체(쉽게 알고리즘 전략 바꿀 수 있도록)
- 상태패턴 → 조건문 대체

### 출처

[https://inpa.tistory.com/entry/GOF-💠-상태State-패턴-제대로-배워보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%83%81%ED%83%9CState-%ED%8C%A8%ED%84%B4-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90)

[https://velog.io/@jinmin2216/디자인-패턴-스테이트상태-패턴-State-Pattern](https://velog.io/@jinmin2216/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%8A%A4%ED%85%8C%EC%9D%B4%ED%8A%B8%EC%83%81%ED%83%9C-%ED%8C%A8%ED%84%B4-State-Pattern)
