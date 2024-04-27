# 서블릿

1. HttpServletRequest
2.
  1) 웹 서버와의 통신 지원
  2) 서블릿 생명주기 관리
  3) 멀티쓰레드 지원 및 관리
  4) 선언적인 보안 관리
  
# Lazy Loading & Eager Loading

1. 즉시 로딩이란 엔티티를 조회할 때 자신과 연관되는 엔티티를 조인(join)을 통해 함께 조회하는 방식
	지연 로딩이란 자신과 연관된 엔티티를 실제로 사용할 때 연관된 엔티티를 조회(SELECT) 하는 방식

2. 프록시 객체

# Interceptor & Filter

1. 서블릿 컨테이너, 스프링 컨테이너
2. preHandle()
