# 1. TCP vs UDP

### 전송 계층(Transport layer) → 데이터의 전달

- 계층 구조의 네트워크 구성 요소와 프로토콜 내에서 송신자와 수신자를 연결하는 통신 서비스를 제공
- **송신자와 수신자를 연결**하는 통신 서비스를 제공하는 계층

### TCP(**T**ransmission **C**ontrol **P**rotocol : 전송 제어 프로토콜)

- **특징**
    - **연결 지향적**, 데이터를 전송하는 동안 수신자와 발신자 사이에 연결을 설정하고 이를 유지한다.
        - 연결 = 3 way handshake / 연결 해제 = 4 way handshake
        - **가상 회선 방식** 패킷 교환 → 데이터 전송 전 송신자와 수신자 간에 가상의 경로나 회선을 설정
    - **신뢰성 보장**, 데이터를 완전히 온전하게 도착하도록 보장
        - ex) 파일 전송, 웹 브라우저, 이메일 전송
        - **신뢰성 보장을 위해 4가지 해결해야 할 문제점**
            1. 패킷의 손실 → 재전송
            2. 패킷의 순서가 바뀌는 현상 → 순서 데이터에 포함
            3. 네트워크 혼잡 → 혼잡 제어
            4. Overload (수신자의 속도 < 송신자의 속도) → 흐름 제어
    - **전송 흐름을 제어**하는 기능 포함 (흐름 제어 & 혼잡 제어)
    - **1 : 1 통신**
        - 전이중(Full-Duplex): 전송이 양방향으로 동시에 일어남
        - 점대점(Point to Point): 각 연결이 정확히 2개의 종단점을 가짐
- **장점**
    - **신뢰성 있는 데이터 전송**
    - 패킷 순서 보장 O
    - TCP는 수신자의 용량에 따라 데이터를 전송하는 속도를 최적화하고 변경함 (흐름 제어 & 혼잡 제어)
    - TCP는 데이터가 목적지에 도달했는지 확인하고 첫 번째 전송이 실패한 경우 재전송을 시도함
- **단점**
    - **UDP보다 느리다** (신뢰성 있는 데이터 전송을 위한 여러 기능 때문)
    - 오버헤드와 지연
    - 전송 중에 소량의 데이터라도 손실 시 나머지 다른 정보를 로드하지 못할 수 있음
    - 신뢰성 보장에 따른 네트워크 부하
- **가상 회선 방식**
    
    ![출처) [https://prinha.tistory.com/entry/Network-패킷교환방식-가상회선방식데이터그램방식](https://prinha.tistory.com/entry/Network-%ED%8C%A8%ED%82%B7%EA%B5%90%ED%99%98%EB%B0%A9%EC%8B%9D-%EA%B0%80%EC%83%81%ED%9A%8C%EC%84%A0%EB%B0%A9%EC%8B%9D%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B7%B8%EB%9E%A8%EB%B0%A9%EC%8B%9D)](1%20TCP%20vs%20UDP%2061911b6520fc4f72b2caf16b48334986/Untitled.png)
    
    출처) [https://prinha.tistory.com/entry/Network-패킷교환방식-가상회선방식데이터그램방식](https://prinha.tistory.com/entry/Network-%ED%8C%A8%ED%82%B7%EA%B5%90%ED%99%98%EB%B0%A9%EC%8B%9D-%EA%B0%80%EC%83%81%ED%9A%8C%EC%84%A0%EB%B0%A9%EC%8B%9D%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B7%B8%EB%9E%A8%EB%B0%A9%EC%8B%9D)
    
    - **연결형 (3 way handshake, 4 way handshake)**
    - **논리적인 경로를 설정**하고 유지하여 데이터를 전송하는 방식
- **작동 원리**
    - 작은 데이터 패킷을 전송 → 수신기에 도착하면 재조립
    1. TCP는 각 데이터 패킷에 고유 식별자와 시퀀스 번호를 할당, 이를 통해 수신자는 어떤 패킷이 수신되었고 다음에 어떤 패킷이 도착하는지 식별가능
        
        → A,B,C라는 패킷에 1,2,3이라는 번호를 부여하여 패킷의 분실 확인과 같은 처리를 하여 목적지에서 재조립
        
    2. 데이터 패킷이 수신되고 올바른 순서로 도착하면 수신자는 발신자에게 수신 확인을 보냄
    3. 수신 확인을 받으면 발신자는 다른 패킷을 보낼 수 있음
    4. 패킷이 분실되거나 잘못된 순서로 전송된 경우, 수신자는 동일한 데이터 패킷을 다시 보내야 함을 알리는 침묵 상태 유지
    
    → 데이터가 순차적으로 전송되기 때문에 혼잡 제어, 흐름 제어에 도움이 되며 오류를 쉽게 발견하고 수정할 수 있음, TCP를 통해 전송된 데이터가 목적지에 온전히 도달할 가능성이 더 높지만 두 당사자 간에 많은 양의 통신이 오가기 때문에 연결을 설정하고 데이터를 교환하는 데 시간이 오래 걸림
    

### UDP(User Datagram Protocol : 사용자 데이터그램 프로토콜)

- **특징**
    - **비연결 지향적**, 두 당사자 간에 사전 연결을 설정하지 않음 → 데이터가 손실될 가능성이 있지만, 훨씬 빠른 속도를 얻을 수 있다.
    - **연속성, 실시간**
        - ex) 스트리밍, 게임, DNS
        - **데이터그램** **방식** 패킷 교환 → 독립적인 관계를 지니는 패킷, 연결을 위해 할당되는 논리적 경로 없음
    - **최소한의 오류만 검출** (UDP 헤더의 Checksum필드를 통해 값 비교)
    - **1 : 1, 1 : N, N : N 통신**
