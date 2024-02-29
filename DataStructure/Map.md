# Map
> 리스트나 배열처럼 순차적으로(sequential) 해당 요소 값을 구하지 않고 key를 통해 value를 얻는다.

- 맵(Map)의 가장 큰 특징이라면 key로 value를 얻어낸다는 점이다.

Map 컬렉션 클래스

- Collection 인터페이스와는 다른 저장 방식
- 키와 값을 하나의 쌍으로 저장하는 방식(key-value 방식)

# Map의 특징

1. 요소의 저장 순서를 유지하지 않습니다.

2. key :  중복을 허용  X, value :  중복은 허용 O

# Map의 기본 메서드

| get | 해당 key에 해당되는 값을 반환 |
| --- | --- |
| containsKey | 맵(Map)에 해당 키(key)가 있는지를 조사하여 그 결과값을 리턴 |
| remove | 맵(Map)의 항목을 삭제하는 메소드로 key값에 해당되는 아이템(key, value)을 삭제한 후 그 value 값을 리턴 |
| size | Map의 갯수를 리턴 |

Ex)

```java
import java.util.HashMap;

public class TestMap {
    public static void main(String[] args) {
        HashMap<String, String> map = new HashMap<String, String>();
        map.put("people", "사람");
        map.put("baseball", "야구");

        System.out.println(map.get("people")); // 사람
        System.out.println(map.containsKey("people")); // true
        System.out.println(map.remove("people")); // 사람
        System.out.println(map.size()); // 1
    }
}
```

# Map 컬렉션 클래스에 속하는 클래스

## **1. HashMap<K, V> 클래스**

- Map 컬렉션 클래스에서 가장 많이 사용되는 클래스 중 하나입니다.
- HashMap은 Map을 구현한다. key와 value를 묶어 하나의 entry로 저장한다는 특징을 갖는다.
- 해시 알고리즘(hash algorithm)을 사용하여 많은 양의 데이터를 검색하는데 검색 속도가 매우 빠르다.
- HashMap 클래스는 Map 인터페이스를 구현하므로, 중복된 키로는 값을 저장할 수 없다.
- value에 null값도 사용 가능하다.
- 멀티쓰레드에서는 HashTable을 사용한다.
- 같은 값을 다른 키로 저장하는 것은 가능

![Untitled](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/3bfbb2ea-74f8-473b-a03b-77941f96ecdb)

## **2. Hashtable<K, V>**

- HashMap 클래스와 같은 동작을 하는 클래스.
- Hashtable 클래스는 HashMap 클래스와 마찬가지로 Map 인터페이스를 상속받음.
- 기존 코드와의 호환성을 위해서만 남아있으므로, Hashtable 클래스보다는 HashMap 클래스를 사용하는 것이 좋다.

## 3. LinkedHashMap<K, V>

```java
시간복잡도
get           : O(1)
containsKey   : O(1)
next          : O(1)
java 1.4 에서 나옴
특징 : 순서대로 등록한다. Null을 허용한다. thread-safe 보장하지 않는다.
```

- 입력된 순서대로 Key가 보장


<img width="500" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/c14f838a-9318-424d-aa2b-80c81922ce0c" />
<img width="500" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/7296dcc8-1f77-4d76-b9a6-a79eab313e7a" />

```java
package a0229;

import java.util.HashMap;
import java.util.Iterator;
import java.util.LinkedHashMap;
import java.util.Set;

public class Test {

	public static void main(String[] args) {
		  HashMap<String, String> map = new LinkedHashMap<String, String>();
		  map.put("people", "사람");
		  map.put("baseball", "야구");
		  map.put("zed", "야구");
		  map.put("irelia", "야구");
		  map.put("llli", "야구");

      System.out.println(map.get("people")); // 사람
      System.out.println(map.containsKey("people")); // true
      System.out.println(map.remove("people")); // 사람
      System.out.println(map.size()); // 4
      
      /*
       * baseball
			 * zed
			 * irelia
			 * llli
       */
      for(String k : map.keySet()) {
      	System.out.println(k);
      }
	}
}

```

## 4. TreeMap<K, V>

```
시간복잡도
get           : O(log n)
containsKey   : O(log n)
next          : O(log n)
java 1.2 에서 나옴
특징 : 정렬이 되면서 추가가 됨
     -  null은 허용하지 않음
     -  thread-safe 보장하지 않는다.
```

- 키와 값을 한 쌍으로 하는 데이터를 이진 검색 트리(binary search tree)의 형태로 저장합니다.
- TreeMap에 객체를 저장하면 자동으로 정렬되는데, 키는 저장과 동시에 자동 오름차순으로 정렬되고 숫자 타입일 경우에는 값으로, 문자열 타입일 경우에는 유니코드로 정렬합니다.
- 이진 검색 트리는 데이터를 추가하거나 제거하는 등의 기본 동작 시간이 매우 빠릅니다.
- TreeMap 클래스는 NavigableMap 인터페이스를 기존의 이진 검색 트리의 성능을 향상시킨 레드-블랙 트리(Red-Black tree)로 구현합니다.
- Map 인터페이스를 구현하므로, 중복된 키로는 값을 저장할 수 없습니다.
- Comparator 인터페이스를 구현하게 한다면 원하는대로 순서를 조정할 수 있다.
- 같은 값을 다른 키로 저장하는 것은 가능

