# Servlet

## 서블릿

> 1. 동적 웹 페이지를 만들 때 사용되는 자바 기반의 웹 애플리케이션 프로그래밍 기술이다. 서블릿은 웹 요청과 응답의 흐름을 간단한 메서드 호출만으로 체계적으로 다룰 수 있게 해준다.
> 2. 자바로 구현된 CGI

<br>
CGI(Common Gateway Interface)

```
특별한 라이브러리나 도구를 의미하는 것이 아니고, 별도로 제작된 웹서버와 프로그램간의 교환방식입니다.
CGI방식은 어떠한 프로그래밍언어로도 구현이가능하며, 별도로 만들어 놓은 프로그램에
HTML의 Get or Post 방법으로 클라이언트의 데이터를 환경변수로 전달하고,
프로그램의 표준 출력 결과를 클라이언트에게 전송하는 것입니다.즉, 자바 어플리케이션 코딩을 하듯
웹 브라우저용 출력 화면을 만드는 방법
```

<br>

### 참고
서블릿은 톰캣 같은 웹 애플리케이션 서버를 직접 설치하고,그 위에 서블릿 코드를 클래스 파일로 빌드해서 올린 다음, 톰캣 서버를 실행하면 된다. 하지만 이 과정은 매우 번거롭다.

> 스프링 부트는 톰캣 서버를 내장하고 있으므로, 
> 톰캣 서버 설치 없이 편리하게 서블릿 코드를 실행할 수 있다.

내장 톰켓 서버 생성

<img width="543" alt="Untitled" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/6e5b38f9-b539-4325-9a6f-6fc89f41993f">
<img width="550" alt="Untitled 1" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/696442ec-2288-492b-978b-1c71b04efdbb">

### 참고
HTTP 응답에서 Content-Length는 웹 애플리케이션 서버가 자동으로 생성해준다.

<img width="546" alt="Untitled 2" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/6f008008-458b-47bd-a252-cab6f185c2a8">

### HttpServletRequest, HttpServletResponse ⇒ HTTP 메시지, HTTP 응답 메시지를 편리하게 사용할 수 있도록 도와주는 객체

## HttpServletRequest 역할

HTTP 요청 메시지를 개발자가 직접 파싱해서 사용해도 되지만, 매우 불편할 것이다. 서블릿은 개발자가 HTTP 요청 메시지를 편리하게 사용할 수 있도록 개발자 대신에 HTTP 요청 메시지를 파싱한다. 그리고 그 결과를 HttpServletRequest 객체에 담아서 제공한다.

### 임시 저장소 기능

- 해당 HTTP 요청이 시작부터 끝날 때까지 유지되는 임시 저장소 기능
    - 저장: request.setAttribute(name, value);
    - 조회: request.getAttribute(name);

### 세션 관리 기능

request.getSession(create: true);

HTTP 요청 데이터

1. GET - 쿼리 파라미터
    1. /url?username=hello&age=20
    2. 메시지 바디 없이, URL의 쿼리 파라미터에 데이터를 포함해서 전달
    3. 예) 검색, 필터, 페이징 등에서 많이 사용하는 방식
2. POST - HTML Form
    1. content-type: application/x-www-form-urlencoded
    2. 메시지 바디에 쿼리 파라미터 형식으로 전달 username=hello&age=20
    3. 예) 회원 가입, 상품 주문, HTML Form 사용
3. HTTP message body에 데이터를 직접 담아서 요청
    1. HTTP API에서 주로 사용, JSON, XML, TEXT
4. 데이터 형식은 주로 JSON 사용
    1. POST(create), PUT(전체 update), PATCH(일부 update)

HTTP 요청 데이터 - API 메시지 바디 - JSON

- POST
- content-type: application/json
- message-body: {”username”: “hello”, “age”: 20}
- 결과: messageBody = {”username”: “hello”, “age”: 20}

참고

> JSON 결과를 파싱해서 사용할 수 있는 자바 객체로 변환하려면 Jackson, Gson 같은 JSON 변환
라이브러리를 추가해서 사용해야 한다. 스프링 부트로 Spring MVC를 선택하면 기본으로 Jackson 라이브러리(ObjectMapper)를 함께 제공한다.
> 

참고

> HTML form 데이터도 메시지 바디를 통해 전송되므로 직접 읽을 수 있다. 하지만 편리한 파리미터 조회
기능( request.getParameter(...) )을 이미 제공하기 때문에 파라미터 조회 기능을 사용하면 된다.
> 

## HTTPServletResponse 역할

HTTP 응답 메시지 생성

