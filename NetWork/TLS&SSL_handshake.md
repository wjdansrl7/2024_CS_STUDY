## 개념
### SSL(Secure Sockets Layer)
> 전달되는 모든 데이터를 암호화하고, 특정한 유형의 사이버 공격도 차단하는 암호화 기반 인터넷 보안 프로토콜<br/>
> 암호화를 사용하여 데이터의 무결성과 기밀성을 보호<br/>
> 컴퓨터 네트워크를 통해 통신 보안을 제공하는 C언어로 작성된 암호화 프로토콜<br/>

### TLS(Transport Layer Security)
> 인터넷을 통한 보안 통신을 위한 표준<br/>
> 클라이언트/서버 애플리케이션이 도청 및 정보 변조를 방지하도록 설계된 방식으로, 네트워크를 통해 통신할 수 있도록 함.<br/>
> TLS는 SSL의 업데이트 버전이며, 명칭만 다르다고 볼 수 있음.

### CA(Certificate authority)
> 인증서의 역할은 클라이언트가 접속한 서버가 의도한 서버가 맞는지 보장하는 것 <br/>
> 이 역할을 하는 민간 기업들이 있는데 이런 기업들을 CA(Certificate authority) 혹은 Root Certificate라고 부름.<br/>
> CA는 신뢰성이 엄격하게 공인된 기업들만 참여.<br/>
> 개발자 입장에서 HTTPS를 적용하려면 신뢰할 수 있는 CA 기업의 인증서를 구입해야 한다.<br/>
> 이 인증서를 구입하게 되면 CA 기업의 개인 키를 이용하여 암호화 한 인증서를 준다.

### SSL 인증서
> 사용자가 서버의 진위 및 전송되는 데이터(서버와 사용자 간)의 암호화를 확인할 수 있는 서버 인증서.

### HTTPS: 하이퍼 텍스트 전송 프로토콜 보안(Hyper Text Transfer Protocol Secure)
> 웹사이트가 SSL/TLS 인증서로 보호되는 경우 HTTPS가 URL에 표시 <br/>
> 사용자는 브라우저 표시줄의 자물쇠 기호를 클릭해 발급 기관 및 웹사이트 소유자의 상호를 포함한 인증서의 세부 정보를 볼 수 있습니다.

## SSL/TLS의 필요성
- 고객이 결제 영역과 공유하는 신용카드 정보가 안전하다는 것을 납득하면 구매에 이를 가능성이 더 높습니다.
- SSL은 사용자 이름과 비밀번호는 물론 개인 정보 제출에 사용되는 양식, 문서 또는 이미지를 암호화하고 보호합니다.
- 결제정보나 민감한 정보를 수집하지 않는 블로그나 웹사이트도 사용자 활동의 보호를 위해 HTTPS가 필요합니다.
- 세션 기간 동안 암호화된 모든 데이터를 교환.
- 써드 파티에서 인터넷을 통해 들어오는 정보들을 읽거나 수정하지 못하도록 막아줌.
    

## SSL/TLS의 작동 방식
- SSL은 개인정보 보호를 제공하기 위해, 웹에서 전송되는 데이터를 암호화 한다.
- 데이터를 가로채려해도 거의 복호화가 불가능하다.
- SSL은 클라이언트와 서버간에 핸드셰이크를 통해 인증이 이루어진다.
- 데이터 무결성을 위해 데이터에 디지털 서명을 하여, 데이터가 의도적으로 도착하기 전에 조작된 여부를 확인한다.

SSL/TLS HandShake
> Handshake는 악수를 의미하는데, 통신을 하는 브라우저와 웹 서버가 서로 암호화 통신을 시작할 수 있도록 신분을 확인하고, 필요한 정보를 클라이언트와 서버가 주거니 받거니 하는 과정이 악수와 비슷하여 붙여진 이름. <br/>
> 보안 협상 목적으로, SSL 내에서 사용되는 프로토콜.<br/>
> HTTPS에서 클라이언트와 서버간 통신 전 SSL 인증서로 신뢰성 여부를 판단하기 위해 연결하는 방식

<br/><br/>
<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/513fc79a-3a81-46df-8b67-bca5202e3940" width="500px">

