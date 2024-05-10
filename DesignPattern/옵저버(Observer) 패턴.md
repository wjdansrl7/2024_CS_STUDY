# 옵저버 패턴(Observer)

- 옵저버 패턴은 옵저버들이 관찰하고 있는 대상자의 상태에 변화가 있을 때마다 대상자는 직접 목록의 각 관찰자들에게 통지하고, 관찰자는 알림을 받아 조치를 취하는 행동 패턴이다.

- 옵저버는 다른 디자인 패턴과 다르게 **일대다(one-to-many) 의존성**을 가지는데, 주로 **분산 이벤트 핸들링 시스템**을 구현하는 데 사용된다. Pub/Sub(발행/구독) 모델로도 알려져 있다.

> 프로그래밍적으로 옵저버 패턴은 사실 '관찰'하기 보단 갱신을 위한 힌트 정보를 '전달'받길 기다린다고 보는 것이 적절하다. **관찰자는 수동적으로 전달받기를 기다린다.**

## 옵저버 패턴 흐름
1. 옵저버 패턴에서는 한개 관찰 대상자(Subject)와 여러개의 관찰자(Observer A, B, C)로 일 대 다 관계로 구성되어 있다.
2. Observer 패턴에서는 관찰 대상 Subject의 상태가 바뀌면 변경사항을 옵저버한테 통보해준다. (notifyObserver)
3. 대상자로부터 통보를 받은 Observer는 값을 바꿀수도 있고, 삭제하는 등 적절히 대응한다. (update)
4. 또한 Observer들은 언제든 Subject의 그룹에서 추가/삭제 될 수 있다. Subject 그룹에 추가되면 Subject로부터 정보를 전달받게 될 것이며, 삭제될 경우 더이상 Subject의 정보를 받을 수 없게 된다.

![ObserverPattern](/DesignPattern/images/observer.png)

```java
class Subject {

    List<IObserver> observers = new ArrayList<>();

    public void registerObserver(IObserver o) {
        observers.add(o);
    }

    public void removeObserver(IObserver o) {
        observers.remove(o);
    }

    public void notifyObserver() {
        for(IObserever o : observers) o.update();
    }
}

Interface IObserver {
    void update();
}

class ObserverA implements IObserver {
    void update() {
        System.out.println("A에게로 이벤트 알림이 왔습니다.");
    }
}

class ObserverB implements IObserver {
    void update() {
        System.out.println("B에게로 이벤트 알림이 왔습니다.");
    }
}

public class Client {
    public static void main(String[] args) {
        Subject publisher = new Subject();

        // 옵저버 등록
        publisher.registerObserver(new ObserverA());
        publisher.registerObserver(new ObserverB());

        // 공지
        publisher.notifyObserver();
        
        // B제거 후 A에게만 공지
        publisher.removeObserver(o2);
        publisher.notifyObserver();
    }
}

```

## 옵저버 패턴 사용 예시

- 날짜가 변경되었을 때 이를 주민들에게 알려주어야하는 상황

    - 날짜 변경 api에 주민들을 관리하는 기능. -> 변경시에 주민들에게 자동으로 공지!

    - main 함수에서 주민들을 받아놓았다가, 변경시에 주민들에게 공지를 보내는 방법은 올바른 옵저버 패턴이 아니다.


## 옵저버 패턴 사용 시기
- 앱이 한정된 시간, 특정한 케이스에만 다른 객체를 관찰해야 하는 경우
- 한 객체의 상태가 변경되면 다른 객체도 변경해야 할때. 그런데 어떤 객체들이 변경되어야 하는지 몰라도 될 때
- MVC 패턴
    - MVC의 Model과 View의 관계는 Observer 패턴의 Subject 역할과 Observer 역할의 관계에 대응된다.
    - 하나의 Model에 복수의 View가 대응한다.

## 옵저버 패턴의 장점
- Subject의 상태 변경을 주기적으로 조회하지 않고 자동으로 감지할 수 있다.
- 발행자의 코드를 변경하지 않고도 새 구독자 클래스를 도입할 수 있어 개방 폐쇄 원칙(OCP) 준수한다
- 런타임 시점에서에 발행자와 구독 알림 관계를 맺을 수 있다.
- 상태를 변경하는 객체(Subject)와 변경을 감지하는 객체(Observer)의 관계를 느슨하게 유지할 수 있다. (느슨한 결합)


## 옵저버 패턴의 단점
- 알림 순서를 제어할 수 없고 무작위 순서로 알림을 받음.
    - 물론 하드코딩으로 구현할 수 있지만, 복잡성과 결합성이 높아져 추천되지 않는다.
- 옵저버 패턴을 자주 구성하면 구조와 동작을 알아보기 힘들어져 코드 복잡도가 증가한다.
- 다수의 옵저버 객체를 등록 이후 해지하지 않는다면 **메모리 누수**가 발생할 수 있다.


## 자바에서의 옵저버 객체

- 옵저버 패턴을 위해서 java.util.Observer와 java.util.Observable 가 존재한다. 하지만 현재는 deprecated된 상태라고 한다. 

    - Observable이 class 이기에 상속과 관련한 문제
    - 멀티쓰레드 환경을 보장하지 못한다.
    - Obserable 클래스의 핵심 메소드를 외부에서 호출할 수 없다(protected)

- 현재는 `java.beans` 에서 제공하는 다른 이벤트 모델을 주로 사용한다.



<br>
<br>

**Ref.**

- https://inpa.tistory.com/entry/GOF-💠-옵저버Observer-패턴-제대로-배워보자 [Inpa Dev 👨‍💻:티스토리]