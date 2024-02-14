# PCB & Context Switching


### Process Management

> process management란 CPU가 여러개의 프로세스들을 CPU 스케줄링을 통해 관리하는 것을 말한다. 이때, CPU는 각 프로세스들이 누군지 알아야 한다. 프로세스들의 특징을 갖고있는 것이 바로 `Process Metadata` 이다!

- Process Metadata 
  - Process ID : PID, 프로세스 고유 식별 번호
  - Process State : 프로세스의 현재 상태(준비, 실행, 대기 상태)를 기억
  - Process Priority : 프로세스 우선순위 등과 같은 스케줄링 관련 정보를 기억
  - CPU Registers : 프로세스의 레지스터 상태를 저장하는 공간
  - Owner : CPU 사용시간의 정보(Quantum), 각종 스케줄러에 필요한 정보를 기억
  - CPU Usage
  - Memeory Usage

이 메타데이터는 프로세스가 생성되면 `PCB(Process Control Block)`이라는 곳에 저장된다.

<br>

## PCB(Process Control Block)

> 프로세스 메타데이터들을 저장해 놓는 곳, 한 PCB 안에는 한 프로세스의 정보가 담김

> 프로그램 실행 → 프로세스 생성 → 프로세스 주소 공간에 (코드, 데이터, 스택) 생성 
→ 프로세스 메타데이터들이 PCB에 저장

<img src="https://t1.daumcdn.net/cfile/tistory/25673A5058F211C224" width="400">


<br>

> CPU에서는 프로세스의 상태에 따라 교체작업이 이루어진다. (interrupt가 발생해서 할당받은 프로세스가 waiting 상태가 되고 다른 프로세스를 running으로 바꿔 올릴 때)
>
> 이때, **앞으로 다시 수행할 대기 중인 프로세스에 관한 저장 값을 PCB에 저장해두는 것**이다.

#### PCB 관리

> Linked List 방식으로 관리함
>
> PCB List Head에 PCB들이 생성될 때마다 붙게 된다. 주소값으로 연결이 이루어져 있는 연결리스트이기 때문에 삽입 삭제가 용이함.
>
> 즉, 프로세스가 생성되면 해당 PCB가 생성되고 프로세스 완료시 제거됨

<br>

<br>


## Context Switching

> 만약 컴퓨터가 하나의 Task만 수행할 수 있다면, 상당히 느리고 불편 -> 다양한 사람들이 동시에 사용하는 것처럼 하기 위해서 Context Switching!

- 수행 중인 프로세스를 변경할 때, CPU의 레지스터 정보가 변경되는 것을 `Context Switching`이라고 한다. CPU가 이전의 프로세스 상태를 PCB에 보관하고, 또 다른 프로세스의 정보를 PCB에서 읽어서 레지스터에 적재하는 과정.


### Context Switching이 일어나는 경우 
1. interrupt 발생
2. 실행 중인 CPU의 사용 허가시간을 모두 소모
3. 입출력을 위해 대기해야 하는 경우에 Context Switching이 발생

`즉, 프로세스가 Ready → Running, Running → Ready, Running → Waiting처럼 상태 변경 시 발생!` 

<br>

### Context Switching OverHead

> Context Switching은 **cache 초기화, Memory Mapping 초기화, 메모리의 접근을 위한 Kernel 실행** 등의 Cost가 소요된다.

> 즉 잦은 Context switching은 성능 저하를 가지고 온다.

> 하지만 프로세스를 수행하다가 I/O event가 발생하여 BLOCK 상태로 전환시켰을 때, CPU가 그냥 놀게 놔두는 것보다 다른 프로세스를 수행시키는 것이 효율적이다. 즉 이러한 오버헤드가 있음에도 CPU가 놀지 않도록 만들고, 사용자에게 빠르게 일처리를 제공해주기 위한 것이다.


### Context Switching과 시간 할당량

프로세스들은 시간 할당량을 가지며 할당된 시간이 지나면 context Switching이 일어난다.​

> 시간 할당량이 적어지면 : 문맥 교환 수, 인터럽트 횟수, 오버헤드가 증가하지만 여러 개의 프로세스가 동시에 수행되는 느낌을 갖는다.

> 시간 할당량이 커지면 : 문맥 교환 수, 인터럽트 횟수, 오버헤드가 감소하지만 여러 개의 프로세스가 동시에 수행되는 느낌을 갖지 못한다.

​