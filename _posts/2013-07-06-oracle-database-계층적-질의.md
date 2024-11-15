---
layout: post
date: 2013-07-06 22:29:00 +0900
title: '[Oracle Database] 계층적 질의'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - hierarchical-join
  - sql
---

* Kramdown table of contents
{:toc .toc}

오라클에서는 계층적인 데이터를 저장한 컬럼으로부터 데이터를 검색하여 계층적으로 출력할 수 있는 기능을 제공한다. SELECT 문에서 START WITH와 CONNECT BY 절을 이용하여 데이터를 계층적인 형태로 출력할 수 있다.

- START WITH: 절 계층적인 출력 형식을 표현하기 위한 최상위 행
- CONNECT BY: 절 계층관계의 데이터를 지정하는 컬럼
- PRIOR 연산자: CONNECT BY는 PRIOR 연산자와 함께 사용하여 부모 행을 확인할 수 있다. PRIOR 연산자의 위치에 따라 top-down 방식인지 bottom-up 방식인지를 결정한다. PRIOR 연산자가 붙은 쪽의 컬럼이 부모 행이 된다.
- WHERE 절: where 절이 JOIN을 포함하고 있을 경우 CONNECT BY 절을 처리하기 전에 JOIN 조건부를 적용하여 처리하고, JOIN을 포함하고 있지 않을 경우 CONNECT BY 절을 처리한 후에 WHERE 절의 조건을 처리한다.
- LEVEL: 계층적 질의문에서 검색된 결과에 대해 계층별로 레벨 번호 표시, 루트 노드는 1, 하위 레벨로 갈수록 1씩 증가

## SELECT

### emp 테이블에서 매니저 사번 확인

```sql
SELECT empno, ename, mgr FROM emp;


     EMPNO ENAME             MGR
---------- ---------- ----------
      7369 SMITH            7902
      7499 ALLEN            7698
      7521 WARD             7698
...
      7839 KING

 14개의 행이 선택됨
```

emp테이블의 모든 직원은 매니저가 있음을 알 수 있다.

### LEVEL을 이용하여 데이터 확인

```
SELECT LEVEL, 컬럼1, 컬럼2...
FROM 테이블
WHERE 조건
START WITH 계층의 시작점이 될 행을 구별하는 논리식
CONNECT BY 계층을 구성할 때 사용될 논리식
```

```sql
SELECT LEVEL, empno, ename, mgr
FROM emp
START WITH mgr IS NULL
CONNECT BY PRIOR empno=mgr;


     LEVEL      EMPNO ENAME             MGR
---------- ---------- ---------- ----------
         1       7839 KING
         2       7566 JONES            7839
         3       7788 SCOTT            7566
         4       7876 ADAMS            7788
         3       7902 FORD             7566
...
 14개의 행이 선택됨
```

WHERE절에 앞서 START WITH와 CONNECT BY가 먼저 처리 된다. START WITH는 계층의 중심, 시작점을 의미하며 CONNECT BY는 상위 계층과 하위 계층의 행을 연결하는 규칙이며 생략은 불가능하다. PRIOR는 일종의 연산자이며 계층이 구성되는 방향을 결정한다.

### LPAD 함수로 계층 표현

```sql
SELECT LPAD(' ', LEVEL*5, ' ')||empno||'_'||ename
FROM emp
START WITH mgr IS NULL
CONNECT BY PRIOR empno=mgr;

->
LPAD('',LEVEL*5,'')||EMPNO||'_'||ENAME
------------------------------------------------------
    7839_KING
        7566_JONES
            7788_SCOTT
                7876_ADAMS
            7902_FORD
                7369_SMITH
        7698_BLAKE
```

```sql
SELECT LPAD('.', LEVEL*4-4, '....') || empno AS empno, ename
FROM emp
START WITH ename ='ADAMS'
CONNECT BY empno = PRIOR mgr;

->
EMPNO        ENAME
-----------------------------
7876        ADAMS
....7788        SCOTT
........7566    JONES
............7839     KING
```

### PRIOR / SYS_CONNECT_BY_PATH

```sql
SELECT LPAD(empno, LEVEL*3, ' ') AS empno, ename,
    PRIOR ename AS mgrname,
    SYS_CONNECT_BY_PATH(ename, '/') AS path
FROM emp
START WITH mgr IS NULL
CONNECT BY PRIOR empno=mgr;


EMPNO        ENAME     MGRNAME     PATH
----------------------------------------------------------------
783        KING                /KING
  7566        JONES        KING        /KING/JONES
     7788    SCOTT        JONES        /KING/JONES/SCOTT
        7876    ADAMS        SCOTT        /KING/JONES/SCOTT/ADAMS
     7902    FORD        JONES        /KING/JONES/FORD
        7369    SMITH        FORD        /KING/JONES/FORD/SMITH
  7698        BLAKE        KING        /KING/BLAKE
     7499    ALLEN        BLAKE        /KING/BLAKE/ALLEN
     7521    WARD        BLAKE        /KING/BLAKE/WARD
     7654    MARTIN    BLAKE        /KING/BLAKE/MARTIN
     7844    TURNER    BLAKE        /KING/BLAKE/TURNER
     7900    JAMES        BLAKE        /KING/BLAKE/JAMES
  7782        CLARK        KING        /KING/CLARK
     7934    MILLER        CLARK        /KING/CLARK/MILLER
```

