### 싱글톤(Singleton) 패턴

1. 인스턴스를 하나만 생성하여 메모리를 절약할 수 있다.
2. 모듈간 의존성이 높아진다 or SOLID원칙 위반(SRP, OCP, DIP) or TDD단위테스트 어려움

### 어댑터(Adapter) 패턴

1. 호환성이 없는 인터페이스 때문에 함께 동작할 수 없는 클래스들을 함께 작동해주도록 변환해주는 패턴 (호환성 부여 및 신규 기능 확장)
2. 아래 보기 중 택 1

- java.util.Arrays 의 asList()
- java.util.Collections 의 list()
- java.util.Collections 의 enumeration()
- java.io.InputStreamReader(InputStream) (returns a Reader)
- java.io.OutputStreamWriter(OutputStream) (returns a Writer)
- javax.xml.bind.annotation.adapters.XmlAdapter 의 marshal() and unmarshal()

### 상태(State) 패턴

1. 객체가 특정 상태에 따라 행위를 달리하는 상황에서, 상태를 조건문으로 검사해서 행위를 달리하는 것이 아닌, 상태를 객체화하여 상태가 행동할 수 있도록 위임하는 패턴
2. 아무거나 상태 관련이면 정답(ex: 노트북 전원)

### 복합(Compound) 패턴

1. 여러 패턴이 혼합된 패턴
2. Model - Observer, View - Composite, Controller - Strategy

### 컴포지트 패턴

1. 전체, 부분
2. 복합 객체와 단일 객체를 동일한 컴포넌트로 취급하여, 클라이언트에게 이 둘을 구분하지 않고 동일한 인터페이스를 사용하도록 하는 구조 패턴

### 템플릿 메소드 패턴

1. 여러 클래스에서 공통으로 사용하는 메서드를 템플릿화 하여 상위 클래스에서 정의하고, 하위 클래스마다 세부 동작 사항을 다르게 구현하는 패턴
2. 1번

### 퍼사드 패턴

1. 복잡한 서브클래스들의 공통적인 기능을 정의하는 상위 수준의 인터페이스를 제공하는 패턴.
2. - 복잡한 서브클래스들의 공통적인 기능을 정의하는 상위 수준의 인터페이스를 제공하는 패턴.
   - 하위 시스템의 복잡성에서 코드를 분리하여, 외부에서 시스템을 사용하기 쉬워진다.
   - 하위 시스템 간의 의존관계가 많을 경우 이를 감소시키고 의존성을 한 곳으로 모을 수 있다.
   - 복잡한 코드를 감춤으로써, 클라이언트가 시스템의 코드를 모르더라도 Facade클래스만 이해하고 사용 가능하다.

### 데코레이터 패턴
1. 기능
2. 4번

### 반복자 패턴
1. 반복자 패턴
2. 1) 클래스가 늘어나고, 복잡도가 증가 2) 구현 방법에 따라 캡슐화를 위배할 수 있음

### 전략 패턴
1. 객체가 할 수 있는 행위들 각각을 전략으로 만들어 놓고, 동적으로 행위의 수정이 필요한 경우, 전략을 바꾸는 것만으로 행위의 수정이 가능하도록 만든 패턴
2. 캡슐화

### 커맨드 패턴
1. 재사용성, 메서드
2. 요청의 캡슐화, 확장성, 히스토리 및 로그 관리, 작업의 취소 및 재실행, 명령 큐잉 및 매크로 명령

## 팩토리 패턴
1. OCP
2. 
``` java
public class client {

    public static void main(String[] args) {
        // hint : 팩토리 패턴을 이용한다. 팩토리를 만저 만들고 해당 팩토리에서 객체를 만들면 됨.
        UserFactory userFactory = new NaverUserFactory();
        User user = userFactory.newInstance();
    }
}
```