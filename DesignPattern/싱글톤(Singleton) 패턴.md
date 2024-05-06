# 싱글톤(Singleton) 패턴

**싱글톤 패턴 = 단 하나의 유일한 객체를 만들기 위한 코드 패턴**

⇒ 메모리 절약을 위해 인스턴스가 필요할 때 똑같은 인스턴스를 새로 만들지 않고 기존의 인스턴스를 가져와 활용하는 기법 (전역 변수랑 비슷한 느낌)

⇒ 어플리케이션에서 유일해야 좋은 것에 싱글톤 패턴 적용

⇒ 객체가 리소스를 많이 차지하는 역할일 경우 싱글톤 패턴 적합

ex) 데이터베이스 연결 모듈, 디스크 연결, 네트워크 통신, DBCP 커넥션 풀, 스레드 풀, 캐시, 로그 기록 객체 등등

### 구현

싱글톤으로 사용할 클래스를 외부에서 마구잡이로 new 생성자를 통해 인스턴스화 하는 것을 제한하기 위해 클래스 생성자 메서드에 private 키워드를 붙여준다.

getInstance() 메서드에 생성자 초기화를 해주어, instance 필드 변수가 null일 경우 초기화를 진행하고, null이 아닐 경우 생성돼 있는 객체를 반환하는 방식

### 방식

1. Eager Initialization
    
    ```java
    class Singleton {
        // 싱글톤 클래스 객체를 담을 인스턴스 변수
        private static final Singleton INSTANCE = new Singleton();
    
        // 생성자를 private로 선언 (외부에서 new 사용 X)
        private Singleton() {}
    
        public static Singleton getInstance() {
            return INSTANCE;
        }
    }
    출처: https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자 [Inpa Dev 👨‍💻:티스토리]
    ```
    
    - static final 이라 멀티 스레드 환경에서도 안전
    - static 멤버는 객체를 사용하지 않더라도 메모리에 적재하기 때문에 리소스가 크면 공간 자원 낭비가 크다. → 크기 않을 객체일 때 사용
    - 예외 처리 불가능
2. Static block initialization
    
    ```java
    class Singleton {
        // 싱글톤 클래스 객체를 담을 인스턴스 변수
        private static Singleton instance;
    
        // 생성자를 private로 선언 (외부에서 new 사용 X)
        private Singleton() {}
        
        // static 블록을 이용해 예외 처리
        static {
            try {
                instance = new Singleton();
            } catch (Exception e) {
                throw new RuntimeException("싱글톤 객체 생성 오류");
            }
        }
    
        public static Singleton getInstance() {
            return instance;
        }
    }
    출처: https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자 [Inpa Dev 👨‍💻:티스토리]
    ```
    
    - static block : 클래스가 로딩되고 클래스 변수가 준비된 후 자동으로 실행되는 블록
    - static block을 이용해 예외를 잡을 수 있지만 여전히 static 문제
