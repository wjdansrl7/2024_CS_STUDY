# Hash
- **저장 또는 검색 등에서 자주 활용되는 자료구조**
- 특정한 함수(알고리즘)를 통해서 값을 추출하고 활용
- **입력 데이터를 고정된 길이의 데이터로 변환된 값**
- 정수로 변환된 해시는 배열의 인덱스, 위치, 데이터 값을 저장하거나 검색할 때 활용

# Hash의 특징
- 키(KEY)에 데이터(Value)를 매핑할 수 있는 데이터 구조
- 해시 함수를 통해 키의 데이터를 배열에 저장할 수 있는 주소(인덱스 번호)를 계산
- 키를 통해서 저장된 데이터를 빠르게 찾고, 저장 및 탐색 속도가 획기적으로 빨라짐

## 해시 테이블(Hash Table)
- 키와 값을 함께 저장해 둔 데이터 구조

<img width="635" alt="Untitled" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/a6c4f71c-608c-481b-80b0-b1002e04dd01">

# Hash 충돌 처리 방법

1. Chaining 기법
    Linked list를 이용해서 충돌을 해결하는 방법입니다. (주로 사용할 방법)
    자료 저장시, 저장소(bucket)에서 충돌이 일어나면 해당 값을 기존 값과 연결시키는 기법입니다.
    <img width="614" alt="Untitled 1" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/64bb2777-450f-435d-b616-f391a62a6d69">

2. Open Address 기법
    
    충돌이 발생할 경우, 해시 테이블의 다른 자리에 대신 저장하는 방법입니다.
    
    따라서 Open addressing 방식에서는 하나의 해시에 하나의 값이 매칭됩니다.
    
    **장점 :**
    
    1. 한정된 저장소(Bucket)를 효율적으로 사용할 수 있습니다.
    2. 상대적으로 적은 메모리를 사용합니다
    
    단점 :
    
    1. 한 Hash에 자료들이 계속 연결될 수 있으며, 이때 검색 효율이 떨어집니다.
    
    ex)
    
    - 선형 탐색(Linear Probing): 다음 해시(+1)나 n개(+n)를 건너뛰어 비어있는 해시에 데이터를 저장
    - 제곱 탐색(Quadratic Probing): 충돌이 일어난 해시의 제곱을 한 해시에 데이터를 저장
    - 이중 해시(Double Hashing): 다른 해시함수를 한 번 더 적용한 해시에 데이터를 저장

    <img width="445" alt="Untitled 2" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/54e951bf-464d-4217-9738-25d1427de1cf">
    
# djb2 → 문자열의 hash 함수
1.  magic number 5381과 33을 활용

```cpp
// 원형
unsigned long djb2(unsigned char* str) {
            unsigned long hash = 5381;
            int c;

            while (c = *str++) {
              hash = ((hash << 5) + hash) + c; /* hash * 33 + c */
            }
            return hash;
}
```

```java
static long djb2(String str) {
        long hash = 5381;

        int c;

        for(int i = 0; i < str.length(); i++) {
            hash = ((hash<<5) + hash) + str.charAt(i);
        }  /* hash * 33 + c */
        return hash;
    }
public static void main(String[] args) {
        long a = djb2("HelloWorld");
        System.out.println("a = " + a);
    }

// a = 8244914940577085761
```

ex) 길이가 10 이하인 문자열을 key로, int를 value로 가지는 해시 테이블 구현

1. Chaining을 linked list로 구현했습니다.

2. Single linked list이기 때문에 작업을 위해 이전 노드를 사용하고 있습니다.

3. 노드에 원본 문자열을 복사하는 이유는 충돌 때문입니다. 해시값이 같다면 원본 문자열을 비교합니다.

## **---해당 소스코드는 발표로 대체---**

1. 적절한 hash table size
    - 소수나 2의 거듭제곱(2m)을 사용
    - 테이블의 크기로 소수를 사용 할 경우, 충돌이 적지만 느린 % 연산자를 사용
    - 편향된 해시 값이 많은 경우에 효율적인 테이블의 크기는 자신을 제외한 모든 수와 서로소인 소수
    - 10007, 20011, 30011, 40009, 100003, 200003이 흔히 쓰이는 소수
