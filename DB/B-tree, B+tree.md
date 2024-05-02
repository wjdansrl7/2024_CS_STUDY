# B-tree, B+tree

**균형이진트리** : 리프 노드들의 레벨 차이가 최대 1레벨까지만 나는 트리, 검색할 때 O(logN) 시간 복잡도 유지 (균형이 깨지면 별도 로직을 통해 다시 균형 유지)

ex) AVL 트리, 레드 블랙 트리, B-Tree, B+Tree 등에서 사용

# B-tree

데이터 베이스 인덱스, 파일 시스템, 파일 시스템 캐시 등에 널리 사용되는 트리 자료구조

이진 트리의 확장, 하나의 노드에 여러 개의 값, 최대 자식 2개 이상

내부 노드에 key와 data를 담을 수 있다.

(대량의 데이터는 메모리 보다 블럭 단위로 입 출력하는 하드디스크 or SSD에 저장해야 한다. 한 블럭이 1024 바이트면, 2바이트를 읽으나 1024바이트를 읽으나 똑같은 입출력 비용이 발생한다. 따라서 하나의 노드를 모두 1024바이트로 꽉 채워서 조절할 수 있으면 입출력에 있어서 효율적인 구성을 갖출 수 있다. → 이러한 장점을 토대로 데이터베이스 시스템의 인덱스 저장 방법으로 활용)

데이터가 **정렬된 상태**로 유지되어 있다. **O(logN) 빠르게 탐색 가능**

