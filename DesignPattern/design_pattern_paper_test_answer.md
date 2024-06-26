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