- **장점**
    - **TCP보다 빠르다**
    - 네트워크 부하 적음 (추가적인 제어 기능 없음)
    - UDP는 일부 패킷이 누락되더라도 데이터를 전송하므로 패킷 손실로 인해 전체 전송이 중단되지 않음
    - 브로드캐스트 및 멀티캐스트 기능을 통해 하나의 UDP 전송을 여러 수신자에게 한 번에 전송할 수 있음
- **단점**
    - **신뢰성 보장** **X**, UDP는 전송이 온전하게 도착한다고 보장할 수 없음, 일부 패킷이 손실 됐을 수 있지만 발신자 측에서 이를 확인할 수 있는 방법이 없음
    - 패킷 순서 보장 X
    - 라우터가 데이터 패킷의 우선순위를 정해야 하는 경우, UDP 패킷보다 TCP 패킷을 먼저 전송할 가능성이 높음
- **데이터 그램 방식**
    
    ![출처) [https://prinha.tistory.com/entry/Network-패킷교환방식-가상회선방식데이터그램방식](https://prinha.tistory.com/entry/Network-%ED%8C%A8%ED%82%B7%EA%B5%90%ED%99%98%EB%B0%A9%EC%8B%9D-%EA%B0%80%EC%83%81%ED%9A%8C%EC%84%A0%EB%B0%A9%EC%8B%9D%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B7%B8%EB%9E%A8%EB%B0%A9%EC%8B%9D)](1%20TCP%20vs%20UDP%2061911b6520fc4f72b2caf16b48334986/Untitled%201.png)
    
    출처) [https://prinha.tistory.com/entry/Network-패킷교환방식-가상회선방식데이터그램방식](https://prinha.tistory.com/entry/Network-%ED%8C%A8%ED%82%B7%EA%B5%90%ED%99%98%EB%B0%A9%EC%8B%9D-%EA%B0%80%EC%83%81%ED%9A%8C%EC%84%A0%EB%B0%A9%EC%8B%9D%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B7%B8%EB%9E%A8%EB%B0%A9%EC%8B%9D)
    
    - **비연결형**, 연결을 상태를 유지하지 않고 각각의 패킷 독립적으로 처리
    - 패킷이 손실되거나 순서가 바뀔 수 있음 (신뢰성 X)
- **작동 원리**
    - UDP는 고유 식별자나 시퀀스 번호 없이도 TCP와 동일한 작업을 수행하는 방식으로 작동함
    - 스트림으로 데이터를 전송
    - UDP는 오류 수정 기능이 거의 없으며 패킷 손실에 대해서도 신경 쓰지 않음
    
     → 오류가 발생하기 쉽지만 TCP보다 훨씬 빠르게 데이터를 전송
    

### 요약:

![출처) [https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)](1%20TCP%20vs%20UDP%2061911b6520fc4f72b2caf16b48334986/Untitled%202.png)