- HTTP 응답코드 지정
- 헤더 생성
- 바디 생성

편의 기능 제공

- Content-Type, 쿠키, Redirect

```java

// Content 편의 메서드

private void content(HttpServletResponse response) {
	//Content-Type: text/plain;charset=utf-8
	//Content-Length: 2
	//response.setHeader("Content-Type", "text/plain;charset=utf-8");
	response.setContentType("text/plain");
	response.setCharacterEncoding("utf-8");
	//response.setContentLength(2);//(생략시 자동 생성)
}
```

```java
// 쿠키 편의 메서드

private void cookie(HttpServletResponse response) {
	//Set-Cookie: myCookie=good; Max-Age=600;
	//response.setHeader("Set-Cookie", "myCookie=good; Max-Age=600");
	Cookie cookie = new Cookie("myCookie", "good");
	cookie.setMaxAge(600); //600초
	response.addCookie(cookie);
}
```

```java
// redirect 편의 메서드

private void redirect(HttpServletResponse response) throws IOException {
    //Status Code 302
    //Location: /basic/hello-form.html
    //response.setStatus(HttpServletResponse.SC_FOUND); //302
    //response.setHeader("Location", "/basic/hello-form.html");
    response.sendRedirect("/basic/hello-form.html");
}

```

HTTP 응답 데이터 - 단순 텍스트, HTML

- 단순 텍스트 응답
    - writer.println(”ok”);
- HTML 응답
- HTTP API - MessageBody JSON 응답

## 서블릿의 동작 과정

<img width="651" alt="Untitled 3" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/2a398091-82a4-4b43-a094-3bbf4781315a">

## 서블릿의 생명주기

1. 초기화 : init()

- 서블릿 요청 시 맨 처음 한 번만 호출된다.
- 서블릿 생성 시 초기화 작업을 주로 수행한다.
1. 작업 수행 : doGet(), doPost()
- 서블릿 요청 시 매번 호출된다.
- 실제로 클라이언트가 요청하는 작업을 수행한다.
1. 종료 : destroy()
- 서블릿이 기능을 수행하고 메모리에서 소멸될 때 호출된다.
- 서블릿의 마무리 작업을 주로 수행한다.
<br>
<img width="396" alt="Untitled 4" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/bbaa62c6-65b5-43c6-b7a4-a0437b4c6f7f">
<br>
## 서블릿 컨테이너

- 서블릿을 관리해주는 컨테이너
- 클라이언트의 요청(Request)을 받아주고 응답(Response)할 수 있게, 웹서버와 소켓으로 통신
- 클라이언트에서 요청을 하면 컨테이너는 HttpServletRequest, HttpServletResponse 두 객체를 생성하여 post, get여부에 따라 동적인 페이지를 생성하여 응답을 보낸다.

### 서블릿 컨테이너의 특징

1. 웹서버와의 통신 지원
    1. 서블릿과 웹서버 손쉽게 통신 지원(소켓 생성 및 listen, accept → API로 제공) → 비즈니스 로직에만 집중
2. 서블릿 생명주기 관리
    1. 서블릿 클래스 인스턴스화 → init() → 요청 시 적절한 서블릿 메서드 호출 → 종료시 destory()를 통해 Garbage Collection을 진행
3. 멀티쓰레드 지원 및 관리
    1. 서버가 다중 쓰레드를 생성 및 운영하므로 쓰레드의 안정성 걱정 안해도 됨. 
4. 선언적인 보안 관리

## JSP(Java Server Page)

- Java 코드가 들어가 있는 HTML 코드
- 서블릿은 자바 소스코드 속에 HTML코드가 들어가는 형태인데, JSP는 이와 반대로 HTML 소스코드 속에 자바 소스코드가 들어가는 구조를 갖는 웹어플리케이션 프로그래밍 기술
- <% 소스코드 %> <%= 소스코드 =%> 형태 ⇒ 웹 서버에서 실행됨.
- 서블릿 규칙은 복잡, JSP는 WAS에 의하여 서블릿 클래스로 변환하여 사용

<img width="784" alt="Untitled 5" src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/d9ab753e-282b-4726-9ce7-7dfabf6f8559">

참고자료
- [https://mangkyu.tistory.com/14](https://mangkyu.tistory.com/14)
- [https://velog.io/@falling_star3/Tomcat-서블릿Servlet이란](https://velog.io/@falling_star3/Tomcat-%EC%84%9C%EB%B8%94%EB%A6%BFServlet%EC%9D%B4%EB%9E%80)
- 김영한의 스프링 MVC 1편
