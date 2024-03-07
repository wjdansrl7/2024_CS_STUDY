# List

## 1. ArrayList

- 특징
    - 담을 값들의 수가 정해지지 않았을 때 사용한다. **가변적**
    - 연속적인 데이터 리스트
    - 인덱스를 활용하여 요소에 빠르게 접근
    - 배열과 같이 리스트 중간에 삽입/삭제를 할 경우 나머지 요소들의 위치를 조정하는데 시간이 오래걸린다.
    - **조회를 많이 하는 경우에 효과적**
- 생성

```java
ArrayList<Integer> al = **new** ArrayList<>(); 
```

- 추가 / 변경

```java
colors.add("White"); // 끝에 추가하기
colors.add(2, "Green"); // index 2에 추가하기 (뒤의 값들은 밀린다.)
colors.set(0, "Blue"); // index 0에 덮어쓰기
colors.addAll(colors2); // Collections 통으로 추가
colors.addAll(2, colors3); // 작성한 index에 추가, 원래 값들은 뒤로 밀린다.
```

- 삭제

```java
String removedColor = colors.remove(0); // 특정 index 값 삭제하기, 인덱스를 이용할 시 값 리턴
System.out.println("Removed color is " + removedColor);
colors.remove("White"); // 직접 값 입력해서 삭제하기
colors.clear(); // ArrayList 비우기
```

- 리스트 순회

```java
// for-each loop
for (String color : colors) {
    System.out.print(color + "  ");
}

// for loop
for (int i = 0; i < colors.size(); ++i) {
    System.out.print(colors.get(i) + "  ");
}
```

- 값 유무 확인

```java
ArrayList<String> colors = new ArrayList<>(Arrays.asList("Black", "White", "Green", "Red"));
boolean contains = colors.contains("Black"); // 값 있는지 확인
System.out.println(contains);
int index = colors.indexOf("Blue"); // 값이 존재하는 경우, 값의 index 리턴, 없으면 -1 리턴
System.out.println(index);
index = colors.indexOf("Red");
System.out.println(index);
colors.isEmpty() // 비어있는지 확인
colors.size() // 들어있는 값 개수 확인
colors.get(i); // i번째 값 확인
colors.subList(0, 3); // index 0 ~ 2까지 값 확인
```

- 정렬

```java
colors.sort(); // 원본 리스트 자체를 정렬한다.
```

- 주의사항
    1. 그래프 관련 문제 풀이 시 정점 별 인접리스트를 각각 초기화 해줘야한다.
    
    ```java
    		ArrayList<Node>[] graph = new ArrayList[n + 1];
    		for (int i = 0; i <= n; i++) {
    			graph[i] = new ArrayList<>();
    		}
    ```
    

## 2. LinkedList

- 특징
    - 노드(객체)를 연결하여 리스트 처럼 만든 컬렉션
    - k번째 원소를 확인/변경하기 위해 O(k) 필요
    - **임의의 위치에 원소를 추가/삭제 O(1), 추가/삭제가 빠르다.**
    - **데이터의 중간 삽입, 삭제가 빈번할 경우 빠른 성능을 보장**
    - 데이터 저장순서가 유지되고 중복을 허용
- 생성

```java
LinkedList<String> list = new LinkedList<>();
LinkedList<String> list = new LinkedList<>(Arrays.asList("A", "B", "C")));
```

- 추가 / 삽입

![출처) [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)](images/ll2.png)

출처) [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)

```java
list.addFirst("new");
list.addLast("new");
list.add(1, "new"); // index 1 중간 위치에 추가
```

- 삭제

![출처) [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)](images/ll3.png)

출처) [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)

```java
list.removeFirst();
list.removeLast();
list.remove(1); // index 1 중간 위치 제거
```

- 요소 검색

![출처) [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)](images/ll4.png)

출처) [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)

```java
// 해당 값을 가지고 있는 요소 위치를 반환 (앞에서 부터 검색) 
list1.indexOf("B"); // 1

// 해당 값을 가지고 있는 요소 위치를 반환 (뒤에서 부터 검색) 
list1.lastIndexOf("D"); // -1 : 찾고자 하는 값이 없으면

// list1안에 "1" 값이 있는지 확인
list1.contains("1"); // true

LinkedList list2 = new LinkedList();
list2.add("1");
list2.add("2");
 
// list1에 list2의 모든 노드가 포함되어 있는지 확인
list1.containsAll(list2); // true
```

- 요소 얻기

![출처) [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)](images/ll5.png)

출처) [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)

```java
list.get(0); // 0번째 index 요소의 값 출력
```

- 요소 변경

![출처) [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)](images/ll6.png)

출처) [https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)

