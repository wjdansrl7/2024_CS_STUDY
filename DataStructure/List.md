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
    

### ref)

[https://psychoria.tistory.com/765](https://psychoria.tistory.com/765)

[https://inpa.tistory.com/entry/JAVA-☕-ArrayList-구조-사용법](https://inpa.tistory.com/entry/JAVA-%E2%98%95-ArrayList-%EA%B5%AC%EC%A1%B0-%EC%82%AC%EC%9A%A9%EB%B2%95)