# L4, L7 Load Balancing

**용어 정리**

- Port : TCP나 UDP에서 어플리케이션이 상호구분을 위해서 사용하는 번호

## 스위치(Switch)란?

교차로에 존재하면서 경로 선택(Switching)을 하는 기기

상위 레벨 스위치라면 자신 아래 모든 하위 레벨 스위치를 이해

![출처) [https://aws-hyoh.tistory.com/149](https://aws-hyoh.tistory.com/149)](images/img2-0.png)

출처) [https://aws-hyoh.tistory.com/149](https://aws-hyoh.tistory.com/149)

## 로드 밸런싱(Load Balancing)이란? (=부하 분산)

외부의 사용자에게서 들어오는 다수의 요청을 서버들에게 적절히 배분하여 서버들로 하여금 요청을 처리하게 하는 것을 뜻한다. 분산 처리는 부하 분산 Network Switch 혹은 소프트웨어가 담당한다. 즉 외부로부터 요청을 서버가 직접 받는 것이 아닌 ‘로드 밸런싱 스위치’ 혹은 ‘소프트웨어’가 받은 후 이를 서버로 적절히 나누어 주는 것이다. 서버 부하 분산을 담당하는 Network Switch를 L4/L7라고 부른다.

## 서버 로드 밸런싱 방법

### Round Robin

로드 밸런서가 다수의 서버에게 순서대로 요청을 할당

가장 단순한 방법으로 서버군에 차례로 요청을 할당하여 분산한다.

![출처) [https://aws-hyoh.tistory.com/62](https://aws-hyoh.tistory.com/62)](images/img2-1.png)

출처) [https://aws-hyoh.tistory.com/62](https://aws-hyoh.tistory.com/62)

### Least Connection

로드밸런서가 서버에게 요청을 전달한 뒤, 사용자와 서버가 정상적인 연결을 맺으면 사용자와 서버는 Connection을 생성한다. 로드밸런서 또한 중간자로서 Connection정보를 갖고 있는데 이 정보를 기반으로 가장 Connection이 적은 서버, 즉 부하가 가장 덜한 서버에게 요청을 전달한다.

![출처) [https://aws-hyoh.tistory.com/62](https://aws-hyoh.tistory.com/62)](images/img2-2.png)

출처) [https://aws-hyoh.tistory.com/62](https://aws-hyoh.tistory.com/62)

### Ratio(가중치)

서버의 처리 능력을 고려하여 각 서버가 가질 수 있는 Connection 비율을 미리 정해둔다. 성능이 좋은 서버 우선 할당

![출처) [https://aws-hyoh.tistory.com/62](https://aws-hyoh.tistory.com/62)](images/img2-3.png)

출처) [https://aws-hyoh.tistory.com/62](https://aws-hyoh.tistory.com/62)

### Fastest(Response Time)

응답속도가 가장 빠른 서버에게 우선적으로 할당하는 방식이다.

## L4 스위치 (L4 로드밸런서)

- 로드밸런싱(서버 부하 분산)을 처리하는 장비
- Layer 3의 IP + Layer 4의 TCP/UDP Port 정보로 스위칭
- 단순히 서버의 부하 분산을 처리하는 장비가 아니라 Layer 4 Protocol, Layer 7 Protocol 등의 헤더를 이용한 부하 분산 처리가 가능하기 때문에 기능이 매우 많다.
- 외부에서 들어오는 모든 요청은 서버가 아닌 L4 스위치를 거쳐야 하며 모든 요청을 L4 스위치가 받아 서버들에게 나누어준다.
- TCP, UDP, HTTP와 같은 프로토콜의 헤더를 분석하여 트래픽을 관리하고 로드 밸런싱을 수행할 수 있다. 이때 Source IP나 Destination IP를 NAT하여 통신 경로를 변경하는 것이 가능한데, 이를 통해 클라이언트와 서버 간의 통신이 로드 밸런서를 경유하게 하여 부하 분산이나 다양한 네트워크 관리 목적을 달성할 수 있다.

![출처) [https://aws-hyoh.tistory.com/65](https://aws-hyoh.tistory.com/65)](images/img2-4.png)

출처) [https://aws-hyoh.tistory.com/65](https://aws-hyoh.tistory.com/65)

- 클라이언트와 서버가 3 way handshake를 거쳐 Connection을 생성하면 중간자 역할인 L4 스위치도 Connection을 생성하여 리스트를 관리한다. (3 way handshake도 L4 스위치를 통해 실시)
- L4 스위치가 필요한 이유
    - 서버가 여럿일 때 부하를 고르게 분산 시키기 어려운 문제 → L4 스위치에 모든 요청을 전달하고 L4 스위치가 로드밸런싱
    
    ![출처) [https://aws-hyoh.tistory.com/65](https://aws-hyoh.tistory.com/65)](images/img2-5.png)
    
    출처) [https://aws-hyoh.tistory.com/65](https://aws-hyoh.tistory.com/65)
    
    - L4 스위치로 인해 외부에서 서버의 IP를 알 수 없으니 서버에 직접 접속할 방법이 사라진다 → 외부에서 서버에 악의적인 트래픽 공격을 하고자 하여도 서버의 IP를 모르기 때문에 직접적인 공격이 불가능
- L4 스위치의 구성요소
    
    ![출처) [https://aws-hyoh.tistory.com/65](https://aws-hyoh.tistory.com/65)](images/img2-6.png)
    
    출처) [https://aws-hyoh.tistory.com/65](https://aws-hyoh.tistory.com/65)
    
    - Layer 4의 Port(80)를 사용해서 L4 스위치이다.
    - 로드밸런싱을 위해 L4 스위치에는 외부에서 접속할 IP와 Port를 명시해주어야한다.

## L7 스위치 (L7 로드밸런서)

- IP주소 + 포트번호 + L5 ~ L7 패킷 payload까지 보고 스위칭
    
    (ex: HTTP/HTTPS헤더, payload정보, URL type, 쿠키 등)
    
- 패킷에 적힌 URL 정보, 쿠키등을 판단해 더 정교한 로드밸런싱 수행

## L4 Load Balancing vs L7 Load Balancing

- L4와 L7을 나누는 이유: 트래픽 처리에 리소스를 소모한다. 단순 서버 부하 분산에만 목적을 둔다면 L7의 HTTP 헤더 해석은 불필요, 필요에 따라 사용
- L4 Load Balancing
    - Layer 4(TCP, UDP)에서 IP와 Port를 활용하여 서버 부하 분산 의미
    - 기준이 IP와 Port이기 때문에 TCP/UDP의 헤더를 분석하여 로드밸런싱에 활용하지는 않는다.
- L7 Load Balancing
    - Layer 4를 넘어 Layer 7 프로토콜의 헤더를 분석하여 로드밸런싱을 실시하는 것을 의미
    - Layer 7 프로토콜을 통해 사용자 정의 로드밸런싱을 실시하거나 Layer 7 프로토콜 헤더를 조작 / 활용할 수 있다.
- Layer 7 프로토콜은 Layer 4 프로토콜과 별개로 움직이는 것은 아니다.

## 로드밸런서 기본 기능

- 상태 확인: 서버에 대한 주기적인 상태확인 으로 서버들의 장애 여부를 판
- Tunneling: 통로를 만들어 통신할 수 있게 하는 개념, 데이터를 캡슐화해서 연결된 상호 간에만 캡슐화된 패킷을 구별해 캡슐화 해제 가능
- NAT: IP주소를 변환해 주는 기능, 사설IP - 공인IP
- DSR(Direct Server Routing): 서버에서 클라이언트로 되돌아 갈 때, 목적지를 클라이언트로 설정한 다음 네트워크 장비나 로드밸런서를 거치지 않고 바로 클라이언트를 찾아가는 방식, 로드밸런서 부하를 줄여준다.

## 로드밸런서 장애 대비

서버를 분배하는 로드밸런서에 문제가 생길 수 있기 때문에 이중화하여 대비

Active / Passive 상태

![출처) [https://steady-coding.tistory.com/535](https://steady-coding.tistory.com/535)](images/img2-7.png)

출처) [https://steady-coding.tistory.com/535](https://steady-coding.tistory.com/535)

### ref)

[https://aws-hyoh.tistory.com/62](https://aws-hyoh.tistory.com/62)

[https://etloveguitar.tistory.com/135](https://etloveguitar.tistory.com/135)

[https://aws-hyoh.tistory.com/entry/L4-Switch-쉽게-이해하기](https://aws-hyoh.tistory.com/entry/L4-Switch-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0)

[https://jiwondev.tistory.com/189](https://jiwondev.tistory.com/189)

[https://etloveguitar.tistory.com/136](https://etloveguitar.tistory.com/136)

[https://steady-coding.tistory.com/535](https://steady-coding.tistory.com/535)