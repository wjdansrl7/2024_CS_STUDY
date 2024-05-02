# Clustering, replication, sharding

# **Clustering**

- 한 번에 데이터베이스 서버를 여러 개 두어 서버 한 대가 죽었을 때 대비할 수 있는 분산 기법(수평적인 구조)
- 서버의 부하를 나눠서 감당하므로 CPU, Memory 등 관리 차원에서도 효율적으로 운영 할 수 있음
- 동기 방식으로 노드들 간의 데이터를 동기화
- 분산 환경을 구성하여 Single point of failure와 같은 문제를 해결할 수 있는 Fail Over 시스템을 구축하기 위해서 사용

# **single point of failure(단일 장애점,SPOF)**

시스템 구성 요소 중에서, 동작하지 않으면 전체 시스템이 중단되는 요소가 이중화가 되어 있지 않다면 SPOF일 가능성 높음

# **Fail over**

실 운용환경(컴퓨터 서버, 시스템, 네트워크) 등에서 이상이 생겼을 때, 대체 작동 또는 장애 극복(조치)을 위해 예비 운용환경으로 자동전환되는 기능

## 구성방법

1. Active-Active
![Untitled](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/f2efe7ff-9b3a-46dc-86e2-2537921d64b1)

- Cluster를 구성하는 Component를 동시에 가동
- 데이터베이스 서버를 여러 대 한꺼번에 운영하므로 비용이 추가적으로 발생

2. Active-Standby

![Untitled 1](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/6e0a8f7a-db85-49d7-bb9a-678231723c99)

- Cluster를 구성하는 Component 중 실제로 가동하는 것은 Active, 남은 것은 Standby
- 서버가 다운되었을 때 Standby 상태의 서버를 Active 상태로 전환 하는데 일부 지연 시간이 발생

단점

- 두 방식은 Storage를 서로 공유하기 때문에 병목현상이 발생 할 수 있음
- 다중화도 하지 않는 공통적인 단점이 있음

# **Replication**

- 단일 Storage에 대해 DB서버와 저장소를 세트로 구성하는 방식
- 여러 개의 DB를 수직적인 구조, 즉 Master, Slave로 구축하는 방식

![Untitled 2](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/033d4da8-9219-4dc2-976a-415090233fcb)

Master와 Slave를 읽기와 쓰기라는 역할로 각기 배분하여 부하가 많이 발생하는 부분을 분리해 분산 처리 합니다.

데이터 무결성을 위해 동기적으로 처리되는거 같지만 비동기 방식으로 진행되어 지연 속도를 최소화

단점

- 다만 데이터가 많아지면 Slave를 늘려도 시간이 다수 소요되며 데이터 동기화에 의해서 스케일링에 한계가 존재한다

# **Sharding**

- 같은 테이블의 데이터를 특정 기준으로 나눠서 저장 및 검색하는 방법
- 데이터 세트를 단일 데이터베이스만으로 저장하기에 너무 큰 경우 Sharding 방식을 활용
- Shard key라고 알려진 key를 기준으로 데이터가 나뉘며 알고리즘에 따라 아래와 같이 구분
- NoSQL에 최적화가 되어 있는 반면

## Hash Sharding

![Untitled 3](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/675adb60-7240-460d-9552-6bba9496f805)

장점

- Shard의 수 만큼 Hash 함수를 사용해 나온 결과에 따라 DB 서버에 저장하는 방식으로 구현이 매우 간단

단점

- 확장성이 낮아 데이터베이스 서버가 추가 될 경우 Hash 함수가 변경되어야 하므로 기존에 저장되던 데이터의 정합성이 깨지게 됨.

## Dynamic Sharding

![Untitled 4](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/fbccf7e1-6e81-4271-abe7-a1e61322f32a)

- Locator Service를 통해 테이블 형식의 데이터를 바탕으로 Shard를 결정해 적절히 저장하는 방식
- Hash Sharding 방식과 달리 단순히 키만 추가해주면 되므로 확장에 유연한 구조
- Locator Service에 장애가 발생하면 나머지 Shard 또한 문제가 발생

Entity Group

![Untitled 5](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/6eda27c1-4eed-4878-8f2b-f11584758a83)

- Hash Sharding과 Dynamic Sharding의 Key-Value 형태를 지원하기 위해 나온 방법으로 연관성이 있는 Entity를 한 Shard에 두는 방식
- 같은 Shard에 있는 데이터를 조회할 때는 효과적이지만, 다른 Shard에 있는 데이터를 함께 조회할 때는 오히려 성능이 떨어지는 단점
- RDB에 최적화되어 이와 잘 어울리는 방식

# **Hierarchical keys & Column-Oriented Databases**

![Untitled 6](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/49b51c12-9d5f-4948-b454-e1f7be502ce5)

- Column-oriented 데이터베이스는 Column 방식으로 저장하면서 모든 Column들을 개별적인 데이터로 저장하기 때문에 압축에도 높은 효율
- 특정 데이터 집합의 연산이 필요한 경우 추가 메모리 소모 없이도 결과를 도출할 수 있으며, 이에 따라 성능이 크게 향상
- 반대로 다수의 특정 위치의 Column을 조회하는 상황에서는 비효율적

참고자료
- [https://eunsun-zizone-zzang.tistory.com/50](https://eunsun-zizone-zzang.tistory.com/50)
- [https://block-odyssey-tech.medium.com/backend-clustering-replication-and-sharding-e6abbcd129ae](https://block-odyssey-tech.medium.com/backend-clustering-replication-and-sharding-e6abbcd129ae)
