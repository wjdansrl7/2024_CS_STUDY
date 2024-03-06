# SET
> 객체(데이터)를 중복해서 저장할 수 없다. <br/> 
> 저장된 객체(데이터)를 인덱스로 관리하지 않기 때문에 **저장 순서가 보장되지 않는다**.

## SET의 종류 : HashSet, TreeSet, LinkedHashSet

<img width="500" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_2 14 28" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/7bc80866-1992-4954-9a5f-cdfbab0b562f">

# HashSet<E> 클래스
- JDK 1.2부터 제공된 HashSet 클래스는 해시 알고리즘(hash algorithm)을 사용하여 검색 속도가 매우 빠름.
- 이러한 HashSet 클래스는 내부적으로 HashMap 인스턴스를 이용하여 요소를 저장 ⇒ 성능상 차이 없음!
    - 참고자료: [https://potwings.tistory.com/m/56](https://potwings.tistory.com/m/56)
- HashSet 클래스는 Set 인터페이스를 구현하므로, 요소를 순서에 상관없이 저장하고 중복된 값은 저장하지 않음.
- 요소의 저장 순서를 유지해야 한다면, JDK 1.4부터 제공하는 LinkedHashSet 클래스를 사용하면 됨.

Ex1) HashSet 생성 기본

```java
package a0306;

import java.util.*;
public class HashSetEx1 {

	public static void main(String[] args) {
		Set<String> set = new HashSet<>();
		set.add("one");
		set.add("two");
		set.add("three");
		set.add("one");
		set.add("two"); // add() 메소드를 사용하여 해당 HashSet에 이미 존재하는 요소를 추가하려고 하면, 해당 요소를 저장하지 않고 false를 반환
		set.add("4");
		set.add("6");
		set.add("six");
		
		System.out.println("저장된 데이터 수 : " + set.size());
		
		Iterator<String> it = set.iterator(); // Iterator(반복자) 생성
		
		while(it.hasNext()) { // hasNext(): 데이터가 있으면 true 없으면 false
			System.out.println(it.next()); // next(): 다음 데이터 리턴
		}
		
		System.out.println("-----------------------");
		
		set.remove("three"); // 데이터 제거
		System.out.println("저장된 데이터 수 : " + set.size()); // 저장된 데이터 수 출력
		it = set.iterator(); // 반복자 재생성(위의 반복문에서 next 메서드로 데이터를 다 가져왔기 대문에 재생성을 해야함)
		
		while(it.hasNext()) {
			System.out.println(it.next());
		}
		
		System.out.println("------------------------");
		
	}

}

/*
저장된 데이터 수 : 6
six
4
6
one
two
three
-----------------------
저장된 데이터 수 : 5
six
4
6
one
two
------------------------
*/
```

Ex2) HashSet에 객체를 저장하고 가져오는 방법

```java
package a0306;

import java.util.*;

class Member {
	String name;
	String id;
	
	public Member(String name, String id) {
		this.name = name;
		this.id = id;
	}
}
public class HashSetEx2 {
	
	public static void main(String[] args) {
		Set<Member> set = new HashSet<>();
		
		set.add(new Member("문기1", "moong")); // 객체 추가
		set.add(new Member("문기2", "moongsil"));
		
		Iterator<Member> it = set.iterator(); // 반복자 생성
		
		while(it.hasNext()) {
			Member mb = it.next(); // set에 저장된 다음 객체의 참조값 저장
			
			System.out.println("아이디 : " + mb.id);
			System.out.println("이름 : " + mb.name);
			System.out.println("------------------");
		}
	}
}

/*
아이디 : moongsil
이름 : 문기2
------------------
아이디 : moong
이름 : 문기1
------------------
*/
```

EX3) 입력받은 데이터를 객체에 저장하고 HashSet에 추가한 후 iterator, forEach문을 활용하여 출력

```java
package a0306;

import java.util.*;

class MemberInfo {
	private String name;
	private String id;
	
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	
	
}
public class HashSetEx3 {

	public static void main(String[] args) {
		MemberInfo mm; // MemberInfo 객체 선언
		Set<MemberInfo> set = new HashSet<>();
		Scanner sc = new Scanner(System.in);
		
		do {
			mm = new MemberInfo(); // 반복될 때마다 인스턴스 생성(반복문 외부에서 new 연산자 사용시 1개의 객체만 저장됨)
			
			System.out.println("이름 입력: ");
			String name = sc.next();
			
			System.out.println("아이디 입력: ");
			String id = sc.next();
			
			mm.setName(name);
			mm.setId(id);
			
			set.add(mm);
			
			System.out.println("계속 Y, 정지 N");
			String yn = sc.next();
			
			if(yn.equals("y") || yn.equals("Y")) {
				continue;
			} else {
				break;
			}
			
		} while(true);
		
		System.out.println("-----------------");
		
		Iterator<MemberInfo> it = set.iterator();
		
		while(it.hasNext()) {
			mm = it.next();
			System.out.println("이름 : " + mm.getName());
			System.out.println("아이디 : " + mm.getId());
			System.out.println("------------");
		}
		
		for(MemberInfo m : set) {
			System.out.println("이름 : " + m.getName());
			System.out.println("아이디 : " + m.getId());
			System.out.println("------------");
		}
	}
}

/*
이름 입력: 
문기
아이디 입력: 
moon
계속 Y, 정지 N
Y
이름 입력: 
메롱
아이디 입력: 
화이팅
계속 Y, 정지 N
N
-----------------
이름 : 문기
아이디 : moon
------------
이름 : 메롱
아이디 : 화이팅
------------
이름 : 문기
아이디 : moon
------------
이름 : 메롱
아이디 : 화이팅
------------
*/

```

