---
layout: post
date: 2013-07-21 01:00:00 +0900
title: '[Oracle] PL/SQL (Procedure)'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - procedure
---

* Kramdown table of contents
{:toc .toc}

PL/SQL은 오라클 DBMS에서 SQL 언어를 확장하기 위해 사용하는 컴퓨터 프로그래밍 언어이며 'Procedure Language / Structured Query Language'의 약자다. PL/SQL은 주로 자료 내부에서 SQL 명령문만으로 처리하기에는 복잡한 자료의 저장이나 프로시저와 트리거 등을 작성하는 데 쓰인다. SQL보다 속도가 미세하게 빠르며 SQL에서는 불가능한 제어문, 반복문 사용이 가능하다.

#### SET SERVEROUTPUT 변수설정
DBMS_OUTPUT.PUT_LINE에 의한 출력여부를 지정하며, 출력할 수 있는 최대 크기는 32767바이트이다.

```
SET SERVEROUTPUT { ON | OFF }
  [SIZE { n | UNLIMITED }]
  [FORMAT { WRAPPED | WORD_WRAPPED | TRUNCATED }]
```

```sql
SET SERVEROUTPUT ON
```

## Anonymous PL/SQL

```sql
DECLARE --선언부, 생략가능
  n NUMBER := 1;
  s NUMBER := 0;  
BEGIN --프로그램의 시작
  WHILE n < 100 LOOP
  s := s + n;
  n := n + 2;
  END LOOP;
  DBMS_OUTPUT.PUT_LINE('결과 : ' || s);
-- EXCEPTION -- 예외처리, 생략가능
END;
/
```

## Procedure와 Function

오라클에서 생성된 Stored Procedure는 다른 aplication(JAVA, .NET, VC++)에서도 호출할 수 있다.

프로시저에 에러가 발생하면 자동으로 롤백 처리된다. 이에 비해 커밋은 자동으로 되지 않으므로 반드시 프로시저 마지막에 `COMMIT`을 명시할 것.

### Stored Procedure와 Function의 차이점

- Stored Procedure는 반환값이 있을 수도 있고 없을 수도 있다. (IN MODE, OUT MODE, IN OUT MODE)
- Function은 반드시 그 결과값을 반환한다.

#### Stored Procedure

```sql
CREATE OR REPLACE PROCEDURE 프로시저이름 (
    변수명 [MODE] DATATYPE,   --mode(IN|OUT|IN OUT)를 생략하면 IN,
    변수명 [MODE] DATATYPE
)
IS
    [선언문];
    -- v_cnt NUMBER := 0;   --프로시저 선언부에서 변수초기화하는 방법
BEGIN
    실행문;
    [EXCEPTION]
END;
/
```

#### Function

```sql
CREATE OR REPLACE FUNCTION 함수명 (
    변수명 [MODE] DATATYPE,
    변수명 [MODE] DATATYPE
)
RETURN 반환할데이터타입 --값
IS
    [선언문];
BEGIN
    실행문;
    [EXCEPTION]
END;
/
```

## 프로시저 호출

```
execute 프로시저명([인자값])
또는
exec 프로시저명([인자값])
```

```sql
EXEC pcd_insa_1(1001);
```

## CURSOR를 활용한 저장 프로시저

하나의 레코드가 아닌 여러행으로 구성된 작업영역에서 SQL 결과를 저장하기 위해 커서를 사용

1. CURSOR 작성
1. CURSOR OPEN
1. FETCH
1. CURSOR CLOSE

```sql
CREATE OR REPLACE PROCEDURE s2
IS
    vname VARCHAR2(20);
    vtotal NUMBER;
    CURSOR score_list IS SELECT NAME, (kor+eng+mat) total FROM score; --1. 작성
BEGIN
      OPEN score_list; --2. 오픈
      LOOP
        FETCH score_list INTO vname, vtotal; --3. 패치
        EXIT WHEN score_list%notfound;
        DBMS_OUTPUT.PUT_LINE('이름 : ' || vname || '   총점: ' || vtotal);
      END loop;
      CLOSE score_list; --4. 종료
END;
/

CREATE OR REPLACE PROCEDURE s3
IS
      CURSOR score_list IS SELECT name, (kor+eng+mat) total FROM score;
BEGIN
      FOR rec IN score_list loop
        dbms_output.put_line(rec.NAME || ' ' || rec.total);
      END loop;
END;
/
```

## SYS_REFCURSOR

쿼리실행 결과를 저장하기 위한 자료형이며 테이블의 여러행을 반복적으로 조회하기 위해 사용한다.

