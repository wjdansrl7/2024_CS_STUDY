# Proxy Pattern

- 프록시 패턴은 **원본 객체를 대리하여 대신 처리하게 함으로써 로직의 흐름을 제어**하는 행동 패턴이다.

- 클라이언트가 대상 객체를 직접 쓰는 게 아니라 중간에 프록시를 거쳐서 쓰는 코드 패턴

- 번거럽게 프록시를 이용하는 이유는, 대상 클래스가 **민감한 정보**를 가지고 있거나 **인스턴스화 하기에 무겁**거나 **추가 기능을 가미**하고 싶은데, **원본 객체를 수정할 수 없는 상황**일 때를 극복하기 위해서다.

## Proxy pattern의 목적

1. 보안 : 프록시는 클라이언트가 작업을 수행할 수 있는 권한이 있는 지를 확인하고 요청을 대상에게 전달할 수 있다.
2. 캐싱 : 프록시가 내부 캐시를 유지하여 데이터가 캐시에 아직 존재하지 않는 경우에만 대상에서 작업이 실행되도록 한다.
3. 데이터 유효성 검사 : 프록시가 입력을 대상으로 전달하기 전에 유효성을 검사한다.
4. 지연 초기화 : 대상의 생성 비용이 비싸다면 프록시는 그것을 필요로 할 때까지 연기할 수 있다.
5. 로깅 : 프록시는 메소드 호출과 상대 매개 변수를 인터셉트하고 이를 기록한다.
6. 원격 객체 : 프록시는 원격 위치에 있는 객체를 가져와서 로컬처럼 보이게 할 수 있다.

## Proxy Pattern 구조

![프록시패턴 구조](/DesignPattern/images/Proxy.png)

1. Subject : Proxy와 RealSubject를 하나로 묶는 인터페이스
    - 대상 객체와 프록시 역할을 동일하게 하는 추상 메서드를 정의한다 . `operation()`
    - 인터페이스로 인해 클라이언트는 Proxy과 realSubject 역할의 차이를 의식할 필요가 없다.

2. RealSubject : 원본 대상 객체
3. Proxy : realSubject를 중계할 대리자 역할
    - 프록시는 대상 객체를 합성한다.
    - 프록시는 대상 객체와 같은 이름의 메소드를 호추라며, 별도의 로직을 수행할 수 있다. 
    - 프록시는 흐름제어할 뿐 결과값을 조작하거나 변경해서는 안된다.
4. Client : Subject 인터페이스를 이용하여 프록시 객체를 생성해 이용.
    - 클라이언트는 프록시를 중간에 두고 프록시를 통해 RealSubject와 데이터를 주고 받는다.

## Proxy Pattern 종류

1. 기본형 프록시

```java
Interface ISubject {
    void action();
}

class RealSubject implements ISubject {
    public void action() {
        System.out.println("원본 객체!!");
    }
}

class Proxy implements ISubject {

    private ISubject subject;

    Proxy(ISubject subject) {
        this.subject = subject;
    }

    public void action() {
        subject.action();
        System.out.println("프록시 객체!!");
    }
}

class Client {
    public static void main(String[] args) {
        ISubject sub = new Proxy(new RealSubject());
        sub.action();
    }
}

// 원본 객체!!
// 프록시 객체!!

```


> 기본형 프록시를 어떤식으로 변형하느냐에 따라 프록시 종류가 나누어 진다.

2. 가상 프록시
- 지연 초기화 방식
- 이 구현은 실제 객체의 생성에 많은 자원이 소모 되지만 사용 빈도는 낮을 때 쓰는 방식이다.
- 서비스가 시작될 때 객체를 생성하는 대신에 객체 초기화가 실제로 필요한 시점에 초기화될수 있도록 지연할 수 있다.


```java
class Proxy implements ISubject {

    private ISubject subject;

    Proxy() { }

    public void action() {
        if(subject == null) subject = new RealSubject();
        subject.action();
    }

}

```

> 기본형과 달리 생성자에서 RealSubject주입하지 않고 실제 action에서 주입 여부를 확인하고 주입한다. => 사용되지 않을 수도 있다면 가상 프록시 방식이 더 효율적일 것이다.


3. 보호 프록시
- 프록시가 대상 객체에 대한 자원으로의 엑세스를 제어한다.
- 프록시 객체를 통해 클라이언트의 자격 증명이 기중과 일치하는 경우에만 서비스 객체에 요청을 전달할 수 있게 한다.


