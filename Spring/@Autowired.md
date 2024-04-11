# @Autowired

Spring에서 사용되는 어노테이션 중 하나로, 의존성 주입(DI)을 자동으로 처리하는데 사용된다.

### 1. 생성자 주입 (Constructor Injection)

- 클래스의 필드를 파라미터로 사용하는 생성자에 @Autowired를 붙여서 사용
- 객체가 생성될 때 딱 한 번 호출되는 것이 보장된다 → 의존 관계가 변하지 않는 경우(불변), 필수 의존 관계에 사용
- 의존 관계에 있는 객체(필드)들을 final로 선언할 수 있다는 장점 → 생성자에 무조건 설정 → 누락 발생 X (NullPointerException 방지)
- 생성자가 하나일 경우 @Autowired를 생략할 수 있다.

```java
@Component
public class OrderServiceImpl implements OrderService {
	private final MemberRepository memberRepository;
	private final DiscountPolicy discountPolicy;
	
	@Autowired
	public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
            this.memberRepository = memberRepository;
            this.discountPolicy = discountPolicy;
	}
}
```

### 2. 수정자 주입 **(Setter Injection)**

- Setter method를 생성하고 @Autowired를 붙여서 사용
- 선택과 변경 가능성이 있는 의존 관계에 사용
- 단점
    - 수정자 주입을 사용하면 Setter method를 public으로 열어두어야 하기 때문에 언제 어디서든 변경이 가능하다.

```java
@Component
public class OrderServiceImpl implements OrderService {
	private MemberRepository memberRepository;
	private DiscountPolicy discountPolicy;
	
	@Autowired
	public void setMemberRepository(MemberRepository memberRepository) {
	    this.memberRepository = memberRepository;
	}
	
	@Autowired
	public void setDiscountPolicy(DiscountPolicy discountPolicy) {
	    this.discountPolicy = discountPolicy;
	}
}
```

### 3. 필드 주입 **(Field Injection)**

- Class에 속한 Field에 @Autowired를 붙여서 사용
- 단점
    - 코드가 간결하지만, 외부에서 변경하기 힘들다.
    - 프레임워크에 의존적이고 객체지향적으로 좋지않다.
    - 필드 주입을 할 경우 DI 컨테이너 안에서만 작동 → 순수 자바 코드로 테스트 불가능, final 키워드를 통해 불변 속성이라고 볼 수도 없고, Setter로 가변 속성이라고 볼 수도 없다.

```java
@Component
public class OrderServiceImpl implements OrderService {
	
	@Autowired
	private MemberRepository memberRepository;
	
	@Autowired
	private DiscountPolicy discountPolicy;
}
```

### 생성자 주입을 써야하는 이유

1. **불변**
    1. 대부분의 의존 관계는 어플리케이션 종료까지 변경될 일이 없다.
    2. 만일 수정자 주입을 사용한다면 setter를 public으로 설정해야 한다. → 실수로 변경가능 = 좋은 방법이 아니다.
    3. 만일 생성자 주입을 사용한다면, 생성자 호출 시점에 1번 호출 → 불변한 설계 가능
2. **누락**
    1. 호출했을 때 NullPointerException가 발생한다면, 의존 관계 주입이 누락되었기 때문이다.
    2. 생성자 주입을 활용하면 주입 데이터 누락 시 컴파일 오류가 발생하고 누락되고 실행되는 일이 발생하지 않는다.
3. **final 키워드의 사용**
    1. 생성자 주입 사용 시 final 키워드 사용 가능 → 생성자를 통해서만 설정 가능
    2.  컴파일 오류를 통해 누락을 놓치지 않을 수 있다.
4. **순환참조 방지, 발견**
    1. 생성자 주입은 순환 참조를 방지할 수 있다.
    2. 개발을 하다 보면 여러 컴포넌트 간에 의존성이 생기는데, 필드 주입과 수정자 주입은 빈이 생성된 후에 참조를 하기 때문에 애플리케이션이 아무런 오류, 경고없이 구동된다.
    3. 실제 코드가 호출될 때까지 문제를 알 수 없다.
    4. 생성자를 통해 의존성을 주입하고 실행하면 BeanCurrentlyInCreationException 발생
    5. 의존 관계에 내용을 외부로 노출 시킴으로써 애플리케이션을 실행하는 시점에서 오류 체크 가능
5. **테스트에 용이하다.**

**생성자 주입 정리**

- 생정자 주입 → 순수한 자바 언어의 특징을 잘 살리는 방법이다
- 기본적으로 생성자 주입을 사용하고 필수값이 아닌 경우, Setter 주입 방식을 옵션으로 부여하자 → 생성자 주입과 Setter 주입을 동시에 사용할 수 있다.
- 필드 주입은 사용하지 말자

### 출처)

[https://m42-orion.tistory.com/100](https://m42-orion.tistory.com/100)

[https://codingnojam.tistory.com/9](https://codingnojam.tistory.com/9)

[https://dev-coco.tistory.com/70](https://dev-coco.tistory.com/70)

[https://ittrue.tistory.com/227](https://ittrue.tistory.com/227)