# HashSet에 이미 존재하는 요소인지를 어떻게 파악할까?

HashSet에 이미 존재하는 요소인지를 파악하기 위해서

1. 해당 요소에서 hashCode() 메소드를 호출하여 반환된 해시값으로 검색할 범위를 결정합니다.
2. 해당 범위 내의 요소들을 equals() 메소드로 비교합니다.

⇒ HashSet에서 add() 메소드를 사용하여 중복 없이 새로운 요소를 추가하기 위해서는 hashCode()와 equals() 메소드를 상황에 맞게 오버라이딩

Ex4) 사용자가 정의한 Animal 클래스의 인스턴스를 HashSet에 저장하기 위해 hashCode()와 equals() 메소드를 오버라이딩한 예제

```java
class Animal {
    String species;
    String habitat;

    Animal(String species, String habitat) {
	    this.species = species;
	    this.habitat = habitat;
}

 

public int hashCode() { return (species + habitat).hashCode(); }
    public boolean equals(Object obj) {
        if(obj instanceof Animal) {
            Animal temp = (Animal)obj;
            return species.equals(temp.species) && habitat.equals(temp.habitat);
        } else {
            return false;
        }
    }
}

 

public class Set02 {

    public static void main(String[] args) {

        HashSet<Animal> hs = new HashSet<Animal>();

        hs.add(new Animal("고양이", "육지"));
        hs.add(new Animal("고양이", "육지"));
        hs.add(new Animal("고양이", "육지"));

        System.out.println(hs.size());
    }
}

// 1
```

# TreeSet<E> 클래스
> 중복된 데이터를 저장할 수 없고, 입력한 순서대로 값을 저장하지 않는다. <br/>
> TreeSet은 기본적으로 오름차순으로 데이터를 정렬

# TreeSet 특징
- TreeSet 클래스는 데이터가 정렬된 상태로 저장되는 이진 검색 트리(binary search tree)의 형태로 요소를 저장함.
- 이진 검색 트리는 데이터를 추가하거나 제거하는 등의 기본 동작 시간이 매우 빠름.
- JDK 1.2부터 제공되는 TreeSet 클래스는 NavigableSet 인터페이스를 기존의 이진 검색 트리의 성능을 향상시킨 레드-블랙 트리(Red-Black tree)로 구현함.
- TreeSet 클래스는 Set 인터페이스를 구현하므로, 요소를 순서에 상관없이 저장하고 중복된 값은 저장하지 않음.

```java
package a0306;

import java.util.*;

public class TreeSetEx1 {

	public static void main(String[] args) {
		Set<Integer> set = new TreeSet<>();

		set.add(4); // 데이터 추가
		set.add(2);
		set.add(1);
		set.add(3);
		set.add(1);
		set.add(3);

		Iterator<Integer> it = set.iterator(); // 반복자 생성

		while (it.hasNext()) {
			System.out.println(it.next());
		}

		System.out.println("----------");

		Set<String> ts = new TreeSet<String>();
		ts.add("a");
		ts.add("c");
		ts.add("d");
		ts.add("b");

		Iterator<String> itr = ts.iterator();
		while (itr.hasNext()) {
			System.out.println(itr.next());
		}
		
		System.out.println("----------");
		
		for(String s : ts) {
			System.out.println(s);
		}
	}

}

/*
1
2
3
4
----------
a
b
c
d
----------
a
b
c
d
*/
```

# LinkedHashSet<E> 클래스
> LinkedHashSet도 중복된 데이터를 저장할 수 없다.
> 차이점은 입력된 순서대로 데이터를 관리

```java
package a0306;

import java.util.*;

public class LinkedHashSetEx1 {

	public static void main(String[] args) {
		Set<String> set = new LinkedHashSet<String>();
		set.add("1");
		set.add("1");
		set.add("two");
		set.add("3");
		set.add("4");
		set.add("five");

		Iterator<String> it = set.iterator();
		while (it.hasNext()) {
			System.out.println(it.next());
		}

		System.out.println("--------------------------");

		LinkedHashSet<Integer> lh = new LinkedHashSet<Integer>();
		lh.add(1);
		lh.add(1);
		lh.add(4);
		lh.add(2);
		lh.add(3);
		Iterator<Integer> it2 = lh.iterator();

		while (it2.hasNext()) {
			System.out.println(it2.next());
		}
	}

}

/*
1
two
3
4
five
--------------------------
1
4
2
3

*/
```

<img width="500" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_3 17 10" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/a625a243-7b9d-4fa0-94a2-b51b0331a8d8">

뇌피셜 <br/>
~~그럼 맵과 차이가 대체 뭘까…?~~ <br/>
~~맵은 key, value로 저장할때 key값은 고유하지만 value는 중복이 가능, Set은 같은 값에 대해 중복 저장을 허용하지 않음.~~ <br/>
~~map은 해당 key를 알고 있다면 바로 value를 꺼내올 수 있음. set은 for문으로 돌아야함.~~ <br/>

참고자료
- [https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=heartflow89&logNo=220994601249](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=heartflow89&logNo=220994601249)
- [https://www.tcpschool.com/java/java_collectionFramework_set](https://www.tcpschool.com/java/java_collectionFramework_set)