```java
class Proxy implements ISubject {
    private RealSubject subject;
    boolean access;

    Proxy(RealSubject subject, boolean access) {
        this.subject = subject;
        this.access = access;
    }

    public void action() {
        if(access) {
            subject.action();
        }
    }
}

```

4. 로깅 프록시
- 대상 객체데 해단 로깅을 추가하는 경우

```java
class Proxy implements ISubject {
    private RealSubject subject; 

    Proxy(RealSubject subject) {
        this.subject = subject;
    }

    public void action() {
        System.out.println("로깅..................");
        subject.action(); 
        System.out.println("로깅..................");
    }
}
```

5. 원격 프록시
- 대상객체가 원격서버에 존재하는 경우, 로컬에 프록시 객체 생성해 **네트워크와 관련된 불필요한 작업**들을 프록시에서 처리하고 결과값만 반환하는 방법
- 클라이언트 입장에선 프록시를 통해 객체를 이용하는 것이니 원격이든 로컬이든 신경 쓸 필요가 없으며, 프록시는 진짜 객체와 통신을 대리하게 된다.

![원격 프록시](/DesignPattern/images/%EC%9B%90%EA%B2%A9%ED%94%84%EB%A1%9D%EC%8B%9C.png)

> 프록시를 스터브, 프록시로부터 전달된 명령어를 이해하고 적합한 메소드를 호출해주는 보조객체를 스켈레톤이라 한다..



6. 캐싱 프록시
- 데이터가 큰 경우에 캐싱하여 재사용을 유도
- 클라이언트 요청의 결과를 캐시하고 이 캐시의 수명주기를 관리한다.


```java
class Proxy implements ISubject {
    private RealSubject subject; 
    Map<String, String> data = new HashMap<>();

    Proxy(RealSubject subject) {
        this.subject = subject;
    }

    public void action(String key) {
        if(data.containsKey(key)) return data.get(key);
        String value = subject.action(); 
        data.put(key, value);

        return value;
    }
}
```


> **HTTP Proxy**
> HTTP Proxy는 웹서버와 브라우저 사이에서 웹 페이지의 캐싱을 실행하는 소프트웨어이다. 웹 브라우저가 어떤 웹페이지를 표시할 때 직접 웹 서버에서 가져오는 것이 아니라, HTTP Proxy가 캐쉬해서 어떤 페이지를 대신해서 취득한다. 유효기간이 지났다면 웹 서버에 웹페이즐 다시 가지러 간다.


![HTTPPRoxy](/DesignPattern/images/HTTP_Proxy.png)

## 프록시 패턴 특징

### 프록시 패턴 사용시기
- 접근을 제어하거나 기능을 추가하고 싶은데, 기존의 특정 객체를 수정할 수 없는 상황
- 초기화 지연, 접근 제어, 로깅, 캐싱 등 **기존 객체 동작에 수정없이 가미**하고 싶을 때

### 프록시 패턴 장점
- **개방 폐쇄 원칙(OCP)** 준수 : 기존 대상 객체의 코드를 변경하지 않고 새로운 기능 추가 가능
- **단일 책임 원칙(SRP)** 준수 : 대상 객체는 자신의 기능에만 집중하고, 그외의 부가 기능은 프록시객체에 위임해 다중 책임을 피할 수 있다.
- 원래의 기능에다가 부가적인 기능(로깅, 인증, 네트워크 통신 등)을 수행하는 데 용이하다.
- 사이즈가 큰 객체가 로딩되기 전에도 프록시 객체를 통해 참조할 수 있다. 


### 프록시 패턴 단점
- 객체를 생성할 때 한 단계를 거치게 되므로, 빈번한 객체 생성이 필요한 경우 성능이 저하될 수 있다.
- 로직이 난해해져 가독성이 떨어질 수 있다.
- 많은 프록시 클래스를 도입해야하므로 코드의 복잡도가 증가한다. -> 동적 프록시를 이용해 해결함


## Java & Spring에서의 Proxy
- Java
    - java.lang.reflect.Proxy
    - java.rmi.* (원격 프록시 모듈)
    - javax.ejb.EJB 
    - javax.inject.Inject
    - javax.persistence.PersistenceContext
- Dynamic Proxy
- Spring AOP

<br>
<br>

**Ref.**
- https://inpa.tistory.com/entry/GOF-💠-프록시Proxy-패턴-제대로-배워보자?category=967431 [Inpa Dev 👨‍💻:티스토리]
- https://velog.io/@j3beom/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%ED%94%84%EB%A1%9D%EC%8B%9C-%ED%8C%A8%ED%84%B4-Proxy-Pattern