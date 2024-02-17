# 명령어 사이클
> 프로그램 속 각각의 명령어들은 일정한 주기가 반복되며 실행되는 데 이러한 주기를 명령어 사이클이라고 한다. <br/>
> CPU가 하나의 명령어를 처리하는 과정에는 어떤 정해진 흐름이 있고, CPU는 그 흐름을 반복하여 명령어들을 처리해 나간다. 명령어를 처리하는 정형화된 흐름을 명령어 사이클이라고 한다.

<img width="380" alt="Untitled 1" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/884bd33d-1563-4a68-8799-70c8cce93d65">

- 인출 사이클: 메모리에 있는 명령어를 CPU로 가져오는 단계
  
<img width="380" alt="Untitled 2" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/0a0c5356-1ab5-43b6-92ae-f8dddfad34d9">

- 실행 사이클: CPU로 가져온 명령어를 실행하는 단계(제어 장치가 명령어 레지스터에 담긴 값을 해석하고, 제어 신호를 발생시키는 단계)
<img width="320" alt="Untitled 3" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/d6a6260f-093e-4717-86b6-a5635b29d88f">

- 프로그램을 이루는 수많은 명령어는 일반적으로 인출과 실행 사이클을 반복하며 실행
- 그러나, 명령어를 인출하여 CPU로 가져왔다 하더라도 곧바로 실행할 수 없는 경우도 있음(간접 주소 지정 방식)
- 간접 주소 지정 방식은 오퍼랜드 필드에 유효 주소의 주소를 명시 → 명령어를 실행하기 위해서는 메모리 접근을 한 번 더 해야한다. (간접 사이클)

# 인터럽트(Interrupt)
> CPU는 정해진 흐름에 따라 명령어를 처리해 나가지만, 간혹 이 흐름이 끊어지는 상황이 발생, 이를 인터럽트라고 한다.
> CPU의 작업을 방해하는 신호
> ex) 회사에서 일을 하는 도중, 상사가 “이게 더 급한거니까, 하던 일 멈추고 이것부터 해줘”라는 요청

## 인터럽트의 종류(종류를 구분하는 통일된 기준X, 인텔의 공식 문서 기준)

<img width="363" alt="Untitled 4" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/66edb199-4fba-4bb9-b6b2-fcb9bfac22a6">

### 동기 인터럽트(예외)
- CPU에 의해 발생하는 인터럽트
- CPU가 명령어들을 수행하다가 예상치 못한 상황에 마주쳤을 때, 가령 CPU가 실행하는 프로그래밍상의 오류와 같은 예외적인 상황에 마주쳤을 때 발생하는 인터럽트

### 비동기 인터럽트(하드웨어 인터럽트) - 주로 입출력장치에 의해 발생하는 인터럽트(알림 역할)
- ex) CPU가 프린터와 같은 입출력장치에 입출력 작업을 부탁하면 작업을 끝낸 입출력장치가 CPU에 완료 알림(인터럽트)을 보냄.
- 주로 인터럽트라고 칭하는 것.
- 알림과 같은 인터럽트, 하드웨어 인터럽트를 이용하면 CPU는 주기적으로 입출력 작업의 결과를 매번 확인할 필요 없이 완료 인터럽트를 받을 때까지 다른 작업을 처리할 수 있음.
- 따라서 하드웨어 인터럽트는 입출력 작업중에도 CPU로 하여금 효율적으로 명령어를 처리할 수 있음

<img width="513" alt="Untitled 5" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/b9911e93-c68a-4da8-ab11-50393771a4c6">

# 하드웨어 인터럽트 처리 순서

1. 입출력장치는 CPU에 인터럽트 요청 신호를 보낸다.
   - 인터럽트 요청 신호: CPU의 정상적인 실행 흐름을 끊는 것이므로 CPU에게 물어보는 것.
  
<img width="330" alt="Untitled 6" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/ceecc386-0044-4538-a05a-d474dea523bf">



2. CPU는 실행 사이클이 끝나고, 명령어를 인출하기 전 항상 인터럽트 여부를 확인한다.
3. CPU는 인터럽트 요청을 확인하고 인터럽트 플래그를 통해 현재 인터럽트를 받아들일 수 있는지 여부를 확인한다.
   - 인터럽트 플래그: CPU가 인터럽트 요청을 수용하기 위해서는 활성화 되어있어야 함.(하드웨어 인터럽트를 받아들일지 무시할지를 결정하는 플래그, 불가능이라면 인터럽트 요청이 오더라도 해당 요청을 무시)
   - 그러나, 모든 하드웨어 인터럽트를 인터럽트 플래그로 막을 수 있는 것은 아님. 무시할 수 없는 하드웨어 인터럽트는 가장 먼저 처리해야 하는 인터럽트들이 해당(정전이나 하드웨어 고장으로 인한 인터럽트가 이에 해당)

