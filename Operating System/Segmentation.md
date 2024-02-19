# Segmentation
> 가상 주소 공간을 세그먼트 단위로 실제 메모리 주소 공간에 독립적으로 각각 매핑하는 방식

# segmentation의 등장 배경

<img width="300" alt="Untitled" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/2786c3ef-f76d-4765-8140-fa0a0b4169b3">

기존 가상 주소 공간에서는 **Heap과 Stack 사이의 사용하지 않는 공간도 할당되므로 비효율성**이 발생(**Internal Fragmentation)**

- [1] 사용하지 않는 크기가 발생하므로 **메모리 공간이 낭비**된다.
- [2] 위의 그림과 같이 (16KB 이상의) 메모리가 **큰 주소 공간에는 프로세스를 지원할 수 없다.**
- [3] Code 공간에 여러 코드가 들어가서 **중복**이 발생할 수 있다.

<img width="300" alt="Untitled 1" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/eeb27f6d-e93b-49fb-b516-636c6e20c928"> <br/><br/>

<img width="465" alt="Untitled 2" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/b275221e-1016-4d0f-8a31-0bbd81392bd5">

# segment의 종류

- Code
- Heap
- Stack

<img width="294" alt="Untitled 3" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/1672591a-e897-4b61-b695-7b2e48ad7f28">

# 세그멘테이션에서 Virtual memory 주소를 받고, Physical memory 주소를 찾는 방법(주소 변환)

<img width="500" alt="Untitled 4" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/4fe5c1f8-1ecf-4da8-aa86-7ade1879435b">

가상 주소 1000을 참조시 

→ 가상 주소에서는 code 부분

→ segment 레지스터 값으로 Code segment의 base를 확인

→ 32KB이므로 실제 메모리에서는 32KB + 1000 부분을 참조

## size를 넘어서는 부분을 참조시

<img width="500" alt="Untitled 5" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/41f75b2b-736b-4422-9979-27c1547e8e0c">

segmentation fault 발생

→ OS에 트랩하여 해당 프로세스를 종료

## 실제로는 하드웨어가 변환 중 segment 레지스터를 사용하여 처리

ex1)

<img width="500" alt="Untitled 6" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/87fce403-ca1f-4872-9bb7-fa5f0cda3d0c">

→ 가상 메모리 16KB, 14 비트로 표현

→ 상위 2개 비트로 segment를 구분, 하위 12개 비트로 offset을 계산

→ Code segment: 00

ex2)

<img width="500" alt="Untitled 7" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/a88e34f7-9cac-47d6-9b85-eec3f9eb25ab">

Heap → 01, offset: 4 → 실제 메모리 주소: 34KB + 4

```
// 상위 2 비트를 가지고 옵니다.
Segment = (VirtualAddress & SEG_MASK) >> SEG_SHIFT

// 하위 12 비트로 offset을 구합니다.
Offset = VirtualAddress & OFFSET_MASK

// Offset이 limit을 넘는지 확인합니다.
if (Offset >= Limit[Segment])
    RaiseException(PROTECTION_FAULT)
else
    PhysicalAddress = Base[Segment] + Offset
    Register = AccessMemory(PhysicalAddress)
```

stack은 거꾸로 확장 → 주소 변환도 다르게 해야 하는 것 → 추가적인 하드웨어의 지원 필요

Base, Limit 레지스터를 사용할 때와는 다르게 하드웨어는 segment가 어떤 방향으로 확장되는지 알아야 함.(Grow Positive)

<img width="500" alt="Untitled 8" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/fc6a703a-bfa5-4be9-b20e-9f06c2d44054">

## Stack의 주소 변환

<img width="500" alt="Untitled 9" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/eed78b0e-f1bd-4d83-8414-7297e7e15c7a">

→ 가상 메모리 15KB에 접근 → segment: 11, offset: 3KB

→ Stack은 확장되는 방향이 반대방향

→ offset인 3KB에서 segment의 최대 크기인 4KB를 빼줘야 한다. 

→ stack의 Base인 28KB에 offset인 -1KB를 더해주면 됨

→ 28KB + (-1KB) = 27KB가 실제 메모리의 주소가 됨.

## 유의

heap 예제에서 offset이 할당된 size를 넘어서면 segmentation fault를 발생

Stack의 경우엔 해당 검사를 실제 계산에 사용될 offset의 절댓값을 활용해서 계산

→ 위 예제에서는 offset인 -1KB의 절댓값이 Stack의 Size인 2KB보다 작으니 segmentation fault는 발생하지 않음.

# Segmentation에 대한 지원이 증가함에 따라 시스템 설계자는 하드웨어 지원을 조금 더 추가하여 새로운 효율성을 실현

<img width="500" alt="Untitled 10" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/918bd1ed-bd00-409f-9f5a-dd3cb1872760"> <br/> <br/>
→ 동일한 프로그램 Code 동일 → 해당 부분을 공유

이러한 메모리 공유를 지원하려면 하드웨어에서 추가적인 지원이 필요

<img width="500" alt="Untitled 11" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/6128fd72-e32d-4a60-a525-1e52a43f79ee">

Segment의 size가 크다면 segment의 수가 줄어들게 되어 관리가 편해지지만, 다소 단순하게 구성할 수 밖에 없음.