진행 순서(하이브리드 방식: 공개키 암호화 방식 -> 대칭키 암호화 방식)
1. '클라이언트 헬로' 메시지: 클라이언트가 서버로 "헬로" 메시지를 전송하면서 핸드셰이크를 개시, 이 메시지에는 클라이언트가 지원하는 TLS 버전, 지원되는 암호 제품군, 그리고 "클라이언트 무작위"라고 하는 무작위 바이트 문자열이 포함.
2. '서버 헬로' 메시지: 클라이언트 헬로 메시지에 대한 응답으로 서버가 서버의 SSL 인증서, 서버에서 선택한 암호 제품군, 그리고 서버에서 생성한 또 다른 무작위 바이트 문자열인 "서버 무작위"를 포함하는 메시지를 전송.
3. 인증: 클라이언트가 서버의 SSL 인증서를 인증서 발행 기관을 통해 검증. 이를 통해 서버가 인증서에 명시된 서버인지, 그리고 클라이언트가 상호작용 중인 서버가 실제 해당 도메인의 소유자인지를 확인.
4. 예비 마스터 암호: 클라이언트가 "예비 마스터 암호"라고 하는 무작위 바이트 문자열을 하나 더 전송, 예비 마스터 암호는 공개 키(서버의 SSL 인증서를 통해 받은 공개 키)로 암호화되어 있으며, 서버가 개인 키로만 해독할 수 있음.
5. 개인 키 사용: 서버가 예비 마스터 암호를 해독.
6. 세션 키 생성: 클라이언트와 서버가 모두 클라이언트 무작위, 서버 무작위, 예비 마스터 암호를 이용해 세션 키를 생성. 모두 같은 결과가 나와야 함. => 대칭키 암호화 방식을 사용하기 위해.
7. 클라이언트 준비 완료: 클라이언트가 세션 키로 암호화된 "완료" 메시지를 전송합니다.
8. 서버 준비 완료: 서버가 세션 키로 암호화된 "완료" 메시지를 전송합니다.
9. 안전한 대칭 암호화 성공: 핸드셰이크가 완료되고, 세션 키를 이용해 통신이 계속 진행됩니다.


## 그림으로 이해하기

<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/eedabfa0-560c-4f2f-821f-0c80a267331a" width="500px">
<br/><br/>
1. 사이트는 CA에 사이트 정보와 사이트 공개 키를 보낸다.
<br/><br/>
<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/2e70e18f-4eb5-4b01-8b2c-82f17a16e918" width="500px">
<br/><br/>
2. CA는 자신의 개인 키로 사이트 정보와 사이트 공개 키에 대해 암호화하여 인증서를 생성한 뒤, 인증서를 사이트에게 전달한다.
<br/><br/>
<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/7b7d5879-e3eb-46ec-9ea0-b91447f3f8e8" width="500px">
<br/><br/>
3. 사용자는 CA의 공개 키가 브라우저에 내장되어 있다고 가정한다. <br/>
4. 사용자는 사이트에 접속을 요청한다. (ClientHello) <br/>
5. 사이트는 사용자의 Cipher Suite 중 하나를 고르고, 자신의 SSL 프로토콜 버전을 사용자에게 알린다. (SeverHello) <br/>
6. 사이트는 자신의 사이트 인증서를 사용자에게 전송한다. (Certificate) <br/>
<br/><br/>

<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/5920d235-0578-4d67-8222-6aeccf492cd0" width="500px">
<br/><br/>
7. 사용자는 브라우저에 내장된 CA의 공개 키를 이용하여 사이트 인증서를 복호화하면서 인증서가 유효한지 검증하고, 사이트의 공개 키를 가져온다. (Certificate & ServerHello Done) 참고로, 현재 사이트의 공개 키가 있으므로 Server Key Exchange 과정은 생략된다. 
<br/><br/>
<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/90adba5a-7e8d-48e2-88d6-6f83f7b4a4d2" width="500px">
<br/><br/>
8. 사용자는 자신이 전달할 데이터를 암호화 할 대칭 키를 만들고, 그 대칭 키를 사이트 공개 키로 암호화한다. (Client Key Exchange)
<br/><br/>
<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/7d9be981-5c54-45ec-80b8-52fa34acf553" width="500px">
<br/><br/>
9. 암호화한 대칭 키를 사이트에게 전달한다. (사용자 입장: ChangeCipherSpec, Finished) <br/>
10. 사이트는 자신의 개인 키를 사용하여 사용자 대칭 키를 복호화하여 사용자 대칭 키를 얻어 낸다.(사이트 입장: ChangeCipherSpec, Finished)
<br/><br/>
<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/421b6dcd-e3a5-49ef-94fe-9746bab1ced3" width="500px">
<br/><br/>
11. 이렇게 얻은 대칭 키를 활용하여 서로가 서로의 데이터를 안전하게 복호화 하면서 통신할 수 있다. (SSL Handshake 종료)
<br/><br/><br/>
참고자료
- https://wangin9.tistory.com/entry/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90-URL-%EC%9E%85%EB%A0%A5-%ED%9B%84-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94-%EC%9D%BC%EB%93%A4-5TLSSSL-Handshake <br/>
- https://www.ssl.com/article/ssl-tls-handshake-ensuring-secure-online-interactions/ <br/>
- https://www.cloudflare.com/ko-kr/learning/ssl/what-happens-in-a-tls-handshake/ <br/>
- https://steady-coding.tistory.com/512 <br/>
- https://kanoos-stu.tistory.com/46 <br/>
- https://powerdmarc.com/ko/difference-between-ssl-and-tls/ <br/>
