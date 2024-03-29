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

## http
1. put은 어떤 메서드인지 설명하세요.
2. http란 무엇인가.

## 쿠키
1. 쿠키와 세션의 차이점을 설명하세요.
2. 쿠키와 세션의 사용 이유를 설명하세요.

## DNS
1. DNS Resolver는 SLD의 IP를 가지고 있다. (O,X)
2. DNS란 무엇인가.

## 웹통신흐름
1. 웹 통신흐름의 작동방식을 설명하세요.
2. 웹이란 무엇인가.

## 프록시 서버
1. Forward proxy의 장점 가운데 클라이언트 보안이 있는데, 프록시 방화벽과 일반 방화벽과의 차이점은?
2. Reverse proxy의 장점?

## SOP & CORS
1. 동일하지 않은 origin은?
1) http://example.com:80/app1/index.html
2) http://example.com/app2/index.html
3) http://example.com
4) https://example.com

2. Preflight Request는 어떤 HTTP 메소드를 통해 요청이 될까요?


## 네트워크란

- 병목현상(Bottle neck)이란?   
- Network Bandwitdh은 네트워크의 실질적인 성능을 나타낸다(o, x). o/x에 대한 이유도 같이 적으시오.

## OSI 7계층

- Network Layer 가 가지는 역할은 무엇인지 설명하시오.
- 캡슐화는 데이터를 수신받을 때 사용된다(O, X)

## REST API

- REST는 HTTP URI를 통해 자원을 명시하고 HTTP Method를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다 (O, x)

- 다음 중 REST API 설계 규칙에 따른 예시로 적절한 것은?
1) http://khj93.com/test/  
2) http://khj93.com/test_blog
3) http://khj93.com/photo.jpg  
4) http://khj93.com/post/1  

## Blocking & Non Blocking IO

- Blocking은 A 함수가 B 함수를 호출 할 때, B 함수가 자신의 작업이 종료되기 전까지 A 함수에게 제어권을 돌려주지 않는 것을 말한다.(O, X)

- IO 시간 동안 다른 일을 수행하고 싶다면, Sync-Blocking 을 사용하면 된다.(O, X)