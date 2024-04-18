# 필터(Filter)와 인터셉터(Interceptor)

# 필터(Filter)

- J2EE 표준 스펙 기능으로 디스패처 서블릿(Dispatcher Servlet)에 요청이 전달되기 전/후에 url 패턴에 맞는 모든 요청에 대해 부가작업을 처리할 수 있는 기능을 제공
- 스프링 범위 밖에서 처리
- 웹 컨테이너(서블릿 컨테이너)에 의해 관리

# 필터의 흐름

HTTP 요청  ->  WAS ->  필터 ->  서블릿 ->  컨트롤러

필터체인: HTTP 요청 -> WAS -> 필터 1 ~ N  -> 서블릿 -> 컨트롤러

![Untitled](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/afadf0a1-fc03-4cb7-8db0-b5d2e9e4c06c)

# 필터(Filter)의 메소드 - Filter 인터페이스를 구현해야함!

- init() - init 메소드는 필터 객체를 초기화하고 서비스에 추가하기 위한 메소드이다. 웹 컨테이너가 1회 init 메소드를 호출하여 필터 객체를 초기화하면 이후의 요청들은 doFilter를 통해 처리된다.
- doFilter() - doFilter 메소드는 url-pattern에 맞는 모든 HTTP 요청이 디스패처 서블릿으로 전달되기 전에 웹 컨테이너에 의해 실행되는 메소드이다. doFilter의 파라미터로는 FilterChain이 있는데, FilterChain의 doFilter 통해 다음 대상으로 요청을 전달하게 된다. chain.doFilter() 전/후에 우리가 필요한 처리 과정을 넣어줌으로써 원하는 처리를 진행할 수 있다.
- destory() - destroy 메소드는 필터 객체를 서비스에서 제거하고 사용하는 자원을 반환하기 위한 메소드이다. 이는 웹 컨테이너에 의해 1번 호출되며 이후에는 이제 doFilter에 의해 처리되지 않는다.

```java
public interface Filter {

    public default void init(FilterConfig filterConfig) throws ServletException {}

    public void doFilter(ServletRequest request, ServletResponse response,
            FilterChain chain) throws IOException, ServletException;

    public default void destroy() {}
}

// 출처: https://mangkyu.tistory.com/173 [MangKyu's Diary:티스토리]
```

# 필터의 사용 사례

- 보안 및 인증/인가 관련 작업
- 모든 요청에 대한 로깅 또는 검사
- 이미지/데이터 압축 및 문자열 인코딩
- Spring과 분리되어야 하는 기능

# 인터셉터(Interceptor)

- Spring이 제공하는 기술로써, 디스패처 서블릿(Dispatcher Servlet)이 컨트롤러를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할 수 있는 기능을 제공
- 스프링 컨텍스트에서 동작
- 요청에 대한 작업 전 / 후로 가로챈다
- 스프링 MVC가 제공
- 공통 코드 사용으로 코드 재사용성 증가
- 로깅, 모니터링 정보 수집, 접근 제어 처리 등의 실제 Business Logic과는 분리되어 처리해야 하는 기능들을 넣고 싶을 때 유용
- interceptor를 여러 개 설정 할 수 있으므로 순서 주의
- AOP(비웹) vs 인터셉터(웹) - aop는 메소드 기반, 인터셉터는 HttpRequest, HttpResponse와 같은 웹과 관련된 로직

# 인터셉터 흐름

HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 스프링 인터셉터 -> 컨트롤러

인터셉터 체인 : HTTP 요청 -> WAS -> 필터 -> 서블릿 -> 스프링 인터셉터1 ~ N -> 컨트롤러

# 인터셉터의 메소드

- preHandle()
    - 컨트롤러가 호출되기 전에 실행
    - 컨트롤러 이전에 처리해야 하는 전처리 작업이나 요청 정보를 가공하거나 추가하는 경우에 사용
    - 반환 타입이 boolean 타입으로 true일 경우 계속 진행하고 false일 경우 진행을 멈춘다.
    - 3번째 파라미터인 handler 파라미터는 핸들러 매핑이 찾아준 컨트롤러 빈에 매핑되는 HandlerMethod라는 새로운 타입의 객체로써, @RequestMapping이 붙은 메소드의 정보를 추상화한 객체
- postHandle()
    - 컨트롤러가 호출된 후에 실행된다
    - 컨트롤러 이후에 처리해야 하는 후처리 작업이 있을 때 사용할 수 있다.
    - 메소드에는 컨트롤러가 반환하는 ModelAndView 타입의 정보가 제공되는데, 최근에는 Json 형태로 데이터를 제공하는 RestAPI 기반의 컨트롤러(@RestController)를 만들면서 자주 사용되지는 않는다.
    - 컨트롤러 하위 계층에서 작업을 진행하다가 중간에 예외가 발생하면 postHandle은 호출되지 않음.
- afterCompletion()
    - view 페이지가 렌더링 되고 난 후 호출
    - 예외가 발생하여도 실행

![Untitled 1](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/787b7a5d-86a6-4abc-ba09-14cddc16cc57)

```java
public interface HandlerInterceptor {

    default boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
        throws Exception {
        
        return true;
    }

    default void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
        @Nullable ModelAndView modelAndView) throws Exception {
    }

    default void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
        @Nullable Exception ex) throws Exception {
    }
}
```

# 인터셉터의 사용 사례

- 세부적인 보안 및 인증/인가 공통 작업
- API 호출에 대한 로깅 또는 검사
- Controller로 넘겨주는 정보(데이터)의 가공

![Untitled 2](https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/d001c4bf-848f-47a9-ae5a-0843cdf30db7)

참고자료

[https://mangkyu.tistory.com/173](https://mangkyu.tistory.com/173)

[https://dev-coco.tistory.com/173](https://dev-coco.tistory.com/173)

[https://mozzi-devlog.tistory.com/9](https://mozzi-devlog.tistory.com/9)