<img width="410" alt="Untitled 7" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/e0dac687-3e8c-4785-a03d-15c5f901fbc4">
    
4. 인터럽트를 받아들일 수 있다면 CPU는 지금까지의 작업을 백업한다.
   - CPU는 인터럽트 서비스 루틴을 실행하기 전에 프로그램 카운터 값 등 현재 프로그램을 재개하기 위해 필요한 모든 내용을 스택에 백업, 프로그램 카운터 값을 인터럽트 서비스 루틴의 시작 주소가 위치한 곳으로 갱신하고 인터럽트 서비스 루틴을 실행

<img width="521" alt="Untitled 8" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/59fd16e6-2b11-40ab-840c-4460e9764af7">
    
5. CPU는 인터럽트 벡터를 참조하여 인터럽트 서비스 루틴을 실행한다.
   - 인터럽트 서비스 루틴: 인터럽트를 처리하기 위한 프로그램, 다른 말로 인터럽트 핸들러  → 어떤 인터럽트가 발생했을 때 해당 인터럽트를 어떻게 처리하고 작동해야 할지에 대한 정보로 이루어진 프로그램
   - 메모리에 여러 개의 인터럽트 서비스 루틴이 저장되어 있고, 수많은 인터럽트 서비스 루틴을 구분하기 위해 인터럽트 벡터를 이용한다.
   - 인터럽트 벡터: 인터럽트 서비스 루틴을 식별하기 위한 정보(인터럽트 서비스 루틴의 시작 주소)
   - CPU는 하드웨어 인터럽트 요청을 보낸 대상으로부터 데이터 버스를 통해 인터럽트 벡터를 전달받는다.
    
<img width="512" alt="Untitled 9" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/4e3018a0-3ded-486b-bea8-e93b2b19c58e">

6. 인터럽트 서비스 루틴 실행이 끝나면 4번에서 백업해 둔 작업을 복구하여 실행을 재개한다.

## “CPU가 인터럽트를 처리한다.” ??

→ 인터럽트 서비스 루틴을 실행하고, 본래 수행하던 작업으로 다시 되돌아온다라는 말과 같다.

→ CPU가 인터럽트 서비스 루틴을 실행하려면 인터럽트 서비스 루틴의 시작 주소를 알아야 하는데, 이는 인터럽트 벡터를 통해 알 수 있다.

→ 인터럽트 서비스 루틴도 명령어와 데이터로 이루어져있기 때문에 프로그램 카운터를 비롯한 레지스터들을 사용하며 실행된다.

<img width="511" alt="Untitled 10" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/026bdbf7-6007-4bf3-b734-38f519a6fade">
<img width="511" alt="Untitled 11" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/2f11fb7e-7a35-4088-a91d-ed61c41293e8">

PCB(Process Control Block)

- PCB는 프로그램마다 하나씩 존재하는데 해당 프로그램의 어느 부분이 실행 중이였는지를 저장한다.
- 저장되는 내용은 실행중이던 코드의 메모리 주소, 레지스터 값, 하드웨어 상태 등이 저장된다.
    
    
    <img width="395" alt="Untitled 12" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/0e9251ae-ffdd-4db6-a558-1d627d7dc3cc">


# 명령어 사이클 전체 그림

<img width="520" alt="Untitled 13" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/ba5a23f4-3c53-4dc7-b58a-1527345bc336">

# 예외의 종류

- 폴트 - 예외를 처리한 직후 예외가 발생한 명령어부터 실행을 재개하는 예외
    - ex) CPU가 한 명령어를 실행하는 데, 명령어를 실행하기 위해 꼭 필요한 데이터가 메모리가 아닌 보조기억장치에 있을 경우, CPU는 폴트를 발생시키고, 보조기억장치로부터 필요한 데이터를 메모리로 가져와서 저장, 그러 이후 다시 실행을 재개할 때 폴트가 발생한 그 명령어부터 실행
- 트랩 - 예외를 처리한 직후 예외가 발생한 명령어의 다음 명령어부터 실행을 재개하는 예외
    - 주로 디버깅할 때, 특정 코드가 실행되는 순간 프로그램의 실행을 멈추게 할 수 있음. 이후, 트랩을 처리해서(트랩을 처리하고 나면, 다시 말해 프로그램을 중단시키고 디버깅이 끝나면) 프로그램은 다음 명령어부터 실행을 이어나가게 되는 것.
- 중단 - CPU가 실행 중인 프로그램을 강제로 중단시킬 수 밖에 없는 심각한 오류를 발견했을 때 발생하는 예외
- 소프트웨어 인터럽트 - 시스템 호출이 발생했을 때 나타남

<br/>
- 참고자료 <br/>
https://zangzangs.tistory.com/106](https://zangzangs.tistory.com/106 <br/>
혼자 공부하는 컴퓨터구조 + 운영체제 책 참고