```sql
CREATE OR REPLACE PROCEDURE listAllScore (
    pResult OUT SYS_REFCURSOR
)
IS
BEGIN
    OPEN pResult FOR
        SELECT hak, name, birth, kor, eng, mat,
            kor+eng+mat AS total, (kor+eng+mat)/3 AS avg,
              rank() OVER(ORDER BY (kor+eng+mat) DESC) AS rank
        FROM score;
END;
/

-- 위에서 만든 프로시저를 확인하기 위한 프로시저
CREATE or replace procedure s4
IS
      result SYS_REFCURSOR;
      vHak VARCHAR2(20);
      vName VARCHAR2(20);
      vBirth VARCHAR2(30);
      vKor NUMBER;
      vEng NUMBER;
      vMat NUMBER;
      vTotal NUMBER;
      vAvg NUMBER;
      vRank NUMBER;
BEGIN
     -- 프로시저 호출
      listAllScore(result);
    LOOP
        FETCH result INTO vHak, vName, vBirth, vKor, vEng, vMat, vTotal, vAvg, vRank;
        EXIT WHEN result%notfound;
        DBMS_OUTPUT.PUT_LINE(vName || ' ' || vTotal);
    END LOOP;
END;
/

EXEC s4;
```

## PL/SQL의 제어문

### 조건문

```sql
if 조건 then 문장1;
elsif 조건 then 문장2;
else 문장3;
end if;
```

### 반복문

```sql
-- 반복문1 기본 loop문
loop
문장;
exit when 조건; -- 조건이 true면 루프탈출
end loop;

-- 반복문2 for loop문
for 변수 in 1(최소값)..10(최대값) loop -- 변수 = 1, 2, 3, 4, 5, 6...
문장;
end loop;

for 플래그변수 in reverse 1..10 loop -- 변수 = 10, 9, 8, 7, 6, 5...
문장;
end loop;

-- 반복문3 while loop문
while 조건 loop -- 조건이 true일 동안 loop
문장;
end loop;
```

## 프로시저 작성 예시

```sql
CREATE OR REPLACE PROCEDURE pcd_emp_1(
    v_empno insa.num%TYPE
)
IS
    v_name insa.NAME%TYPE;
    v_jubunck VARCHAR2(2);
    v_sex VARCHAR2(4);
    v_year NUMBER(4);
BEGIN
    SELECT name, SUBSTR(ssn, 7, 1)
        INTO v_name, v_jubunck
    FROM insa
    WHERE num = v_empno;

    IF v_jubunck IN('1', '3') THEN v_sex := '남';
    ELSE v_sex := '여';
    END IF;

    IF v_jubunck IN ('1', '2') THEN v_year := 1900;
    ELSE v_year := 2000;
    END IF;
END;
/

-- INSERT 프로시저
CREATE PROCEDURE insertScore (
    phak IN VARCHAR2, --변수명 IN 자료형 *크기를 명시하지 않는다.
    pname IN score.NAME%TYPE, --score테이블의 name과 같은 자료형을 의미함
    pbirth IN score.birth%TYPE, -- %TYPE : 좌측에 있는 변수의 자료형
    pkor IN score.kor%TYPE,
    peng IN score.eng%TYPE,
    pmat IN score.mat%TYPE
)
IS
BEGIN
    INSERT INTO score (hak, name, birth, kor, eng, mat)
    VALUES (phak, pname, pbirth, pkor, peng, pmat);
    COMMIT;
END;
/

-- UPDATE 프로시저
CREATE OR REPLACE PROCEDURE updateScore (
    phak IN score.hak%TYPE,
    pname IN score.name%TYPE,
    pbirth IN score.birth%TYPE,
    pkor IN score.kor%TYPE,
    peng IN score.eng%TYPE,
    pmat IN score.mat%TYPE
)
IS
BEGIN
    UPDATE score
    SET name = pname, birth = pbirth, kor = pkor, eng = peng, mat = pmat
    WHERE hak = phak;
    COMMIT;
END;
/

EXEC insertScore('979797', '홍길동', '2000-10-10', 80, 80, 80);
CALL insertScore('0404', '바보', '1000-10-10', 11, 22, 33);


-- delete 프로시저
CREATE OR REPLACE PROCEDURE deleteScore (
    phak IN score.hak%TYPE
)
IS
BEGIN
    DELETE FROM score
    WHERE hak = phak;
    COMMIT;
END;
/

EXEC deleteScore(2175);
EXEC insertScore('3401', 'Hooah', '1938-08-09', 10, 20, 30);


--셀렉트 프로시저
CREATE OR REPLACE PROCEDURE selectScore (
    phak VARCHAR2 --매개변수
)
IS
    vname VARCHAR2(20);
    vtot NUMBER;
BEGIN
    SELECT name, (kor + eng + mat) INTO vname, vtot
    FROM score
    WHERE hak = phak;
    DBMS_OUTPUT.PUT_LINE(vname || '  ' || vtot);
END;
/

EXEC s1('2222');


--프로시저 호출
EXEC insertScore('979797', '홍길동', '2000-10-10', 80, 80, 80);
CALL insertScore('0404', '바보', '1000-10-10', 11, 22, 33);
EXEC s1('2222');   -- CALL 혹은 EXEC로 호출한다

--프로시저 목록 확인
SELECT * FROM user_procedures;

--프로시저 소스 확인
SELECT text FROM user_source;

--프로시저, 함수 등의 의존관계 확인
SELECT * FROM user_dependencies;

--프로시저 삭제
DROP PROCEDURE insertScore;
```
