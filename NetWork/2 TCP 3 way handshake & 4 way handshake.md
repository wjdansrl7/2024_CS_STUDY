# 2. TCP 연결 : 3 way handshake & 4 way handshake

***FLAG 종류***

- SYN(synchronize equence numbers : 연결 요청): 연결 요청을 위한 무작위의 숫자 값
- ACK(acknowledgements : 승인): 패킷을 받은 뒤에, 잘 받았다고 알려준다.
- FIN: 접속 종료, 이 패킷을 보내는 곳이 현재 접속하고 있는 곳과 접속을 끊고자 함을 의미

### - 3 way handshake (연결 과정)

![출처) [https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)](images/img4.png)

출처) [https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)

1. 상대에게 통신을 하고 싶다는 메시지 **SYN(seq : x)**를 보내고 SYN_SENT 상태로 대기
2. 서버가 **SYN(x)**을 받고  SYN-RECEIVED 상태로 바꿈, 메시지에 대한 응답 + 나도 통신 준비가 되었다는 메시지를 클라이언트에 보냄 **SYN+ACK (seq : y, ACK: x+1)**
3. **SYN+ACK**를 받은 클라이언트는 ESTABLISHED 상태로 변경하고 서버에 응답 **ACK(y+1)**를 보낸다. 응답 (ACK)를 받은 서버도 ESTABLISHED 상태로 변경된다.

![출처) [https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)](images/img5.png)

출처) [https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)

### - 데이터 통신 과정

![출처) [https://inpa.tistory.com/entry/NW-🌐-아직도-모호한-TCP-UDP-개념-❓-쉽게-이해하자](https://inpa.tistory.com/entry/NW-%F0%9F%8C%90-%EC%95%84%EC%A7%81%EB%8F%84-%EB%AA%A8%ED%98%B8%ED%95%9C-TCP-UDP-%EA%B0%9C%EB%85%90-%E2%9D%93-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EC%9E%90)](images/img6.png)

출처) [https://inpa.tistory.com/entry/NW-🌐-아직도-모호한-TCP-UDP-개념-❓-쉽게-이해하자](https://inpa.tistory.com/entry/NW-%F0%9F%8C%90-%EC%95%84%EC%A7%81%EB%8F%84-%EB%AA%A8%ED%98%B8%ED%95%9C-TCP-UDP-%EA%B0%9C%EB%85%90-%E2%9D%93-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EC%9E%90)

1. ESTABLISHED 된 상태에서 서버에게 데이터를 보냄
2. 서버는 잘 받았다고 (ACK) 응답
3. 만약 클라이언트가 서버로부터 (ACK)를 못받았다면 제대로 송신하지 못한걸로 판단하고 데이터를 재전송

### - 4 way handshake (연결 해제 과정)

![출처) [https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)](images/img7.png)

출처) [https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)

1. 먼저 close를 실행한 클라이언트가 **FIN(종료)**을 보내고 FIN-WAIT-1 상태로 대기
2. **FIN**을 받고 서버는 CLOSE-WAIT 상태로 변경(모든 데이터를 보내기 위해)후 응답 **ACK** 전달과 함께 해당 포트에 연결되어 있는 애플리케이션에게 close 요청, 만약 서버에서 클라이언트로 보낼 남은 데이터가 있을 경우 나머지를 모두 전송시킴, **ACK**를 받은 클라이언트는 상태를 FIN-WAIT-2로 변경
3. close 요청을 받은 서버 애플리케이션은 종료 프로세스를 진행 후 **FIN**을 클라이언트로 보내고LAST-ACK 상태로 변경
4. **FIN**을 받은 클라이언트는 **ACK**를 서버에 다시 전송 후 TIME-WAIT 상태로 변경 (아직 서버로부터 받지 못한 데이터 기다림) → TIME-WAIT 상태에서 일정 시간이 지나면 (CLOSED)된다. **ACK**를 받은 서버도 소켓을 닫음(CLOSED)

![출처) [https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)](images/img8.png)

출처) [https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)

### ref)

[https://dev-coco.tistory.com/144](https://dev-coco.tistory.com/144)

[https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer Science/Network/TCP 3 way handshake %26 4 way handshake.md](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Network/TCP%203%20way%20handshake%20%26%204%20way%20handshake.md)

[https://inpa.tistory.com/entry/NW-🌐-아직도-모호한-TCP-UDP-개념-❓-쉽게-이해하자](https://inpa.tistory.com/entry/NW-%F0%9F%8C%90-%EC%95%84%EC%A7%81%EB%8F%84-%EB%AA%A8%ED%98%B8%ED%95%9C-TCP-UDP-%EA%B0%9C%EB%85%90-%E2%9D%93-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EC%9E%90)