3. Lazy initialization
    
    ```java
    class Singleton {
        // 싱글톤 클래스 객체를 담을 인스턴스 변수
        private static Singleton instance;
    
        // 생성자를 private로 선언 (외부에서 new 사용 X)
        private Singleton() {}
    	
        // 외부에서 정적 메서드를 호출하면 그제서야 초기화 진행 (lazy)
        public static Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton(); // 오직 1개의 객체만 생성
            }
            return instance;
        }
    }
    출처: https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자 [Inpa Dev 👨‍💻:티스토리]
    ```
    
    - 객체 생성 관리를 내부적으로 처리
    - 메서드를 호출했을 때 인스턴스 변수의 null 유무에 따라 초기화 하거나 있는 걸 반환
    - static의 메모리 차지 한계점 극복
    - 하지만 Thread Safe 하지 않다. (멀티스테드 환경에서 안전하지 않다.)
        - 스레드 A가 if문을 평가하고 인스턴스 생성 코드로 진입(아직 초기화 X)
        - 그때 스레드 B가 if문 평가 → 아직 초기화 안돼서 이것도 참
        - 결과적으로 스레드 A와 B가 인스턴스 초기화를 두번 실행(원자성 결여)
        
        ![[https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4-%EA%BC%BC%EA%BC%BC%ED%95%98%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)](images/dp01-01.png)
        
        [https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4-%EA%BC%BC%EA%BC%BC%ED%95%98%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
        
    
4. Thread safe initialization
    
    ```java
    class Singleton {
        private static Singleton instance;
    
        private Singleton() {}
    
        // synchronized 메서드
        public static synchronized Singleton getInstance() {
            if (instance == null) {
                instance = new Singleton();
            }
            return instance;
        }
    }
    출처: https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자 [Inpa Dev 👨‍💻:티스토리]
    ```
    
    - synchronized 키워드를 통해 메서드에 스레드들을 하나씩 접근하도록 설정(동기화)
    - 하지만 여러개의 모듈들이 매번 객체를 가져올 때 syncronized 메서드를 매번 호출하여 동기화하느라 overhead가 발생 → 성능 하락
    - synchronized : 멀티 스레드 환경에서 두개 이상의 스레드가 하나의 변수에 동시에 접근 할 때 Race condition이 발생하지 않도록 한다. 스레드가 해당 메서드를 실행하는 동안 다른 스레드가 접근하지 못하도록 lock을 건다.
5. Double-Checked Locking
    
    ```java
    class Singleton {
        private static volatile Singleton instance; // volatile 키워드 적용
    
        private Singleton() {}
    
        public static Singleton getInstance() {
            if (instance == null) {
            	// 메서드에 동기화 거는게 아닌, Singleton 클래스 자체를 동기화 걸어버림
                synchronized (Singleton.class) { 
                    if(instance == null) { 
                        instance = new Singleton(); // 최초 초기화만 동기화 작업이 일어나서 리소스 낭비를 최소화
                    }
                }
            }
            return instance; // 최초 초기화가 되면 앞으로 생성된 인스턴스만 반환
        }
    }
    출처: https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자 [Inpa Dev 👨‍💻:티스토리]
    ```
    
    - 매번 동기화를 실행하지 않고, 최초 초기화할때만 적용하는 기법
    - 인스턴스 필드에 volatile 키워드를 붙여야 I/O 불일치 문제를 해결할 수 있다.
    - volatile 키워드를 이용하기 위해선 JVM 1.5 이상 필요, JVM에 따라 여전히 Thread safe 하지 않는 경우가 발생하여 **사용을 지양**한다.
    - volatile : Java에서는 스레드를 여러개 사용할 경우, 성능을 위해 각각의 스레드들은 변수를 메모리(RAM)으로부터 가져오는 것이 아니라 캐시(Cache) 메모리에서 가져오게 되는데 비동기로 변수값을 캐시에 저장하다가 각 스레드마다 할당되어있는 캐시 메모리의 변수값이 일치하지 않을 수 있다. volatile 키워드는 이 변수는 캐시에서 읽지 말고 메인 메모리에서 읽어오도록 지정해주는 역할을 한다.
        
        ![[https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4-%EA%BC%BC%EA%BC%BC%ED%95%98%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)](images/dp01-02.png)
        
        [https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4-%EA%BC%BC%EA%BC%BC%ED%95%98%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)
        
6. Bill Pugh Solution (Lazy Holder) ⭐
    
    ```java
    class Singleton {
    
        private Singleton() {}
    
        // static 내부 클래스를 이용
        // Holder로 만들어, 클래스가 메모리에 로드되지 않고 getInstance 메서드가 호출되어야 로드됨
        private static class SingleInstanceHolder {
            private static final Singleton INSTANCE = new Singleton();
        }
    
        public static Singleton getInstance() {
            return SingleInstanceHolder.INSTANCE;
        }
    }
    출처: https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자 [Inpa Dev 👨‍💻:티스토리]
    ```
    
    - 권장되는 두가지 방법중 하나
    - 멀티 스레드 환경에서 안전하고 Lazy Loading (나중에 객체 생성)도 가능한 완벽한 싱글톤 기법
    - 클래스 안에 내부 클래스(holder)를 두어 JVM의 클래스 로더 메커니즘과 클래스가 로드되는 시점을 이용한 방법 Thread safe
    - static 메서드에서는 static 멤버만을 호출할 수 있기 때문에 내부 클래스를 static으로 설정, 내부 클래스의 치명적인 문제점인 메모리 누수 문제를 해결하기 위해 내부 클래스를 static으로 설정
    - 클라이언트가 임의로 싱글톤을 파괴할 수 있다는 단점을 지닌다. (Reflection API, 직렬화/역직렬화를 통해)
        - Reflection API : **구체적인 클래스 타입을 알지 못해도** 그 클래스의 정보(메서드, 타입, 변수 등등)에 접근할 수 있게 해주는 자바 API다.
    1. 우선 내부클래스를 static으로 선언하였기 때문에, 싱글톤 클래스가 초기화되어도 SingleInstanceHolder 내부 클래스는 메모리에 로드 X
    2. 어떠한 모듈에서 getInstance() 메서드를 호출할 때,  SingleInstanceHolder 내부 클래스의 static 멤버를 가져와 리턴하게 되는데, 이때 내부 클래스가 한번만 초기화되면서 싱글톤 객체를 최초로 생성 및 리턴
    3. 마지막으로 final 키워드를 지정하여 다시 값이 할당되지 않도록 방지
    - JVM의 Class Loader의 클래스 로딩 및 초기화 과정관련 추가 정보
        - [https://inpa.tistory.com/entry/JAVA-☕-클래스는-언제-메모리에-로딩-초기화-되는가-❓](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%ED%81%B4%EB%9E%98%EC%8A%A4%EB%8A%94-%EC%96%B8%EC%A0%9C-%EB%A9%94%EB%AA%A8%EB%A6%AC%EC%97%90-%EB%A1%9C%EB%94%A9-%EC%B4%88%EA%B8%B0%ED%99%94-%EB%90%98%EB%8A%94%EA%B0%80-%E2%9D%93)
        - [https://inpa.tistory.com/entry/JAVA-☕-자바의-내부-클래스는-static-으로-선언하자](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%9E%90%EB%B0%94%EC%9D%98-%EB%82%B4%EB%B6%80-%ED%81%B4%EB%9E%98%EC%8A%A4%EB%8A%94-static-%EC%9C%BC%EB%A1%9C-%EC%84%A0%EC%96%B8%ED%95%98%EC%9E%90)
7. Enum 이용 ⭐
    
    ```java
    enum SingletonEnum {
        INSTANCE;
    
        private final Client dbClient;
    	
        SingletonEnum() {
            dbClient = Database.getClient();
        }
    
        public static SingletonEnum getInstance() {
            return INSTANCE;
        }
    
        public Client getClient() {
            return dbClient;
        }
    }
    
    public class Main {
        public static void main(String[] args) {
            SingletonEnum singleton = SingletonEnum.getInstance();
            singleton.getClient();
        }
    }
    출처: https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자 [Inpa Dev 👨‍💻:티스토리]
    ```
    
    - 권장되는 두가지 방법중 하나
    - enum은 애초에 멤버를 만들때 private으로 만들고 한번만 초기화하기 때문에 Thread safe
    - enum 내에서 상수 뿐만 아니라, 변수나 메서드를 선언해 사용이 가능하기 때문에 이를 이용해 싱글톤 클래스 처럼 응용이 가능
    - 이전의 Bill Pugh Solution 기법과 달리, 클라이언트에서 Reflection을 통한 공격에도 안전
    - 하지만 싱글톤 클래스를 멀티톤 (일반적인 클래스)로 Migration시 코드를 다시 짜야한다는 단점 존대
    - 클래스 상속이 필요할 때 enum 외의 클래스 상속은 불가

### 사용

- Lazy Holder : 성능이 중요시 되는 환경
- Enum : 직렬화, 안정성 중요시 되는 환경

### 싱글톤의 문제점

1. **모듈간 의존성이 높아진다.**
    
    대부분 싱글톤을 이용하는 경우 인터페이스가 아닌 클래스 객체를 미리 생성하고 정적 메서드를 이용해 사용하기 때문에 클래스 사이에 강한 의존성과 높은 결합이 생긴다.
    
    하나의 싱글톤 클래스를 여러 모듈드링 공유해서 싱글톤의 인스턴스가 변경되면 이를 참조하는 모듈들도 수정이 필요하다.
    
2. **SOLID 원칙에 위배되는 사례가 많다.**
    
    싱글톤 인스턴스 자체가 하나만 생성하기 때문에 여러 책임을 지니게 되는 경우가 많아 SRP를 위반하는 경우 존재 
    
    싱글톤 인스턴스 하나가 너무 많은 일을 하거나, 많은 데이터를 공유 시키면 다른 클래스들 간의 결합도가 높아지게 되어 OCP 위반 경우 존재
    
    클라이언트가 인터페이스와 같은 추상화가 아닌, 구체 클래스에 의존하게 되어 DIP 위반하는 경우 존재
    
    싱글톤 인스턴스를 너무 많은 곳에서 사용할 경우 잘못된 디자인 형태가 될 수 있다.
    
3. **TDD 단위 테스트에 애로사항이 있다.**
    
    싱글톤 클래스를 사용하는 모듈을 테스트하기 어렵다.
    
    단위 테스트를 할 때, 테스트가 서로 독립적이어야 하고 어떤 순서로든 실행할 수 있어야 하는데, 싱글톤 인스턴스는 자원을 공유하고 있기 때문에, 테스트가 결함없이 수행되려면 매번 인스턴스의 상태를 초기화시켜야 한다. 그렇지 않으면 어플리케이션 전역에서 상태를 공유하기 때문에 테스트가 온전하게 수행되지 못할 수도 있다.
    

### 정리

하나의 인스턴스 생성을 보증하여 효율적이지만 문제점도 많다.

유연성이 많이 떨어지는 패턴.

직접 만들어 사용하기 보다는 스프링 컨테이너 같은 프레임워크의 도움을 받으면 싱글톤 패턴의 문제점들을 보완하면서 장점의 혜택을 누릴 수 있다.

스프링 프레임워크에서는 싱글톤 패턴이란게 없고 내부적으로 클래스의 제어를 IoC 컨테이너에게 넘겨 컨테이너가 관리하기 때문에, 평범한 객체도 하나의 인스턴스 뿐인 싱글톤으로 존재 가능하기 때문에 싱글톤의 단점이 없다.

프레임워크의 도움 없이 싱글톤 패턴을 적용하고 싶다면 장단점을 고려하여 사용해야 한다.

### Ref)

[https://inpa.tistory.com/entry/GOF-💠-싱글톤Singleton-패턴-꼼꼼하게-알아보자](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EC%8B%B1%EA%B8%80%ED%86%A4Singleton-%ED%8C%A8%ED%84%B4-%EA%BC%BC%EA%BC%BC%ED%95%98%EA%B2%8C-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90)

[https://tecoble.techcourse.co.kr/post/2020-07-16-reflection-api/](https://tecoble.techcourse.co.kr/post/2020-07-16-reflection-api/)