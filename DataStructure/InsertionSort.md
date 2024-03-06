# Insertion Sort

- 앞에서부터 차례대로 이미 정렬된 배열 부분과 비교하여, 자신의 위치를 찾아 삽입함으로써 정렬을 완성하는 알고리즘

- 매 순서마다 해당 원소를 삽입할 수 있는 위치를 찾아 해당 위치에 넣는다.

## 삽입 정렬 알고리즘 구현

1. 두번째 자료부터 시작한다.
2. 이전의 자료들과 비교하여 삽입할 위치를 지정한다.
   2-1. 만일 이전의 자료가 더 크다면 이전의 자료를 뒤로 보낸다.
   2-2. 만일 이전의 자료가 더 작다면 해당 위치에 현재 자료를 삽입한다.
3. 2번 과정을 마지막 자료까지 반복한다.

### 예제

[8, 5, 6, 2, 4] 배열 오름차순으로 정렬하기

![삽입정렬예시](/DataStructure/images/insertion-sort.png)

### 코드

```java
import java.util.*;

public class InsertionSort {

	public static void main(String[] args) {
		int[] arr= {8, 5, 6, 2, 4};
//		System.out.println("원본 "  + Arrays.toString(arr));
		for(int i = 1; i < arr.length; i++) { // 2번째 원소부터 위치찾기 시작
			int target = arr[i]; // 위치 찾을 대상
            // i번째부터 역으로 돌면서 위치를 찾는다. j번재보다 크다면 위치 찾기 완료
            int j = i-1;
			for(; j >=0 && arr[j] > target; j--) {
				arr[j+1] = arr[j]; // j번째 보다 작다면, j번째를 뒤로 한칸 옮긴다
			}
            arr[j+1] = target;
//			System.out.println("target : " + target + " " + Arrays.toString(arr));
		}
		System.out.println("결과 " + Arrays.toString(arr));
	}
}

/*
원본 [8, 5, 6, 2, 4]
target : 5 [5, 8, 6, 2, 4]
target : 6 [5, 6, 8, 2, 4]
target : 2 [2, 5, 6, 8, 4]
target : 4 [2, 4, 5, 6, 8]
결과 [2, 4, 5, 6, 8]
*/
```

## 특징

- 장점
  - 안정한 정렬 방법(동일한 두 원소끼리는 순서가 바뀌지 않는다.)
  - 레코드의 수가 적을 경우 알고리즘 자체가 매우 간단하므로 다른 복잡한 정렬방법보다 유리하다.
  - 대부분의 레코드가 이미 정렬되어 있는 경우에 매우 효율적이다.
- 단점
  - 비교적 많은 레코드들의 이동을 포함한다.
  - 레코드 수가 많고 레코드 크기가 클 경우에 적합하지 않다.

## 시간복잡도

- Best : O(n)
- Avg : O(n^2)
- Worst O(n^2)
