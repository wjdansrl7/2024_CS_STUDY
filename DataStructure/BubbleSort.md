# 버블 정렬(Bubble sort)
> 서로 인접한 두 원소를 검사하여 정렬하는 알고리즘(시간복잡도: O(N^2)) <br/>
> 인접한 2개의 레코드를 비교하여 크기가 순서대로 되어 있지 않으면 서로 교환한다.

# 버블 정렬 알고리즘의 동작 방식

- 1회전을 수행하고 나면 가장 큰 자료가 맨 뒤로 이동
- 2회전에서는 맨 끝에 있는 자료는 정렬에서 제외되고, 2회전을 수행하고 나면 끝에서 두 번째 자료까지는 정렬에서 제외됨.
- 정렬을 1회전 수행할 때마다 정렬에서 제외되는 데이터가 하나씩 늘어난다.

![bubble-sort-001](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/f9b4dd66-74b4-4944-bce1-eae228957350)

# 버블 정렬 알고리즘의 예

배열에 7, 4, 5, 1, 3이 저장되어 있다고 가정하고, 자료를 오름차순으로 정렬

<img width="190" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12 23 07" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/7a2b989c-27d8-48c5-b3c5-d970a4744d78"> <br/>
<img width="500" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12 22 43" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/6edaa177-bd9d-40f2-8477-fee997bb538b">

```java
package a0306;

import java.util.Arrays;

public class BubbleSort {
	
	static void bubbleSort(int[] arr) {
		int temp = 0;
		for (int i = arr.length-1; i > 0; i--) {
			for (int j = 0; j < i; j++) {
				if(arr[j] > arr[j+1]) {
					temp = arr[j];
					arr[j] = arr[j+1];
					arr[j+1] = temp;
				}
			}
		}
	}
	
	public static void main(String[] args) {
		int[] arr = new int[] {7,4,5,1,3};
		
		bubbleSort(arr);
		
		System.out.println(Arrays.toString(arr)); // [1, 3, 4, 5, 7]
	}
}

```

# 버블 정렬(bubble sort) 알고리즘의 특징

- 장점
    - 구현이 매우 간단하다.
- 단점
    - 순서에 맞지 않은 요소를 인접한 요소와 교환한다.
    - 하나의 요소가 가장 왼쪽에서 가장 오른쪽으로 이동하기 위해서는 배열에서 모든 다른 요소들과 교환되어야 한다.
    - 특히 특정 요소가 최종 정렬 위치에 이미 있는 경우라도 교환되는 일이 일어난다.
    - 일반적으로 자료의 교환 작업(SWAP)이 자료의 이동 작업(MOVE)보다 더 복잡하기 때문에 버블 정렬은 단순성에도 불구하고 거의 쓰이지 않는다.
        
        <img width="500" alt="%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-07_%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB_12 26 05" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/2e2da306-b4a2-48d6-b97a-17952f7af6d1">
        
참고자료
- [https://gmlwjd9405.github.io/2018/05/06/algorithm-bubble-sort.html](https://gmlwjd9405.github.io/2018/05/06/algorithm-bubble-sort.html)
- [https://github.com/GimunLee/tech-refrigerator/blob/master/Algorithm/resources/bubble-sort-001.gif](https://github.com/GimunLee/tech-refrigerator/blob/master/Algorithm/resources/bubble-sort-001.gif)
