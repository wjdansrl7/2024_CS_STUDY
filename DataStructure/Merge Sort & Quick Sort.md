# Merge Sort & Quick Sort

# Merge Sort (병합정렬)

### 특징

1. 분할정복 방식, 재귀적으로 수열을 절반씩 나눈 후 합치며 정렬
2. Stable Sort (우선 순위가 같은 원소들의 순서가 바뀌지 않음)
3. **추가적인 메모리 필요** (temp 배열)

### 순서

1. 주어진 리스트를 2개로 나눈다.
2. 나눈 리스트 2개를 정렬한다.
3. 정렬된 두 리스트를 합친다.

![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/ms1.png)

출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)

![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/ms3.png)

출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)

### 시간복잡도

![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/ms2.png)

출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)

1. 나눌 때 1 + 2 + 4 +… + 2^k = 2N-1 = O(N)
2. 합칠 때 O(N), 리스트 크기 1 → 2^k까지 매번 2배 = K줄, 
    
    O(Nk) = O(NlgN)
    
3. 최종 O(NlgN)

### 구현

```java
public class MergeSort {
	static int n = 10;
	static int[] arr = { 6, -8, 1, 12, 8, 15, 7, -7, 20, 11 };
	static int[] temp = new int[n];

	public static void main(String[] args) {
		mergeSort(0, n);
		for (int i = 0; i < n; i++) {
			System.out.print(arr[i] + " ");
		}
	}

	static void mergeSort(int st, int en) {
		if (st + 1 == en) {
			return;
		}
		int mid = (st + en) / 2;
		mergeSort(st, mid);
		mergeSort(mid, en);
		merge(st, en);
	}

	static void merge(int st, int en) {
		int mid = (st + en) / 2;
		int left = st;
		int right = mid;
		for (int i = st; i < en; i++) {
			if (right == en)
				temp[i] = arr[left++];
			else if (left == mid)
				temp[i] = arr[right++];
			else if (arr[left] <= arr[right])
				temp[i] = arr[left++];
			else
				temp[i] = arr[right++];
		}
		for (int i = st; i < en; i++) {
			arr[i] = temp[i];
		}
	}
}

```

# Quick Sort (퀵 정렬)

### 특징

1. 분할정복 방식, 재귀적으로 구현, Pivot을 제자리로 보내는 작업 반복
    1. pivot 왼쪽에는 작은 원소가, 오른쪽에는 큰 원소가 오도록
2. **In-Place Sort (추가적인 공간을 필요로 하지 않는다.)**
3. cache hit rate가 높아서 속도가 빠르다.
4. **평균 시간복잡도는 O(NlgN), 평균적으로 Merge Sort보다 빠르지만 최악의 경우 O(N^2)라 테스트에는 적합하지 않다.**
5. **Stable Sort X** (구현 방식에 따라 가능)

### 순서

1. 양쪽 끝에 left, right 포인터를 두고 left는 pivot보다 큰 값이 나올 때까지 오른쪽으로 이동, right는 pivot보다 작은 값이 나올 때까지 이동한다.
    
    ![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/qs1.png)
    
    출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)
    
    ![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/qs2.png)
    
    출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)
    
2. 두 포인터가 가리키는 원소의 값을 스왑
    
    ![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/qs3.png)
    
    출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)
    
3. left와 right가 교차하면 pivot값을 제자리로 바꿔준다.
    
    ![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/qs4.png)
    
    출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)
    
    ![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/qs5.png)
    
    출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)
    
4. pivot보다 작은 부분집합, 큰 부분집합 재귀

### 시간복잡도

pivot이 매번 중앙에 위치해서 리스트를 균등하게 쪼갤 경우 → O(NlgN)

![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/qs6.png)

출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)

리스트가 오름차순이거나 내림차순일 때 O(N^2)

![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/qs7.png)

출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)

### 구현

```java
public class QuickSort {
	static int n = 10;
	static int[] arr = { 6, -8, 1, 12, 8, 15, 7, -7, 20, 11 };

	public static void main(String[] args) {
		quickSort(0, n);
		for (int i = 0; i < n; i++) {
			System.out.print(arr[i] + " ");
		}
	}

	static void quickSort(int st, int en) {
		if (en <= st + 1) {
			return;
		}
		int pivot = arr[st];
		int left = st + 1;
		int right = en - 1;
		while (true) {
			while (left <= right && arr[left] <= pivot)
				left++;
			while (left <= right && arr[right] >= pivot)
				right--;
			if (left > right)
				break;
			swap(left, right);
		}
		swap(st, right);
		quickSort(st, right);
		quickSort(right + 1, en);
	}

	static void swap(int x, int y) {
		int temp = arr[x];
		arr[x] = arr[y];
		arr[y] = temp;
	}
}
```

# 비교

![출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)](images/qs8.png)

출처) [https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)

### ref)

[https://blog.encrypted.gg/955](https://blog.encrypted.gg/955)