size가 작은 Segment를 사용하면 유연하게 메모리를 구성할 수 있지만, segment가 많기 때문에 관리가 힘들어질 수 있음.

# OS는 segmentation을 통해 주소변환을 할 때 어떤일이 발생할까?

1. Context Switch → segment 레지스터의 값들을 저장하고 복원해주는 방식으로 처리
2. segment의 수가 증가하거나 감소할 대 OS와의 상호작용 문제 → 예를 들어, Heap segment 공간이 부족할 경우, segment 자체가 커져야 할 수 있음. → heap을 늘리기 위해 system call을 사용 → 더 많은 공간을 제공하고 segment Size를 수정한 뒤 라이브러리에 성공 여부를 알림, 물론 실제 메모리의 공간이 부족하다면 OS가 거부
3. 실제 메모리의 여유 공간을 관리하는 것. → 새로운 주소 공간이 생성되면 OS는 해당 segment에 대한 실제 메모리의 공간을 찾아줘야 함. → 프로세스마다 크기가 모두 다르기 때문에, 각 segment의 크기도 모두 다름. → 실제 메모리에 존재하던 segment들이 확장할 수도 있고, 너무 큰 여유공간에 작은 segment가 들어가서 효율적이지 못할 수 있는 문제가 발생 → External fragmentation

<img width="500" alt="Untitled 12" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/64f2497b-447e-4bc8-9c0e-87b8a0207778">


→ memory Compaction, free-list management algorithm을 이용

→ 외부 단편화를 없앨 수는 없음.(최소화할뿐)

<img width="502" alt="Untitled 13" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/6c71ccd5-9ae0-4ca8-9852-c4868f5d002a">


## Segmentation은 여러 문제를 해결하고 보다 효과적인 메모리 가상화를 구축하는데 도움

1. dynamic allocation에서 발생한 heap, stack 사이의 메모리 낭비를 segmentation로 해결할 수 있었음.
2. 주소 변환이 간단하고 동일한 프로그램은 code segment를 공유함으로써 메모리 절약

## Segmentation의 보완하기 위한 주요 방법

1. 페이징
2. 세그멘테이션과 페이징의 결합
3. 가상 메모리(virtual memory)
4. 동적 세그멘테이션
5. 스와핑

# TMI

## swapping

- 메인 메모리 크기가 한정메인 메모리 크기가 한정
- 메모리 크기가 다 찼을 때, 프로세스를 실행시킬 수 있도록 도와주는 스와핑(swapping) 방법과
- 메모리 크기보다 크기가 큰 프로세스를 실행시킬 수 있게 해주는 가상 메모리(virtual memory)
<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/18c4e0e3-0836-45ce-ba73-3789d8997680" width="500"/>
<br/>
<img width="500" alt="Untitled 14" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/75861278-ec12-48fa-83a1-a7e6ea627e27">

- 예시) IO 발생 -> 하나의 프로세스가 block 상태 -> disk(HDD, SSD)로 다시 보냄(cpu에서 쓰지 못하니까)
- memory에 있어도 되는데 새로운 D가 들어오니까 수행이 완료되면 A 다시 돌아옴
- 프로세스가 실행되려면 반드시 메모리에 올라가야 한다. 프로세스가 스와핑(swapping) 한다 라는 것은 현재 메모리에서 잠깐 다른 저장공간(HDD나 SSD)으로 옮겨졌다가, 돌아왔다가 이런 식으로 실행에 따라 교체될 수 있다는 걸 의미.

# 스와핑의 주요 이슈

1. 메모리 압축
2. 메모리 공간 크기
3. 메모리 관리
   - 비트맵 <br/>
     <img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/b5550f3b-622b-448e-88c1-7816f3dccd29" width="500"/>
     - External fragmentation 발생
     - DRAM을 할당하는 기본단위 : allocation unit
     - (b) corresponding bit map (c) same information as a list
     - (b) memory에 저장 disk -> memory 그때마다 bit를 탐색해야됨.
     - 노드의 숫자 계속 변함 -> 삽입, 삭제 빈번, 표시되는 단위를 byte 단위로 바꾸면, 최악의 경우 allocation unit의 숫자만큼 생성, 대신 internal framentation X
     - allocation unit 에 따라 (b) 크기 변화 allocation unit 작다면 bitmap은 커진다.
     <img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/0544c20f-b3e6-4325-8821-42928bde89b8" width="400"/>



- 연결리스트 - 공간을 segment <br/>
    <img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/55434333-632b-4e25-9bb7-e35f3928b9c1" width="500"/>
    <img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/0cef0e51-abef-4769-9c52-2100a76ca301" width="500"/>

참고자료

- [https://github.com/devSquad-study/2023-CS-Study/blob/main/OS/os_segmentation.md](https://github.com/devSquad-study/2023-CS-Study/blob/main/OS/os_segmentation.md)
- [https://icksw.tistory.com/145](https://icksw.tistory.com/145)
- 대학때 본 강의록
- [https://resilient-923.tistory.com/397](https://resilient-923.tistory.com/397)
- [https://velog.io/@qkrdbqls1001/OS-16.-Segmentation](https://velog.io/@qkrdbqls1001/OS-16.-Segmentation)
