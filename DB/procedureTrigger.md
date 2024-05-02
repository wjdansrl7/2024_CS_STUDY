# procedure trigger

# 프로시저(Procedure)

프로시저란 절차형 SQL을 활용하여 특정 기능을 수행하는 일종의 트랜잭션 언어이며 호출을 통해 실행되어 미리 저장해 놓은  SQL작업을 수행한다. 여러 프로그램에서 호출하여 사용 가능하고, 시스템의 일일 마감 작업, 일괄 작업 등에 주로 사용된다. 스토어드(Stored)프로시저라고도 불린다. 

## 프로시저 구성

- DECLARE - 프로시저의 명칭, 변수, 인수, 데이터 타입을 정의하는 선언부 (필수)
- BEGIN / END - 프로시저의 시작과 종료를 의미 (필수)
- CONTROL - 조건문 또는 반복문이 삽입, 순차적 처리
- SQL - DML, DCL이 삽입되 조회 추가 수정 삭제 작업을 수행
- EXCEPTION - 구문 실행 중 예외 발생 시 처리 방법 정의
- TRANSACTION - 작업들을 DB에 적용할지 취소할지 결정하는 처리부

## 프로시저 생성과 실행, 제거

- CREATE PROCEDURE 프로시저명(파라미터)
    
    BEGIN
    
    BODY;
    
    END;
    
- CREATE와 PROCEDURE 사이에 OR, PEPLACE 예약어 사용 시 동일한 프로시저 이름이 존재할 경우, 기존의 프로시저를 대체한다.
- 파라미터는 IN, OUT, INOUT, 매개변수명, 자료형이 올 수 있다. 값을 전달하거나 반환할 때 IN, OUT, INPUT을 사용한다.
- 프로시저의 BODY는 BEGIN과 END사이의 코드 작성 부분이며 하나 이상의 SQL문이 있어야한다.
- 프로시저 실행을 위해 EXECUTE, CALL, EXEC로 사용할 수 있다. ex) EXEC 프로시저명;
- 제거를 위해서는 DROP PROCEDURE를 사용한다.

# 트리거(Trigger)

트리거는 DB시스템에서 삽입, 갱신, 삭제 이벤트가 발생할 때마다 자동 수행되는 절차형 SQL이다. 무결성 유지, 로그 메시지 출력 등의 목적으로 사용한다. 구문에는 DCL을 사용할 수 없고 DCL이 포함된 프로시저나 함수 호출도 불가능하다. 

## 트리거 구성

- DECLARE : 프로시저와 같다.
- EVENT : 실행되는 조건 명시
- BEGIN / END : 프로시저와 같다
- CONTROL : 프로시저와 같다
- SQL : 프로시저와 같으나 DCL은 삽입될 수 없다
- EXCEPTION : 프로시저와 같다.

## 트리거 생성

- CREATE TRIGGER 트리거명(실행시기)(옵션) ON 테이블명
    
    REFERENCING (NEW or OLD) AS 테이블명
    
    FOR EACH ROW
    
    BEGIN
    
    BODY;
    
    END;
    
- OR REPLACE는 프로시저와 동일하고, 실행 시기는 이벤트가 발생하기 전에 실행하는지 후에 실행하는지에 따라 AFTER와 BEFORE를 사용한다.
- 실행되게 할 작업의 종류를 지정하는 옵션에는 INSERT, DELETE, UPDATE가 있다.
- 추가되거나 수정에 참여할 테이블은 NEW, 수정되거나 삭제 전 대상이 되는 테이블은 OLD로 사용한다.
- FOR EACH ROW는 각 튜플마다 트리거를 적용한다는 의미이며 BODY부분은 프로시저와 같다.
- 제거는 DROP TRIGGER명령어를 사용한다.

# 프로시저와 트리거의 공통점

- 데이터베이스에 저장한다.
- 절차형 SQL이다
- 구성시 DECLARE, BEGIN, END를 필수로 가져야 한다.

# 프로시저와 트리거의 차이점

- 트리거는 이벤트가 발생할 때 자동으로 호출
    
    (DDL, DML, 또는 일부 DB 작업(LOGOFF, SHUTDOWN)에 대한 응답)
    
    프로시저는 필요할 때마다 호출
    
- 트리거 내부에서 프로시저 정의 가능
    
    프로시저 내부에서 트리거 정의 불가 (트리거는 이벤트 발생 시 자동으로 호출되어야 하므로)
    
- 트리거는 매개 변수 값이나 코드를 반환할 수 없음
    
    프로시저는 매개 변수 값이나 코드를 반환할 수 있음 (리턴값 없어도 됨, 수행하는 절차가 목적이기 때문)
    

출처

[https://m.blog.naver.com/dlguswls9998/221780280565](https://m.blog.naver.com/dlguswls9998/221780280565)

[https://lovi0714.github.io/db/trigger-and-procedure/](https://lovi0714.github.io/db/trigger-and-procedure/)