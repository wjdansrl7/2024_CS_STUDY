# 복합(Compound) 패턴

여러 패턴이 혼합된 패턴

단순히 여러 패턴이 사용되었다고 복합패턴 X

→ 여러 패턴이 사용되는 동시에 일반적인 문제를 해결하는데 반복적으로 사용될 수 있어야 한다.

## MVC = Model + View + Controller

![[https://velog.io/@leetmdgus/복합체-패턴-Composite-Pattern](https://velog.io/@leetmdgus/%EB%B3%B5%ED%95%A9%EC%B2%B4-%ED%8C%A8%ED%84%B4-Composite-Pattern)](images/compound1.png)

[https://velog.io/@leetmdgus/복합체-패턴-Composite-Pattern](https://velog.io/@leetmdgus/%EB%B3%B5%ED%95%A9%EC%B2%B4-%ED%8C%A8%ED%84%B4-Composite-Pattern)

![[https://sgc109.github.io/2020/07/18/compound-pattern-feat-mvc/](https://sgc109.github.io/2020/07/18/compound-pattern-feat-mvc/)](images/compound2.png)

[https://sgc109.github.io/2020/07/18/compound-pattern-feat-mvc/](https://sgc109.github.io/2020/07/18/compound-pattern-feat-mvc/)

### **Model - Observer 패턴**

- Model의 상태가 변경되었을 때 Controller, View에게 이 사실을 알림

### **View - Composition 패턴**

- View를 구성하는 컴포넌트들은 계층 구조를 이룬다. (e.g. Java Swing 의 JFrame/JLabel 등, Android 의 View/ViewGroup, HTML 의 DOM)

### Controller - Strategy 패턴

- Controller가 View의 행동에 해당, 다른 행동을 하고 싶으면 다른 컨트롤러로 교환
- Controller의 핵심 기능을 인터페이스로 분리하여 View가 이 인터페이스를 통해 Controller를 구성(Composition)한다. 그렇기 때문에 View는 Controller를 갈아 끼우며 기능을 변경할 수 있다.

### 출처

[https://sgc109.github.io/2020/07/18/compound-pattern-feat-mvc/](https://sgc109.github.io/2020/07/18/compound-pattern-feat-mvc/)

[https://m.blog.naver.com/zbqmgldjfh/222499154933](https://m.blog.naver.com/zbqmgldjfh/222499154933)