![[https://techdifferences.com/difference-between-b-tree-and-binary-tree.html](https://techdifferences.com/difference-between-b-tree-and-binary-tree.html)](images/db01-01.jpg)

[https://techdifferences.com/difference-between-b-tree-and-binary-tree.html](https://techdifferences.com/difference-between-b-tree-and-binary-tree.html)

![[https://velog.io/@bambookim/B-Tree-BTree](https://velog.io/@bambookim/B-Tree-BTree)](images/db01-11.png)

[https://velog.io/@bambookim/B-Tree-BTree](https://velog.io/@bambookim/B-Tree-BTree)

하나의 노드 최대 자식 수 M → M차 B-Tree

M차 B-Tree가 가지는 키의 최대 개수 M-1

![[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)](images/db01-02.png)

[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)

### B-Tree 조건

*1. 모든 노드들은 최대 m개의 자식을 가질 수 있다.*

*2. k개의 자식을 가진 리프가 아닌 노드는 k-1개의 키를 가지고 있다.*

*3. 모든 내부 노드는 최소 [m/2]개의 자식을 가져야 한다.*

*4. 모든 리프 노드들은 같은 레벨에 있어야 한다.*

*5. 리프가 아닌 모든 노드들은 최소 2개 이상의 자식을 가져야 한다.*

### M = 3, B-Tree 예시

1. 모든 노드들은 최대 M개의 자식을 가진다.
    
    ![[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)](images/db01-03.png)
    
    [https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)
    
2. K개의 자식을 가진 리프가 아닌 노드는 K-1개의 키를 가진다.
    
    ![[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)](images/db01-04.png)
    
    [https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)
    
3. 모든 내부 노드는 최소 M/2개의 자식을 가져야 한다.
    
    ![[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)](images/db01-05.png)
    
    [https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)
    
4. 모든 리프 노드들은 같은 레벨에 있어야 한다.
    
    ![[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)](images/db01-06.png)
    
    [https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)
    
5. 리프가 아닌 모든 노드들은 최소 2개 이상의 자식을 가져야 한다.
    
    ![[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)](images/db01-07.png)
    
    [https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)
    

![[https://zorba91.tistory.com/293](https://zorba91.tistory.com/293)](images/db01-08.png)

[https://zorba91.tistory.com/293](https://zorba91.tistory.com/293)

# B+tree

B-tree를 개선

- 모든 리프 노드들은 Linked 리스트 형태로 이루어진다.
- 실제 데이터는 리프 노드에만 저장된다. 내부 노드들은 단지 키만 가지고 있고 올바른 리프 노드로 연결해 주는 라우팅 기능을 한다.

B+tree는 중복 키를 가진다. → 내부 노드들이 데이터를 가지고 있지 않기 때문에 리프 노드들이 키와 데이터를 모두 가지고 있어야 하기 때문이다.

![[https://zorba91.tistory.com/293](https://zorba91.tistory.com/293)](images/db01-09.png)

[https://zorba91.tistory.com/293](https://zorba91.tistory.com/293)

B+tree는 실제 데이터를 리프 노드에만 저장하므로 B-tree에 비해 같은 노드에 더 많은 키를 저장할 수 있다. (하나의 노드에 더 많은 key를 담을 수 있기 때문에 트리의 높이가 낮아지고 cache hit를 높일 수 있다.)

데이터를 찾기 위해 리프 노드까지 탐색해야 하는데 Linked 리스트로 연결되어 있기 때문에 full scan 시 리프 노드들만 순차 탐색하면 되기 때문에 탐색에 유리하다. (B-tree는 모든 노드를 탐색해야 한다.)

![[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)](images/db01-02.jpg)

[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)

### 장점

- 블럭 사이즈를 더 많이 이용할 수 있음(key 값에 대한 하드디스크 액세스 주소가 없기 때문)
- 리프 노드 끼리 연결 리스트로 연결되어 있어 범위 탐색에 매우 유리

### 단점

- B-tree의 경우 최상 케이스 에서는 루트에서 끝날 수 있지만, B+tree는 무조건 리프 노드까지 내려가 봐야 한다.

### B-tree & B+tree

- B-tree는 각 노드에 데이터가 저장되고 B+tree는 인덱스 노드와 리프 노드로 분리되어 저장된다. (리프 노드는 서로 연결되어 있어서 임의 접근이나 순차 접근이 모두 성능 우수)
- B-tree는 각 노드에 key와 data 모두 들어갈 수 있고, data는 disk block으로 포인터가 될 수 있다.
    - **GPT:** 데이터베이스의 경우에도 유사한 원리가 적용됩니다. B-tree의 각 노드는 데이터베이스 레코드의 특정 열(키)과 해당 레코드에 대한 메타데이터를 가지며, 레코드의 실제 내용은 디스크 블록에 저장됩니다. 데이터 노드의 데이터 부분은 해당 레코드의 첫 번째 디스크 블록을 가리키는 포인터가 될 수 있습니다. (메모리에 모두 저장 X)
- B+tree는 각 노드에서 key만 들어감, data는 모두 리프 노드에 존재
- B+tree는 add와 delete 모두 리프 노드에서만 이루어진다.

![[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)](images/db01-10.png)

[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)

### GPT한테 물어봄

B+ 트리가 B 트리보다 full scan(전체 스캔) 시 더 빠른 이유는 여러 가지가 있습니다. 주요한 이유는 다음과 같습니다:

1. **더 많은 키를 갖는 리프 노드**: B+ 트리에서는 데이터가 리프 노드에만 저장되기 때문에, 리프 노드가 더 많은 키를 갖을 수 있습니다. 따라서 전체 데이터를 읽어야 할 때 리프 노드를 탐색하는 횟수가 줄어들어 전체 스캔이 더 빨라집니다.
2. **내부 노드가 더 많은 키를 저장**: B+ 트리에서는 내부 노드가 데이터를 저장하지 않고 오직 키만 저장합니다. 이로 인해 내부 노드가 더 많은 키를 저장할 수 있으며, 이는 탐색 경로를 더 줄여주는 데 도움이 됩니다.
3. **순차적 액세스 최적화**: B+ 트리에서는 리프 노드가 연결 리스트로 구성되어 있어서 순차적인 액세스가 가능합니다. 이는 범위 검색(range search) 또는 전체 스캔과 같은 작업에서 성능을 향상시킬 수 있습니다.

따라서 B+ 트리는 데이터베이스에서 범위 검색이나 전체 스캔과 같은 작업에서 특히 유용합니다. 이러한 작업에서는 B+ 트리가 더 나은 성능을 제공할 수 있습니다.

### **B-tree vs B+tree 정리**

1. **검색 방법**
    - 데이터를 검색할 때 ***항상 리프 노드까지 이동***하므로 검색 경로가 단순해진다.
    - **B-Tree:** 모든 노드(internal node + leaf node)에 **`키와 값`이 함께 저장**
    - **B+Tree:** internal 노드에는 **`키만 저장`**, 리프 노드에는 **`키와 값이 저장`**됩니다.
2. **포인터의 사용**
    - 데이터를 찾기 위해 internal 노드를 항상 통과해야 조회한다. ⇒ *추가적인 단계 필요 (단점)*
    - internal 노드를 통하지 않고도 형제 노드로 이동 가능하고, 범위 검색이나 범위 쿼리가 쉽다.
    - **B-Tree: internal 노드의 `포인터`**를 통해서만 **리프 노드로 이동 가능**
    - **B+Tree:** 리프 노드끼리 서로 **`연결 리스트`**로 연결되어 있다.
3. **범위 쿼리와 범위 검색**
    - **B-Tree:** 범위 쿼리를 수행하려면 트리의 **루트에서부터 리프 노드까지** 이동하면서, 루트 노드, internal 노드에 있는 데이터까지 함께 조회해야 한다.
    - **B+Tree:** 데이터는 리프 노드에만 존재하므로, 범위 쿼리는 리프 노드를 시작으로 연결된 리스트를 따라가면 모든 데이터를 조회할 수 있다.
4. **순차 탐색 및 정렬**
    - **B-Tree:** 순차적인 탐색이나 정렬을 위한 추가적인 알고리즘이 필요해서 (예. [inorder traversal](https://engineerinsight.tistory.com/321#%E2%9C%94%EF%B8%8F%C2%A0inorder%20traversal%20(%EC%A4%91%EC%9C%84%20%EC%88%9C%ED%9A%8C)-1)) B+Tree보다 더 복잡하다.
    - **B+Tree:** 연결된 리스트를 따라가면서 순차 탐색이 용이하며, 키들은 항상 정렬된 상태를 유지하면서 저장된다.
5. **메모리 사용**
    - **B-Tree:** 리프 노드와 내부 노드가 각각 데이터와 포인터까지 가지고 있기 때문에 B+Tree보다 더 많은 메모리 공간을 차지한다.
    - **B+Tree:** 데이터는 리프 노드에만 저장되고, internal 노드는 키만 갖고 있으면 되므로, **메모리 효율이 좋다.**

**B+Tree가 DB 인덱스에 사용되었을 때 더 좋은 점**

리프 노드를 제외하고 데이터를 담아두지 않기 때문에 내부 노드의 경우, 한정된 메모리 안에 더 많은 key들을 수용할 수 있다.

즉, 하나의 노드에 더 많은 key들을 저장하기 때문에 동일한 데이터 개수라면, **트리의 높이는 더 낮아진다.**

**cache hit**를 높일 수 있을 뿐만 아니라, **범위 검색이나 범위 쿼리에도 큰 강점**을 갖고 있으며, 상대적으로 굉장히 느린 **secondary storage에 대한 접근 횟수가 현저히 줄어든다.**

테이블 full scan 시, B-Tree의 경우에는 데이터가 internal 노드와 리프 노드에 퍼져있기 때문에 모든 노드를 탐색해야 한다.

B+tree는 리프 노드에만 데이터가 존재하고, 존재하는 **모든 데이터는 리프 노드에 있기 때문에 한 번의 선형 탐색**만 하면 되므로 매우 빠르다.

### Ref

[https://wangin9.tistory.com/entry/B-tree-B-tree](https://wangin9.tistory.com/entry/B-tree-B-tree)

[https://zorba91.tistory.com/293](https://zorba91.tistory.com/293)

[https://escapefromcoding.tistory.com/731](https://escapefromcoding.tistory.com/731)

[https://engineerinsight.tistory.com/336](https://engineerinsight.tistory.com/336)

[https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer Science/Data Structure/B Tree %26 B%2B Tree.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Data%20Structure/B%20Tree%20%26%20B%2B%20Tree.md)