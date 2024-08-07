# JPA와 Hibernate

# **JPA(Java Persistence API)**

- 자바  ORM(Object Relational Mapping) 기술에 대한 API 표준 명세(인터페이스)
- 자바 어플리케이션에서 관계형 데이터베이스를 어떻게 사용하는지를 정의
- 패키지의 대부분은 `interface`, `enum`, `Exception`, 그리고 각종 `Annotation`으로 이루어져 있다.

## ORM(Object-Relational Mapping)

우리가 일반적으로 알고 있는 애플리케이션 Class와 RDB(Relational DataBase)의 테이블을 매핑(연결)한다는 뜻이며, 기술적으로는 어플리케이션의 객체를 RDB 테이블에 자동으로 영속화 해주는 것이라고 보면된다.

### ORM의 장점

- SQL문이 아닌 Method를 통해 DB를 조작할 수 있어, 개발자는 객체 모델을 이용하여 비즈니스 로직을 구성하는데만 집중할 수 있음.(내부적으로는 쿼리를 생성하여 DB를 조작함. 하지만 개발자가 이를 신경 쓰지 않아도됨)
- Query와 같이 필요한 선언문, 할당 등의 부수적인 코드가 줄어들어, 각종 객체에 대한 코드를 별도로 작성하여 코드의 가독성을 높임
- 객체지향적인 코드 작성이 가능하다. 오직 객체지향적 접근만 고려하면 되기때문에 생산성 증가
- 매핑하는 정보가 Class로 명시 되었기 때문에 ERD를 보는 의존도를 낮출 수 있고 유지보수 및 리팩토링에 유리
- 예를들어 기존 방식에서 MySQL 데이터베이스를 사용하다가 PostgreSQL로 변환한다고 가정해보면, 새로 쿼리를 짜야하는 경우가 생김. 이런 경우에 ORM을 사용한다면 쿼리를 수정할 필요가 없음

### ORM의 단점

- 프로젝트의 규모가 크고 복잡하여 설계가 잘못된 경우, 속도 저하 및 일관성을 무너뜨리는 문제점이 생길 수 있음
- 복잡하고 무거운 Query는 속도를 위해 별도의 튜닝이 필요하기 때문에 결국 SQL문을 써야할 수도 있음
- 학습비용이 비쌈

**자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 인터페이스**