| void clear() | 해당 맵(map)의 모든 매핑(mapping)을 제거함. |
| --- | --- |
| boolean containsKey(Object key) | 해당 맵이 전달된 키를 포함하고 있는지를 확인함. |
| boolean containsValue(Object value) | 해당 맵이 전달된 값에 해당하는 하나 이상의 키를 포함하고 있는지를 확인함. |
| K firstKey() | 해당 맵에서 현재 가장 작은(첫 번째) 키를 반환함. |
| V get(Object key) | 해당 맵에서 전달된 키에 대응하는 값을 반환함.만약 해당 맵이 전달된 키를 포함한 매핑을 포함하고 있지 않으면 null을 반환함. |
| K lastKey() | 해당 맵에서 현재 가장 큰(마지막) 키를 반환함. |
| V put(K key, V value) | 해당 맵에 전달된 키에 대응하는 값으로 특정 값을 매핑함. |
| V remove(Object key) | 해당 맵에서 전달된 키에 대응하는 매핑을 제거함. |
| boolean remove(K key, V value) | 해당 맵에서 특정 값에 대응하는 특정 키의 매핑을 제거함. |
| int size() | 해당 맵의 매핑의 총 개수를 반환함. |
| V replace(K key, V value) | 해당 맵에서 전달된 키에 대응하는 값을 특정 값으로 대체함. |
| boolean replace(K key, V oldValue, V newValue) | 해당 맵에서 특정 값에 대응하는 전달된 키의 값을 새로운 값으로 대체함. |


![Untitled 3](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/dd638ea0-8798-48d5-ba96-8b495f70d5ee)

```java
package a0229;

import java.util.Iterator;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

public class TreeMapEx {
    public static void main(String[] args) {
        // TreeMap 생성
        TreeMap<Integer, String> treeMap = new TreeMap<>();

        // TreeMap Entry 객체 저장
        treeMap.put(3, "대구");
        treeMap.put(1, "부산");
        treeMap.put(2, "인천");
        treeMap.put(5, "광주");
        treeMap.put(4, "대전");
        treeMap.put(6, "울산");

        // 저장된 총 Entry 수 얻기
        int size = treeMap.size();
        System.out.println(size); // 6

        // 객체 찾기
        Object object = treeMap.get(1);
        System.out.println(object); // 부산

        // key를 요소로 가지는 Set 생성
        Set<Integer> keySet = treeMap.keySet();
        System.out.println(keySet); // [1, 2, 3, 4, 5, 6]

        // value 값 읽기
        /*
         * 키: 1 값: 부산
         * 키: 2 값: 인천
         * 키: 3 값: 대구
         * 키: 4 값: 대전
         * 키: 5 값: 광주
         * 키: 6 값: 울산
         */
        Iterator<Integer> keyIterator = keySet.iterator();
        while (keyIterator.hasNext()) {
            Integer key = keyIterator.next();
            String value = treeMap.get(key);
            System.out.println("키: " + key + " 값: " + value);
        }

        // 객체 삭제 후 크기 출력
        treeMap.remove(1);
        System.out.println(treeMap.size()); // 5

        // entrySet()을 활용한 value 값 읽기
        /*
         * 키: 2 값: 인천
				 * 키: 3 값: 대구
				 * 키: 4 값: 대전
				 * 키: 5 값: 광주
				 * 키: 6 값: 울산
         */
         for (Map.Entry<Integer, String> entry : treeMap.entrySet()) {
            System.out.println("키: " + entry.getKey() + " 값: " + entry.getValue());
         }
    
	        // Entry 객체를 활용한 값 읽기
	        Map.Entry<Integer, String> entry = null;
	        entry = treeMap.firstEntry();
	        System.out.println("키: " + entry.getKey() + " 값: " + entry.getValue()); // 키: 2 값: 인천
	
	        entry = treeMap.lastEntry();
	        System.out.println("키: " + entry.getKey() + " 값: " + entry.getValue()); // 키: 6 값: 울산
	
	        entry = treeMap.higherEntry(5);
	        System.out.println("키: " + entry.getKey() + " 값: " + entry.getValue()); // 키: 6 값: 울산
	
	        entry = treeMap.lowerEntry(6);
	        System.out.println("키: " + entry.getKey() + " 값: " + entry.getValue()); // 키: 5 값: 광주
	
	        // 전체 객체 삭제
	        treeMap.clear();
		  }
	}
//출처: https://ittrue.tistory.com/154 [IT is True:티스토리]

```

참고자료

[https://devlogofchris.tistory.com/41](https://devlogofchris.tistory.com/41)

[https://www.grepiu.com/post/9](https://www.grepiu.com/post/9)

[https://ittrue.tistory.com/154](https://ittrue.tistory.com/154)

[https://fruitdev.tistory.com/141#recentEntries](https://fruitdev.tistory.com/141#recentEntries)