```java
list1.set(1, "A"); // index 1번의 데이터를 문자열 "A"로 변경한다.
System.out.println(list1); // // [10, A, 30]
```

- 순회

```java
LinkedList<String> list = new LinkedList<>();

// for 문을 이용한 순회
for (String data : list) {
    System.out.println(data);
}
```

- 구현

```java
package solve;

import java.util.Arrays;
import java.util.Objects;

public class LinkedList<E> {
	Node<E> head;
	Node<E> tail;

	int size;

	static class Node<E> {
		E data;
		Node<E> next;

		public Node(E data) {
			this.data = data;
			this.next = null;
		}

		public Node(E data, Node<E> next) {
			this.data = data;
			this.next = next;
		}

		public E getData() {
			return data;
		}
	}

	public LinkedList() {
		head = null;
		tail = null;
		size = 0;
	}

	// data 값으로 조회
	public Node<E> search(E data) {
		Node<E> cur = head;
		while (cur != null) {
			if (data == cur.getData()) {
				return cur;
			} else
				cur = cur.next;
		}
		return cur;
	}

	// index 값으로 조회
	public Node<E> search(int index) {
		Node<E> cur = head;
		for (int i = 0; i < index; i++) {
			cur = cur.next;
		}
		return cur;
	}

	// 가장 앞에 값 넣기
	public void addFirst(E data) {
		Node<E> first = head;
		Node<E> newNode = new Node<>(data, first);
		size++;
		head = newNode;
		if (first == null) { // 최초 add -> head = tail
			tail = newNode;
		}
	}

	// 마지막에 값 넣기
	public void addLast(E data) {
		Node<E> last = tail;
		Node<E> newNode = new Node<>(data);
		size++;
		tail = newNode;
		if (last == null) { // 최초 add -> head = tail
			head = newNode;
		} else { // 최초가 아니라면 last -> 추가된 노드 가리키게
			last.next = newNode;
		}
	}

	public boolean add(E value) {
		addLast(value);
		return true;
	}

	// 특정 위치에 삽입
	public void add(int index, E data) {
		// IndexOutOfBoundsException 에러 처리
		if (index < 0 || index >= size) {
			throw new IndexOutOfBoundsException();
		}

		if (index == 0) {
			addFirst(data);
			return;
		}

		if (index == size - 1) {
			addLast(data);
			return;
		}

		// 추가하려는 위치의 이전 노드 얻기
		Node<E> prev_node = search(index - 1);

		// 추가하려는 위치의 다음 노드 얻기
		Node<E> next_node = prev_node.next;

		// 새 노드 생성 (바로 다음 노드와 연결)
		Node<E> newNode = new Node<>(data, next_node);

		size++;

		prev_node.next = newNode;
	}

	public E removeFirst() {

		// IndexOutOfBoundsException 에러 처리
		if (head == null) {
			throw new IndexOutOfBoundsException();
		}

		// 삭제될 첫번째 요소의 데이터 기록
		E returnValue = head.data;

		// 두번째 노드를 임시 저장
		Node<E> first = head.next;

		// 첫번째 노드의 내부 요소를 모두 삭제
		head.next = null;
		head.data = null;

		// head 다음 노드를 가리키도록 업데이트
		head = first;

		size--;

		// 만일 리스트의 유일한 값을 삭제해서 빈 리스트가 될 경우, tail -> null
		if (head == null) {
			tail = null;
		}

		// 삭제된 요소를 반환
		return returnValue;
	}

	public E remove() {
		return removeFirst();
	}

	public E remove(int index) {

		// IndexOutOfBoundsException 에러 처리
		if (index < 0 || index >= size) {
			throw new IndexOutOfBoundsException();
		}

		if (index == 0) {
			return removeFirst();
		}

		// 삭제할 위치의 이전 노드 저장
		Node<E> prev_node = search(index - 1);

		// 삭제할 위치의 노드 저장
		Node<E> del_node = prev_node.next;

		// 삭제할 위치의 다음 노드 저장
		Node<E> next_node = del_node.next;

		// 삭제될 데이터를 기록
		E returnValue = del_node.data;

		// 삭제 노드의 내부 요소를 모두 삭제
		del_node.next = null;
		del_node.data = null;

		size--;

		prev_node.next = next_node;

		// 다음이 없으면 tail 업데이트
		if (prev_node.next == null) {
			tail = prev_node;
		}

		return returnValue;
	}

	public boolean remove(Object data) {

		// 삭제할 요소가 아무것도 없으면 에러
		if (head == null) {
			throw new RuntimeException();
		}

		// 이전 노드, 삭제 노드, 다음 노드를 저장할 변수 선언
		Node<E> prev_node = null;
		Node<E> del_node = null;
		Node<E> next_node = null;

		// 루프 변수 선언
		Node<E> cur = head;

		// 순회하면서 값 찾기
		while (cur != null) {
			if (Objects.equals(cur.data, data)) {
				// 노드의 값과 매개변수 값이 같으면 삭제 노드에 요소를 대입하고 break
				del_node = cur;
				break;
			}

			// 이전 노드 정보가 없기 때문에 기록
			prev_node = cur;

			cur = cur.next;
		}

		// 찾은 요소가 없다면 리턴
		if (del_node == null) {
			return false;
		}

		// 삭제하려는 노드가 head -> 첫번째 요소를 삭제하는 것이니 removeFirst()를 사용
		if (del_node == head) {
			removeFirst();
			return true;
		}

		// 다음 노드에 삭제 노드 next 대입
		next_node = del_node.next;

		// 삭제 요소 데이터 모두 제거
		del_node.next = null;
		del_node.data = null;

		size--;

		prev_node.next = next_node;

		// 다음이 없으면 tail 업데이트
		if (prev_node.next == null) {
			tail = prev_node;
		}
		return true;
	}

	public E removeLast() {
		return remove(size - 1);
	}

	public E get(int index) {
		if (index < 0 || index >= size) {
			throw new IndexOutOfBoundsException();
		}

		return search(index).data;
	}

	// 값 수정하기
	public void set(int index, E data) {
		if (index < 0 || index >= size) {
			throw new IndexOutOfBoundsException();
		}

		Node<E> replace_node = search(index);

		replace_node.data = data;
	}

	// 전체 출력
	void print() throws Exception {
		Node<E> cur = head.next;
		while (cur.next != null) {
			System.out.print(cur.data + " ");
			cur = cur.next;
		}
	}

	@Override
	public String toString() {
		if (head == null) {
			return "[]";
		}

		Object[] array = new Object[size];

		int index = 0;
		Node<E> cur = head;
		while (cur != null) {
			array[index] = (E) cur.data;
			index++;
			cur = cur.next;
		}

		return Arrays.toString(array);
	}

	public static void main(String[] args) {

		LinkedList<Number> l = new LinkedList<>();

		l.add(3);
		l.add(6);
		l.add(4);
		l.add(3);
		l.add(8);
		l.add(10);
		l.add(11);

		System.out.println(l); // [3, 6, 4, 3, 8, 10, 11]

		l.add(6, 100);
		l.add(0, 101);
		l.add(1, 102);
		System.out.println(l); // [101, 102, 3, 6, 4, 3, 8, 10, 11, 100]

		l.removeLast();
		System.out.println(l); // [101, 102, 3, 6, 4, 3, 8, 10, 11]
		l.removeLast();
		System.out.println(l); // [101, 102, 3, 6, 4, 3, 8, 10]
		l.add(9);
		System.out.println(l); // [101, 102, 3, 6, 4, 3, 8, 10, 9]
		l.add(7);
		System.out.println(l); // [101, 102, 3, 6, 4, 3, 8, 10, 9, 7]

		l.removeFirst();
		l.removeFirst();
		l.remove(1);
		System.out.println(l); // [3, 4, 3, 8, 10, 9, 7]

		l.remove(3);
		l.remove(3);
		System.out.println(l); // [3, 4, 3, 9, 7]

		System.out.println(l.get(4)); // 7

		l.set(4, 999);
		System.out.println(l); // [3, 4, 3, 9, 999]

	}
}

```