![Untitled](https://github.com/user-attachments/assets/2705e7c7-0bf5-4a30-b169-cf97b8998d86)

### 저장

![Untitled 1](https://github.com/user-attachments/assets/76d5cee8-e619-41cf-982f-c72b65f32d96)

### 조회

![Untitled 2](https://github.com/user-attachments/assets/c8022b09-564a-45b3-9714-ec8bf5690e2b)

```java
package javax.persistence;
import ...
public interface EntityManager {
     public void persist(Object entity);
     public <T> T merge(T entity);
     public void remove(Object entity);
     public <T> T find(Class<T> entityClass, Object primaryKey);
     
     
     // More interface methods...}
```

# **Hibernate 란?**

- Hibernate는 JPA의 구현체로, JPA의 구현체로 JPA의 인터페이스를 구현하며, 내부적으로 JDBC API 사용한다.
- Hibernate는 SQL을 사용하지 않고 직관적인 코드(메소드)를 사용해 데이터를 조작할 수 있습니다.((Hibernate가 지원하는 메소드 내부에서는 JDBC API가 동작하고 있으며, 단지 개발자가 직접 SQL을 작성하지 않을 뿐 입니다.)
- Hibernate를 사용하면 상속, 다형성, 연결, 구성 및 JAVA 컬렉션 프레임워크를 포함하여 자연스러운 객체 지향 관용구를 따르는 영구 클래스를 개발할 수 있다.
- **JPA를 사용하기 위해서 반드시 Hibernate를 사용할 필요가 없다.** Hibernate의 작동 방식이 마음에 들지 않는다면 언제든지 DataNucleus, EclipseLink 등 다른 JPA 구현체를 사용해도 되고, 심지어 본인이 직접 JPA를 구현해서 사용할 수도 있다. 다만 그렇게 하지 않는 이유는 단지 Hibernate가 굉장히 성숙한 라이브러리
- 개발자 생산성과 런타임 성능 측면에서 JDBC 코드 보다 우수한 성능을 일관되게 제공함.

<img width="872" alt="Untitled 3" src="https://github.com/user-attachments/assets/bb865808-fc6a-4b00-8c2c-19883bfbb1a1">

![Untitled 4](https://github.com/user-attachments/assets/a9599b93-72db-4390-8995-d54f6826ae90)

# **Hibernate의 장단점**

## **장점**

- **생산성**
    - Hibernate는 SQL을 직접 사용하지 않고, 메소드 호출만으로 query가 수행된다.
    - 즉, 반복적인 SQL 작업과 CRUD 작업을 직접 하지 않으므로 생산성이 매우 높아진다.
- **유지보수**
    - 테이블 컬럼이 변경되었을 경우,
    - Mybatis에서는 관련 DAO의 파라미터, 결과, SQL 등을 모두 확인하여 수정해야 하지만
    - JPA는 JPA가 이런 일들을 대신 해주기 때문에 유지보수 측면에서 좋다.
- **객체지향적 개발**
    - 객체지향적으로 데이터를 관리할 수 있기 때문에 비즈니스 로직에 집중할 수 있다.
    - 로직을 쿼리에 집중하기 보다 객체 자체에 집중할 수 있다.
- **특정 벤더에 종속적이지 않다.**
    - 여러 DB 벤더 (MySQL, ORACLE 등 . .)마다 SQL 사용이 조금씩 다르기 때문에 어플리케이션 개발 시 처음 선택한 DB를 나중에 바꾸는 것은 매우 어렵다.
    - 하지만 JPA는 추상화된 데이터 접근 계층을 제공하기 때문에 특정 벤더에 종속적이지 않다.
        - 설정 파일에서 JPA에게 어떤 DB를 사용하고 있는지 알려주기만 하면 얼마든지 DB를 변경할 수 있다.

**단점**

- **어렵다.**
    - 많은 내용이 감싸져 있기 때문에 JPA를 잘 사용하기 위해서는 알아야 할 것이 많다.
    - 잘 이해하고 사용하지 않으면 데이터 손실이 있을 수 있다.
- **성능**
    - 메소드 호출로 쿼리를 실행하는 것은 내부적으로 많은 동작이 있다는 것을 의미하므로, 직접 SQL을 호출하는것보다 성능이 떨어질 수 있다.
    - 실제로 초기의 ORM은 쿼리가 제대로 수행되지 않았고, 성능도 좋지 못했다고 한다.
        - 그러나 지금은 많이 발전하고 있고, 좋은 성능을 보여주고 있다.
- **세밀함이 떨어진다.**
    - 메소드 호출로 SQL을 실행하기 때문에 세밀함이 떨어진다. 또한 객체간의 매핑 (Entity Mapping)이 잘못되거나 JPA를 잘못 사용하여 의도하지 않은 동작을 할 수도 있다.
    - 복잡한 통계 분석 쿼리를 메소드 호출로 처리하는 것은 힘들다.
        - 이것을 보완하기 위해 JPA에서는 SQL과 유사한 기술인 **JPQL**을 지원한다.
        - SQL 자체 쿼리를 작성할 수 있도록 지원도 하고 있다.

**Spring Data JPA**

- **JPA를 쓰기 편하게 만들어놓은 모듈**
- **JPA를 한 단계 추상화시킨 `Repository`라는 인터페이스를 제공함으로써 이루어진다**.
- `Repository` 인터페이스에 정해진 규칙대로 메소드를 입력하면, Spring이 알아서 해당 메소드 이름에 적합한 쿼리를 날리는 구현체를 만들어서 Bean으로 등록

Spring Data JPA가 JPA를 추상화했다. ==  **Spring Data JPA의 `Repository`의 구현에서 JPA를 사용하고 있다**

- 아래 예시는 `Repository` 인터페이스의 기본 구현체인 `SimpleJpaRepository`의 코드를 보면 아래와 같이 내부적으로 `EntityManager`을 사용하고 있는 것을 보여준다.

```java
package org.springframework.data.jpa.repository.support;

import ...

public class SimpleJpaRepository<T, ID> implements JpaRepositoryImplementation<T, ID> {

    private final EntityManager em;

    public Optional<T> findById(ID id) {

        Assert.notNull(id, ID_MUST_NOT_BE_NULL);

        Class<T> domainType = getDomainClass();

        if (metadata == null) {
            return Optional.ofNullable(em.find(domainType, id));
        }

        LockModeType type = metadata.getLockModeType();

        Map<String, Object> hints = getQueryHints().withFetchGraphs(em).asMap();

        return Optional.ofNullable(type == null ? em.find(domainType, id, hints) : em.find(domainType, id, type, hints));
    }

    // Other methods...
}
```

참고자료:

[https://dev-coco.tistory.com/74](https://dev-coco.tistory.com/74)
