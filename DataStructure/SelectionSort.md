# SelectionSort

- 제자리 정렬 알고리즘

> 제자리 정렬 알고리즘 : 입력 배열(정렬되지 않은 값들) 이외에 다른 추가 메모리를 요구하지 않는 정렬 방법

- 해당 순서에 원소를 넣을 위치는 이미 정해져 있고, 어떤 원소를 넣을지 선택하는 알고리즘

## 삽입 정렬 알고리즘 구현

1. 0번째 이후의 어레이에서 가장 작은 값을 찾는다.
2. 찾은 값과 0번째를 교체한다.
3. 1~2번을 하나의 원소만 남을 때까지 반복한다.

### 예제

[8, 5, 6, 2, 4] 배열 오름차순으로 정렬하기

![삽입정렬예시](/DataStructure/images/selection-sort.png)

### 코드

```java
import java.util.*;

public class SelectionSort {

	static int[] arr= {9, 6, 7, 3, 5};
	public static void main(String[] args) {
		// System.out.println("원본 "  + Arrays.toString(arr));
		for(int i = 0; i < arr.length-1; i++) {
			int minIndex = i;
			for(int j = i+1; j < arr.length; j++) { // i번째 이후에서 가장 작은 값을 찾는다.
				if(arr[minIndex] >arr[j]) minIndex = j;
			}
			swap(i, minIndex);  // 해당 위치의 값과 가장 작은 값 swap!
			// System.out.println("i번째 : " + i+ " " + Arrays.toString(arr));
		}
		// System.out.println("결과 " + Arrays.toString(arr));
	}

	private static void swap(int i, int j) {
		int temp = arr[i];
		arr[i] = arr[j];
		arr[j] = temp;
	}
}

/*
원본 [9, 6, 7, 3, 5]
i번째 : 0 [3, 6, 7, 9, 5]
i번째 : 1 [3, 5, 7, 9, 6]
i번째 : 2 [3, 5, 6, 9, 7]
i번째 : 3 [3, 5, 6, 7, 9]
결과 [3, 5, 6, 7, 9]
*/
```

## 특징

- 장점
  - 자료 이동 횟수가 미리 결정된다.
- 단점
  - 안정성을 만족하지 않는다. 즉 값이 같은 레코드가 있는 경우에 상대적인 위치가 변경될 수 있다.

## 시간복잡도

- Best : O(n^2)
- Avg : O(n^2)
- Worst O(n^2)