반환되는 레코드가 많을 경우엔 PRIOR를 통해 출력한다.

**SYS_CONNECT_BY_PATH(A, B)**: 연결된 계층의 위치를 확인할 때 사용

### ORDER SIBLINGS BY를 이용한 계층간 정렬

```sql
SELECT LPAD(empno, LEVEL*4, ' ') AS empno, ename, sal,
    PRIOR ename AS mgrname,
    SYS_CONNECT_BY_PATH(ename, '/') AS path
FROM emp
START WITH mgr IS NULL
CONNECT BY PRIOR empno=mgr
ORDER SIBLINGS BY sal
```

![](/images/oracle-hierarchical-query-1.png)

위를 변형하여 답변형 게시판이나 부서관계도를 나타낼 때 사용한다.

```sql
SELECT 매니저사번, 매니저이름, 매니저근무지역,
    매니저급여등급, 자신사번, 자신이름, 자신근무지역, 자신급여등급,
    DECODE (f, 0, LEAD (자신사번) OVER (ORDER BY rnum)) AS 부사수사번,
    DECODE (f, 0, LEAD (자신이름) OVER (ORDER BY rnum)) AS 부사수이름,
    DECODE (f, 0, LEAD (자신근무지역) OVER (ORDER BY rnum)) AS 부사수근무지역,
    DECODE (f, 0, LEAD (자신급여등급) OVER (ORDER BY rnum)) AS 부사수급여등급
FROM (
    SELECT
        PRIOR E.empno AS 매니저사번,
        PRIOR E.ename AS 매니저이름,
        PRIOR d.loc AS 매니저근무지역,
        PRIOR s.grade AS 매니저급여등급,
        E.empno AS 자신사번,
        E.ename AS 자신이름,
        d.loc AS 자신근무지역,
        s.grade AS 자신급여등급,
        CONNECT_BY_ISLEAF f,
        ROWNUM rnum
    FROM emp e, dept d, salgrade s
    WHERE e.deptno = d.deptno(+)
    AND e.sal BETWEEN s.losal AND s.hisal
    START WITH e.mgr IS NULL
    CONNECT BY PRIOR e.empno = e.mgr
    ORDER SIBLINGS BY e.empno
)
ORDER BY 자신사번
```

![](/images/oracle-hierarchical-query-2.png)

아래는 잘못된 정렬방법:

```sql
SELECT LPAD(empno, LEVEL*3, ' ') AS empno, ename,
    PRIOR ename AS mgrname,
    SYS_CONNECT_BY_PATH(ename, '/') AS path
FROM emp
START WITH mgr IS NULL
CONNECT BY PRIOR empno=mgr
ORDER BY sal
```

![](/images/oracle-hierarchical-query-3.png)

## DELETE

```
DELETE FROM 테이블
WHERE 컬럼a IN (
    SELECT 컬럼A
    FROM 테이블
    START WITH 컬럼a = 삭제할번호
    CONNECT BY PRIOR 컬럼a = 부모컬럼
)
```

```sql
DELETE FROM board
WHERE boardNum IN(
    SELECT boardNum -- 코드
    FROM board
    START WITH boardNum = 삭제할번호 -- 지워야 할 1차 코드
    CONNECT BY PRIOR boardNum = parent -- 부모 코드
)
```

## example

```sql
CREATE TABLE exam (
    num NUMBER PRIMARY KEY,
    dname VARCHAR2(50) NOT NULL,
    loc VARCHAR2(50),
    parent NUMBER
);
INSERT INTO exam VALUES (10, '공과대학', NULL, NULL);
INSERT INTO exam VALUES (100, '정보미디어학부', NULL, 10);
INSERT INTO exam VALUES (200, '메카트로닉스학부', NULL, 10);
INSERT INTO exam VALUES (101, '컴퓨터공학과', '1호관', 100);
INSERT INTO exam VALUES (102, '멀티미디어학과', '2호관', 100);
INSERT INTO exam VALUES (201, '전자공학과', '3호관', 200);
INSERT INTO exam VALUES (202, '기계공학과', '4호관', 200);
COMMIT;

SELECT num, dname, loc, LEVEL, parent FROM exam
START WITH num=10 -- 출력 시작할 최상위 행
CONNECT BY PRIOR num=parent; -- 계층관계지정
-- (PRIOR 연산자가 붙은 쪽이 부모)

SELECT num, dname, loc, level, parent from exam
START WITH num=10 CONNECT by prior num=parent

SELECT LPAD(' ', (LEVEL-1)*4) || dname AS 조직도 FROM exam
START WITH num=10 CONNECT BY PRIOR num=parent;

SELECT num, dname, loc, LEVEL, parent FROM exam
START WITH num=102 CONNECT BY PRIOR parent=num;

-- 정보미디어 학부만 출력 하지 않음
SELECT num, dname, loc, LEVEL, parent FROM exam WHERE num != 100
START WITH num=10 CONNECT BY PRIOR num=parent;

-- 정보미디어학부 및 정보미디어학부의 과도 출력하지 않음
SELECT num, dname, loc, LEVEL, parent FROM exam
START WITH num=10 CONNECT by PRIOR num=parent AND num != 100;
```
