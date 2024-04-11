# Eager, Lazy Loading

### 즉시 로딩: 엔티티를 조회할 때 자신과 연관되는 엔티티를 조인(join)을 통해 함께 조회하는 방식

- 엔티티 A 조회시 관련되어 있는 엔티티 B를 같이 가져온다. 실제 엔티티를 맵핑한다. Join을 사용하여 한번에 가져온다.
- A join B, 쿼리가 한 번만 나간다

### 지연 로딩: 자신과 연관된 엔티티를 실제로 사용할 때 연관된 엔티티를 조회(SELECT) 하는 방식

- 엔티티 A를 조회시 관련(Reference)되어 있는 엔티티 B를 한번에 가져오지 않는다. 프록시를 맵핑하고 실제 B를 조회할때 쿼리가 나간다.
- 쿼리가 두 번 나간다. A 조회시 한 번, B 조회시 한 번
- 조회 대상이 영속성 컨텍스트에 이미 있으면 프록시 객체가 아닌 실체 객체를 사용한다.

```java
@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;

    private String name;

    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();
}
```

```java

Member를 조회할 때, Team도 함게 조회해야 할까?

println(member.getName()); // 단순히 member 정보만 사용하는 비즈니스 로직

// 지연 로딩 LAZY을 사용해서 프록시로 조회

@Entity
public class Member {

	@Id
	@GeneratedValue
	private Long id;
	
	@Column(name="USERNMAME")
	private String name;
	
	@ManyToOne(fetch=FetchType.LAZY)
	@JoinColumn(nmae="TEAM_ID")
	private Team team;
	...
	
	}
```

```java
// Member와 Team을 자주 함께 사용한다면?

@Entity
public class Member {

	@Id
	@GeneratedValue
	private Long id;
	
	@Column(name="USERNMAME")
	private String name;
	
	@ManyToOne(fetch=FetchType.EAGER)
	@JoinColumn(nmae="TEAM_ID")
	private Team team;
	...
	
	}
```

```java
// 예제 코드
public static void main(String[] args) {
	...
	Team team = new Team();
	team.setName("teamA");
	em.persist(team);
	
	Member member = new Member();
	member.setUsername("member1");
	member.setTeam(team);
	em.persist(member);
	
	em.flush();
	em.clear();
	
	Member findMember = em.find(Member.class, member.getId());
}
	

```
![Untitled](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/f96e0e74-174e-4093-99d7-e857269e4ea9)


### 로그(EAGER)

```java
Hibernate: 
    select
        member0_.MEMBER_ID as MEMBER_I1_0_0_,
        member0_.TEAM_ID as TEAM_ID3_0_0_,
        member0_.USERNAME as USERNAME2_0_0_,
        team1_.TEAM_ID as TEAM_ID1_1_1_,
        team1_.name as name2_1_1_ 
    from
        Member member0_ 
    left outer join
        Team team1_ 
            on member0_.TEAM_ID=team1_.TEAM_ID 
    where
        member0_.MEMBER_ID=?
```

### 로그(LAZY)

```java
Hibernate: 
    select
        member0_.MEMBER_ID as MEMBER_I1_0_0_,
        member0_.TEAM_ID as TEAM_ID3_0_0_,
        member0_.USERNAME as USERNAME2_0_0_ 
    from
        Member member0_ 
    where
        member0_.MEMBER_ID=?
```

```java
findMember.getTeam().getName();		// 코드 자체엔 큰 의미가 없음, team 객체의 데이터를 요구하기 위한 코드

Hibernate: 
    select
        team0_.TEAM_ID as TEAM_ID1_1_0_,
        team0_.name as name2_1_0_ 
    from
        Team team0_ 
    where
        team0_.TEAM_ID=?
```

## **즉시 로딩(Eager Loading) vs 지연 로딩(Lazy Loading)**

