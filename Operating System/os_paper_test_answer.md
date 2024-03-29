## 인터럽트

1. 인터럽트 서비스 루틴을 실행하고, 본래 수행하던 작업으로 다시 되돌아온다라는 말과 같다.
2. 트랩

## 세그멘테이션

1. 페이징<br>
   세그멘테이션과 페이징의 결합<br>
   가상 메모리(virtual memory)<br>
   동적 세그멘테이션<br>
   스와핑<br>
   <br>
2. code, Heap, Stack

## 시스템 콜

1. 커널 모드
2. 프로세스 관리

## 주소변환

1. 내부 단편화
2. MMU

## CPU 스케줄링 알고리즘

1. FCFS 스케줄링에 대해 설명해주세요.

   - 중요한 키워드: 큐에 먼저 도착한 스레드를 먼저 스케줄
   - 스케줄링 파라미터: 스레드별 큐 도착 시간
   - (선점/비선점): 비선점
   - 기아 발생(O/X): X

2. Round-Robin 스케줄링에 대해 설명해주세요.
   - 중요한 키워드: 스레드들에게 공평한 기회를 주기 위해 큐에 대기중인 스레드들을 타임 슬라이스 주기로 돌아가면서 선택
   - 스케줄링 파라미터: 타임슬라이스
   - (선점/비선점): 선점
   - 기아 발생(O/X): X

## 교착상태

1. 자원을 소유한 스레드들 사이에서 서로가 다른 스레드의 자원을 요청하여 모든 스레드가 무한정 대기하는 현상
2. 상호 배제, 소유하면서 대기, 강제 자원 반환 불가(비선점), 순환성 대기

## 동기화(스핀락, 세마포어, 뮤텍스)

1. 뭔 사진일까요?
   - 스핀락
   - 뮤텍스
2. |                       | 뮤텍스                          | 스핀락                                                   |
   | --------------------- | ------------------------------- | -------------------------------------------------------- |
   | 대기큐 유무           | O                               | X                                                        |
   | 블록가능여부          | 락이 잠겨 있으면 블록(blocking) | 락이 잠겨있어도 블록되지 않고 계속 락 검사(non-blocking) |
   | lock/unlock 연산 비용 | 저비용                          | CPU를 계속 사용하므로 고비용                             |
   | 적합한 CPU            | 단일 CPU 적합                   | 멀티코어 CPU 적합                                        |
   | 주 사용처             | 사용자 응용 프로그램            | 커널 코드, 인터럽트 서비스 루틴                          |

## 프로세스와 스레드

1. 1 3 4

2. 4 1 3 2

## 운영체제란

1. 커널

2. 커널이 자신을 보호하기 위해 만든 응용프로그램과 커널의 인터페이스.

## 메모리계층

1. 레지스터

2. 주기억장치. RAM은 휘발성. ROM은 비휘발성

## TLB

1. 가상 메모리 주소를 물리적 주소로 변환하는 속도를 높이기 위해 사용되는 캐시

2. 원하는 데이터를 얻기 위해 메인메모리에 최소 2번 접근해야하기 때문에 속도를 높이기 위해

## paging : Smaller Table

1. segmentation과 paging을 섞어서 사용하는 방식

2. page table을 page단위로 잘라 page directory로 접근하는 방식

## 파일 시스템

1. 일반 그래프 디렉터리

2. o

## PCB & ContextSwitching

- 앞으로 다시 수행할 대기 중인 프로세스에 관한 저장 값을 PCB에 저장해두는 것
- (2개 정도 정도만 적어도 통과!)
  Process ID : PID, 프로세스 고유 식별 번호
  Process State : 프로세스의 현재 상태(준비, 실행, 대기 상태)를 기억
  Process Priority : 프로세스 우선순위 등과 같은 스케줄링 관련 정보를 기억
  CPU Registers : 프로세스의 레지스터 상태를 저장하는 공간
  Owner : CPU 사용시간의 정보(Quantum), 각종 스케줄러에 필요한 정보를 기억
  CPU Usage
  Memeory Usage

## 가상메모리와 페이징

1.  물리 메모리의 크기를 벗어나는 프로세스면 실행 불가능, 2. 사용하지 않은 코드도 모두 메모리에 올리는 것이 비효율, 3. 멀티 프로세스 상황에서 swap영역과 io가 빈번하게 발생하는 것을 방지하기 위해

주소 공간이 하나의 단위가 아니라 여러개의 페이지로 나누고, 당장 필요한 부분만 물리 메모리에 가져와 사용한다.

invalid 비트를 사용해서 구분한다.

Least-Recently-Used, 최근에 사용하지 않은 페이지를 가장 먼저 내려보내는 알고리즘

## IPC

프로세스들 간에 자원 공유가 필요할 때

pipe, memory 공유, 소켓
