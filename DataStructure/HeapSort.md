# HeapSort

## Heap

- 완전 이진 트리의 일종으로 우선순위 큐를 위하여 만들어진 자료구조
- 최댓값, 최솟값을 쉽게 추출할 수 있는 자료구조

![삽입정렬예시](/DataStructure/images/heap.png)

## Heap 정렬 알고리즘

1. 정렬해야 할 n개의 요소를 최소힙으로 만든다.
2. 힙에서 하나씩 꺼낸다.

### 예제

[8, 5, 6, 2, 4] 배열 오름차순으로 정렬하기

### 코드

```java
import java.util.*;

public class HeapSort {

	public static void main(String[] args) {
		int[] arr= {8, 5, 6, 2, 4};
//		System.out.println("원본 "  + Arrays.toString(arr));

		PriorityQueue<Integer> pq = new PriorityQueue<>();
		for(int el : arr) pq.add(el); // heap에 넣기
		for(int i = 0; i < arr.length; i++)  arr[i] = pq.poll(); // heap에서 빼서 arr에 넣기
//		System.out.println("결과 " + Arrays.toString(arr));
	}
}


/*
원본 [8, 5, 6, 2, 4]
결과 [2, 4, 5, 6, 8]
*/
```

## 특징

- 장점
  1.  시간 복잡도가 좋은편
  2.  힙 정렬이 가장 유용한 경우는 전체 자료를 정렬하는 것이 아니라 가장 큰 값 몇개만 필요할 때 이다.

## 시간복잡도

- O(nlogn)