출처) [https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)

***TCP는 연속성보다 신뢰성 있는 전송이 중요할 때 사용되는 프로토콜이며, UDP는 TCP보다 빠르고 네트워크 부하가 적다는 장점이 있지만, 신뢰성 있는 데이터 전송을 보장하지는 않음***

*Q) TCP는 패킷을 어떻게 추적 및 관리하나요?*

```
위에서 데이터는 패킷단위로 나누어 같은 목적지(IP계층)으로 전송된다고 설명하였습니다. 예를 들어 한줄로 서야하는 A,B,C라는 사람(패킷)들이 서울(발신지)에서 출발하여 부산(수신지)으로 간다고 합시다. 그런데 A,B,C가 순차적으로 가는 상황에서 B가 길을 잘못 들어서 분실되었다고 합시다. 하지만 목적지에서는 A,B,C가 모두 필요한지 모르고 A,C만 보고 다 왔다고 착각할 수 있습니다. 그렇기 때문에 A,,B,C라는 패킷에 1,2,3이라는 번호를 부여하여 패킷의 분실 확인과 같은 처리를 하여 목적지에서 재조립을 합니다. 이런 방식으로 TCP는 패킷을 추적하며, 나누어 보내진 데이터를 받고 조립을 할 수 있습니다.
```

*Q) TCP와 UDP는 왜 나오게 됐는가?*

```
1. IP의 역할은 Host to Host (장치 to 장치)만을 지원한다. 
	장치에서 장치로 이동은 IP로 해결되지만, 
	하나의 장비안에서 수많은 프로그램들이 통신을 할 경우에는 IP만으로는 한계가 있다.
2. 또한, IP에서 오류가 발생한다면 ICMP에서 알려준다. 
	하지만 ICMP는 알려주기만 할 뿐 대처를 못하기 때문에 IP보다 위에서 처리를 해줘야 한다.
	- 1번을 해결하기 위하여 포트 번호가 나오게 됐고, 
	2번을 해결하기 위해 상위 프로토콜인 TCP와 UDP가 나오게 되었다.
	- ICMP : 인터넷 제어 메시지 프로토콜로 네트워크 컴퓨터 위에서 돌아가는 
	운영체제에서 오류 메시지를 전송받는데 주로 쓰임
```

*Q) DNS(Domain Name System)에서 UDP를 사용하는 이유*

```
Request의 양이 작음 -> UDP Request에 담길 수 있다.
3 way handshaking으로 연결을 유지할 필요가 없다. (오버헤드 발생)
Request에 대한 손실은 Application Layer에서 제어가 가능하다.
DNS : port 53번
But, TCP를 사용할 때가 있다! 크기가 512(UDP 제한)이 넘을 때, TCP를 사용해야한다
```

### ref)

[https://inpa.tistory.com/entry/NW-🌐-아직도-모호한-TCP-UDP-개념-❓-쉽게-이해하자](https://inpa.tistory.com/entry/NW-%F0%9F%8C%90-%EC%95%84%EC%A7%81%EB%8F%84-%EB%AA%A8%ED%98%B8%ED%95%9C-TCP-UDP-%EA%B0%9C%EB%85%90-%E2%9D%93-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EC%9E%90)

[https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)

[https://velog.io/@averycode/네트워크-TCPUDP와-3-Way-Handshake4-Way-Handshake](https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake)

[https://aerimforest.tistory.com/246](https://aerimforest.tistory.com/246)

[https://nordvpn.com/ko/blog/tcp-udp-comparison/](https://nordvpn.com/ko/blog/tcp-udp-comparison/)

[https://velog.io/@mooh2jj/TCP-vs-UDP](https://velog.io/@mooh2jj/TCP-vs-UDP)

[https://daengsik.tistory.com/30](https://daengsik.tistory.com/30)

[https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/main/Network#tcp와-udp의-비교](https://github.com/JaeYeopHan/Interview_Question_for_Beginner/tree/main/Network#tcp%EC%99%80-udp%EC%9D%98-%EB%B9%84%EA%B5%90)

[https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer Science/Network/UDP.md#20190826월-bym-udp란](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/UDP.md#20190826%EC%9B%94-bym-udp%EB%9E%80)