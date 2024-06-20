# 반복자(Iterator) 패턴

- 일련의 데이터 집합에 대하여 순차적인 접근(순회)을 지원하는 패턴
- 해시, 트리와 같은 컬렉션은 데이터 저장 순서가 정해지지 않고 적재되기 때문에, 순회하는 알고리즘 전략을 정의하는 것을 이터레이터 패턴이라고 한다.
- 컬렉션 객체 안에 들어있는 모든 원소들에 대한 접근 방식이 공통화 되어 있다면 어떤 종류의 컬렉션에서도 이터레이터만 뽑아내면 여러 전략으로 순회가 가능해 보다 다형적인 코드를 설계 가능
- 별도의 이터레이터 객체를 반환 받아 이를 이용해 순회하기 때문에, 집합체의 내부 구조를 노출하지 않고 순회 할 수 있다는 장점

## 데이터 집합

- 객체들을 그룹으로 묶어 자료의 구조를 취하는 컬렉션
- 리스트, 트리, 그래프, 테이블 등

# 패턴 사용 시기

- 컬렉션에 상관없이 객체 접근 순회 방식을 통일하고자 할 때
- 컬렉션을 순회하는 다양한 방법을 지원하고 싶을 때
- 컬렉션의 복잡한 내부 구조를 클라이언트로 부터 숨기고 싶은 경우 (편의 + 보안)
- 데이터 저장 컬렉션 종류가 변경 가능성이 있을 때
    - 클라이언트가 집합 객체 내부 표현 방식을 알고 있다면, 표현 방식이 달라지면 클라이언트 코드도 변경되어야 하는 문제가 생긴다.

# 패턴 장점

- 일관된 이터레이터 인터페이스를 사용해 여러 형태의 컬렉션에 대해 동일한 순회 방법을 제공
- 컬렉션의 내부 구조 및 순회 방식을 알지 않아도 된다.
- 집합체의 구현과 접근하는 처리 부분을 객체로 분리해 결합도를 줄일 수 있다.
    - client에서 iterator로 접근하기 떄문에 ConcreteAggregate 내에 수정 사항이 생겨도 iterator에 문제가 없다면 문제가 발생하지 않는다.
- 순회 알고리즘을 별도의 반복자 객체에 추출하여 각 클래스의 책임을 분리하여 단일 책인 원칙(SRP)를 준수
- 데이터 저장 컬렉션 종류가 변경되어도 클라이언트 구현 코드는 손상되지 않아 개방 폐쇠 원칙(OCP)를 준수

# 패턴 단점

- 클래스가 늘어나고 복잡도가 증가
- 구현 방법에 따라 캡슐화를 위배할 수 있음
- 
<img width="721" alt="Untitled" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/a71d110d-3177-409e-8520-c85263592eba">

1. Aggeregate(인터페이스): ConcreateIterator 객체를 반환
    - iterator(): ConcreteIterator 객체를 만드는 팩토리 메서드
2. ConcreteAggregate(클래스): 여러 요소들이 이루어져 있는 데이터 집합체
3. Iterator(인터페이스): 집합체 내의 요소들을 순서대로 검색하기 위한 인터페이스를 제공
    - hasNext(): 순회할 다음 요소가 있는지 확인(true/false)
    - next(): 요소를 반환하고 다음 요소를 반환할 준비를 하기 위해 커서를 이동시킴
4. ConcreteIterator(클래스): 반복자 객체
    - ConcreteAggregate가 구현할 메서드로부터 생성되며, ConcreteAggregate 의 컬렉션을 참조하여 순회
    - 어떤 전략으로 순회할지에 대한 로직을 구체화

## 클래스 구성

```java
// 집합체 객체 (컬렉션)
interface Aggregate {
    Iterator iterator();
}

class ConcreteAggregate implements Aggregate {
    Object[] arr; // 데이터 집합 (컬렉션)
    int index = 0;

    public ConcreteAggregate(int size) {
        this.arr = new Object[size];
    }

    public void add(Object o) {
        if(index < arr.length) {
            arr[index] = o;
            index++;
        }
    }

    // 내부 컬렉션을 인자로 넣어 이터레이터 구현체를 클라이언트에 반환
    @Override
    public Iterator iterator() {
        return new ConcreteIterator(arr);
    }
}
```

```java
// 반복체 객체
interface Iterator {
    boolean hasNext();
    Object next();
}

class ConcreteIterator implements Iterator {
    Object[] arr;
    private int nextIndex = 0; // 커서 (for문의 i 변수 역할)

    // 생성자로 순회할 컬렉션을 받아 필드에 참조 시킴
    public ConcreteIterator(Object[] arr) {
        this.arr = arr;
    }

    // 순회할 다음 요소가 있는지 true / false
    @Override
    public boolean hasNext() {
        return nextIndex < arr.length;
    }

    // 다음 요소를 반환하고 커서를 증가시켜 다음 요소를 바라보도록 한다.
    @Override
    public Object next() {
        return arr[nextIndex++];
    }
}
```

## 클래스 흐름

```java
public static void main(String[] args) {
    // 1. 집합체 생성
    ConcreteAggregate aggregate = new ConcreteAggregate(5);
    aggregate.add(1);
    aggregate.add(2);
    aggregate.add(3);
    aggregate.add(4);
    aggregate.add(5);

    // 2. 집합체에서 이터레이터 객체 반환
    Iterator iter = aggregate.iterator();

    // 3. 이터레이터 내부 커서를 통해 순회
    while(iter.hasNext()) {
        System.out.printf("%s → ", iter.next()); // 1 -> 2 -> 3 -> 4 -> 5
    }
}
```

출처

[https://inpa.tistory.com/entry/GOF-💠-반복자Iterator-패턴-완벽-마스터하기](https://inpa.tistory.com/entry/GOF-%F0%9F%92%A0-%EB%B0%98%EB%B3%B5%EC%9E%90Iterator-%ED%8C%A8%ED%84%B4-%EC%99%84%EB%B2%BD-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0)