- 즉시 로딩(Eager Loading)
    - 장점
        - 연관된 엔티티를 모두 가져올 수 있음
    - 단점
        - 엔티티간의 관계가 복잡해질수록 **조인으로 인한 성능 저하** 가 나타날 수 있음
            - 불필요한 조인까지 포함해서 처리하는 경우가 많음
            - 예를 들어 Order 연관된 객체가 Member가 N개라면, Order 1개 조회 시 필요하지 않은 Member 객체를 조회하는 쿼리 N개가 생성됨
        - JPQL에서 N+1 문제를 일으킴
- 지연 로딩(Lazy Loading)
    - 장점
        - 다른 접근 방식보다 훨씬 적은 초기의 로딩 시간
        - 다른 접근 방식에 비해 메모리 소비량 감소
    - 단점
        - 초기화가 지연되면 원하지 않는 순간 성능에 영향을 줄 수 있음

### 참고

모든 연관관계는 지연로딩으로 설정하자!

- 즉시 로딩(Eager)는 예측이 어렵고, 어떤 SQL이 실행될지 추적하기 어렵다. 특히 JPQL을 실행할 때 N+1 문제가 자주 발생한다
- 실무에서 모든 연관관계는 지연로딩(LAZY)으로 설정해야 한다.
- 연관된 엔티티를 함께 DB에서 조회해야 하면, fetch join 또는 엔티티 그래프 기능을 사용한다.
- @XToOne(OneToOne, ManyToOne) 관계는 기본이 즉시로딩이므로 직접 지연로딩으로 설정해야 한다.

## 추가

### 지연 로딩은 어떻게 이루어질 수 있는가? -> "프록시 객체"

객체 조회 시 항상 연관된 객체까지 함께 조회하는 것은 효율적이지 않다.

그래서 JPA는 엔티티가 실제로 사용되기 전까지 데이터베이스 조회를 지연할 수 있도록 제공하는데 이를 **"지연 로딩"** 이라고 한다.

> "실제로 사용하는 시점에 데이터베이스에서 필요한 데이터를 가져오는 것"
> 

하지만 지연 로딩을 사용하면 실제 엔티티 객체 대신 가짜 객체가 필요한데, 이것이 바로 **"프록시 객체"**

ManyToOne 관계인 Order(N) - Member(1) 를 예로 들어 보자.

엔티티 Order 조회 시 관련 되어 있는 엔티티 Member를 한번에 가져오지 않는다.

프록시를 맵핑하고 실제 Member를 조회할 때 쿼리가 나간다.

![Untitled 1](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/621c7012-77b2-434d-940f-9b9f56eb2657)


- 지연로딩을 설정해놓으면, Order find 메소드 호출 시 Member는 Proxy 상태이다.
- order.getMember(); 메소드를 호출하는 순간 JPA는 DB를 조회하여 Proxy에 값을 채운다. (바꾸는 게 아니라 값을 채워넣는 것!)
- 프록시는 실제 엔티티를 상속받은 오브젝트이다. 따라서 Member 엔티티와 겉모양이 같고 JPA는 이 프록시(Target)에 값을 채워넣는 것이다.

참고자료

[https://velog.io/@ssssujini99/SpringJPA-즉시-로딩Eager-Loading과-지연-로딩Lazy-Loading](https://velog.io/@ssssujini99/SpringJPA-%EC%A6%89%EC%8B%9C-%EB%A1%9C%EB%94%A9Eager-Loading%EA%B3%BC-%EC%A7%80%EC%97%B0-%EB%A1%9C%EB%94%A9Lazy-Loading)

[https://velog.io/@jin0849/JPA-즉시로딩EAGER과-지연로딩LAZY](https://velog.io/@jin0849/JPA-%EC%A6%89%EC%8B%9C%EB%A1%9C%EB%94%A9EAGER%EA%B3%BC-%EC%A7%80%EC%97%B0%EB%A1%9C%EB%94%A9LAZY)
