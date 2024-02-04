# RESTful API

# API
- 웹 기반 소프트웨어 애플리케이션에 액세스하기 위한 일련의 프로그래밍 지침 및 표준.
- 통신에 참여하는 애플리케이션 간의 약속, 규약이다. 소통의 목적과 종류에 따라 사용하는 API가 달라진다.

> 프론트, 백엔드 간의 긴밀한 실시간 소통이 필요하면 ⇒ WebSocket API를 고려해볼 수 있다.

> 그냥 일반적인 소통이고 뭐 그닥 특별한 것 없다 하면 ⇒ REST API를 고려해볼 수 있다.

> 빅데이터 업체에서 로그, 어제 시청자 통계처럼 다계층적인 데이터를 백엔드 개발자에게 제공해야 한다면 ⇒ GraphQL API를 고려해볼 수 있다.

{과목} {내용} 문서 작성(수정) [이름]


# REST(Representational State Transfer)
- 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것을 의미합니다.

> HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고

> HTTP Method(POST, GET, PUT, DELETE, PATCH 등)를 통해 

> 해당 자원(URI)에 대한 CRUD Operation을 적용하는 것을 의미합니다.



## RESTful API 
- REST 아키텍처 스타일의 규칙을 지켜 디자인한 API
- REST의 규칙을 모두 지키기에, 구현이 어렵고 오히려 개발 생산성이나 보안에 취약해질 수도 있다. 

## REST API
- REST의 모든 규칙을 지키는 건 비효율적이기에, 규칙을 좀 줄여 융통성 있게 만든 API


## REST 구성요소
1. 자원 : HTTP URI
2. 자원에 대한 행위 : HTTP METHOD
3. 자원에 대한 행위의 내용 : HTTP Message Pay Load


## REST의 특징
1. Server-Client(서버-클라이언트 구조)
2. Stateless(무상태) 
3. Cacheable(캐시 처리 가능) 
4. Layered System(계층화) 
5. Uniform Interface(인터페이스 일관성)



## REST의 장단점
### 장점 

- HTTP 프로토콜의 인프라를 그대로 사용하므로 REST API 사용을 위한 별도의 인프라를 구출할 필요가 없다.
- HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해 준다.
- HTTP 표준 프로토콜에 따르는 모든 플랫폼에서 사용이 가능하다.
- Hypermedia API의 기본을 충실히 지키면서 범용성을 보장한다.
- REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
- 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
- 서버와 클라이언트의 역할을 명확하게 분리한다.
 

### 단점 

- 표준이 자체가 존재하지 않아 정의가 필요하다.
- HTTP Method 형태가 제한적이다.
- 브라우저를 통해 테스트할 일이 많은 서비스라면 쉽게 고칠 수 있는 URL보다 Header 정보의 값을 처리해야 하므로 전문성이 요구된다.
- 구형 브라우저에서 호환이 되지 않아 지원해주지 못하는 동작이 많다.(익스폴로어)


## REST API 설계 예시
1. URI는 동사보다는 명사를, 대문자보다는 소문자를 사용하여야 한다.

> Bad Example http://khj93.com/Running/

> Good Example  http://khj93.com/run/  
 

2. 마지막에 슬래시 (/)를 포함하지 않는다.

> Bad Example http://khj93.com/test/  

> Good Example  http://khj93.com/test
 

3. 언더바 대신 하이폰을 사용한다.

> Bad Example http://khj93.com/test_blog

> Good Example  http://khj93.com/test-blog  
 

4. 파일확장자는 URI에 포함하지 않는다.

> Bad Example http://khj93.com/photo.jpg  

> Good Example  http://khj93.com/photo  
 

5. 행위를 포함하지 않는다.

> Bad Example http://khj93.com/delete-post/1  

> Good Example  http://khj93.com/post/1  



</br>

--------------

[출처]
- https://engineerinsight.tistory.com/356#%E2%9C%94%EF%B8%8F%C2%A0API-1
- https://khj93.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-REST-API%EB%9E%80-REST-RESTful%EC%9D%B4%EB%9E%80