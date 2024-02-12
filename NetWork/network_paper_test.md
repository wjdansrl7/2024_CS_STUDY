## 대칭키 & 공개키
1. 대칭키 암호방식과 공개키 암호방식의 차이는?
2. 대칭키와 공개키의 장단점을 보완한 방식을 사용하며, SSL 탄생의 시초가 된 프로토콜은 무엇일까요?

## TLS & SSL handshake
1. SSL/TLS란 무엇인가요?
2. SSL/TLS에 대한 동작방식에 대해 설명해주세요.

## TCP vs UDP
1. 표를 채워주세요.

||&nbsp;&nbsp;&nbsp;TCP&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;UDP&nbsp;&nbsp;&nbsp;|
|:-------------|:--------:|:--------:|
|연결 방식|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
|패킷 교환 방식|||
|전송 순서|||
|수신 여부 확인|||
|통신 방식|||
|신뢰성|||
|속도|||
2. TCP 전송이 UDP 전송보다 느린 이유를 설명해주세요.

## 3 way handshake & 4 way handshake
1. 옳은 것을 고르고 표를 채워주세요.
- 3 way handshake는 (연결 과정/연결 해제 과정)이다.

|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;방향&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;FLAG&nbsp;&nbsp;&nbsp;|
|:--------:|:--------:|:--------:|
|#1|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
|#2|||
|#3|||
2. 옳은 것을 고르고 표를 채워주세요.
- 4 way handshake는 (연결 과정/연결 해제 과정)이다.

|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;방향&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;FLAG&nbsp;&nbsp;&nbsp;|
|:--------:|:--------:|:--------:|
|#1|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
|#2|||
|#3|||
|#4|||

## 흐름 제어 & 혼잡 제어
1. 흐름 제어 & 혼잡 제어의 정의를 알려주세요.
- 흐름 제어 : 
- 혼잡 제어 :
2. Slow Start에 관해 틀린 것을 골라주세요.
    1. Slow Start는 윈도우 크기를 선형적으로 증가시킨다.
    2. Slow Start는 혼잡 현상이 발생하면 윈도우 크기를 1로 떨어트린다.
    3. Slow Start는 AIMD 방식을 개선한 방식이다.

## L4, L7 Load Balancing
1. 로드 밸런싱의 정의를 알려주세요.
2. 로드 밸런서 장애에 대비하는 방법은 무엇일까요?