## 3. 배열 vs 연결 리스트

![출처) [https://blog.encrypted.gg/932](https://blog.encrypted.gg/932)](images/ll7.png)

출처) [https://blog.encrypted.gg/932](https://blog.encrypted.gg/932)

### ref)

[https://psychoria.tistory.com/765](https://psychoria.tistory.com/765)

[https://blog.encrypted.gg/932](https://blog.encrypted.gg/932)

[https://inpa.tistory.com/entry/JAVA-☕-ArrayList-구조-사용법](https://inpa.tistory.com/entry/JAVA-%E2%98%95-ArrayList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95)

[https://inpa.tistory.com/entry/JAVA-☕-LinkedList-구조-사용법-완벽-정복하기](https://inpa.tistory.com/entry/JAVA-%E2%98%95-LinkedList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95-%EC%99%84%EB%B2%BD-%EC%A0%95%EB%B3%B5%ED%95%98%EA%B8%B0)

[https://inpa.tistory.com/entry/DS-🧱-Singly-LinkedList-자료구조-Java로-실전-구현하기](https://inpa.tistory.com/entry/DS-%F0%9F%A7%B1-Singly-LinkedList-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0-Java%EB%A1%9C-%EC%8B%A4%EC%A0%84-%EA%B5%AC%ED%98%84%ED%95%98%EA%B8%B0)