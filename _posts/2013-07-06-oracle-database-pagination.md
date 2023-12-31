---
layout: post
date: 2013-07-06 23:46:00 +0900
title: '[Oracle Database] pagination'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - pagination
---

* Kramdown table of contents
{:toc .toc}

## ROWNUM을 이용한 페이징

#### A부터 B까지 출력

```sql
SELECT *
FROM (
    SELECT ROWNUM AS rnum, tb.*
    FROM (
        SELECT num, name, content
        FROM guest
        ORDER BY num DESC
    ) tb
    WHERE ROWNUM <= 200
)
WHERE rnum >= 1;

-- 페이지 인덱스 계산을 위한 데이터 총 개수를 한 번에 가져오는 방식
-- 데이터량이 많을 수록 비용이 크므로 추천하지 않는다.
SELECT *
FROM (
    SELECT ROWNUM AS rnum, tb.*, COUNT(1) OVER () AS cnt
    FROM (
        SELECT num, name, content
        FROM guest
        ORDER BY num DESC
    ) tb
    WHERE ROWNUM <= 200
)
WHERE rnum >= 1;
```

#### 바로 위의 레코드 출력

```sql
SELECT *
FROM (
    SELECT num, name, content, created
    FROM GUEST
    WHERE [컬럼_아이디] > [값] ORDER BY num ASC
)
WHERE ROWNUM = 1;
```

#### 바로 아래의 레코드 출력

```sql
SELECT *
FROM (
    SELECT num, name, content, created
    FROM GUEST
    WHERE [컬럼_아이디] < [값] ORDER BY num DESC
)
WHERE ROWNUM = 1;
```

## ROW_NUMBER를 이용한 페이징

```sql
SELECT *
FROM (
    SELECT ROW_NUMBER() OVER(ORDER BY boardNum DESC) AS rnum,
        boardNum, name, subject, hitCount,
        TO_CHAR(created, 'yyyy-mm-dd') AS created
    FROM bbs
)
WHERE rnum >= [start_value] AND rnum <= [end_value];
-- 혹은
-- WHERE rnum >= [start_value] AND rnum <= [end_value] ORDER BY boardNum DESC
```

검색일 경우엔 인라인 뷰에 WHERE절을 적용한다.

```sql
SELECT 컬럼1, 컬럼2
FROM (
    SELECT ROW_NUMBER() OVER(ORDER BY boardNum DESC) AS rnum, 컬럼1, 컬럼2
    FROM bbs
    WHERE 검색조건1, 검색조건2
)
WHERE rnum between [페이지시작값] AND [페이지마지막값]
```

## CEIL(소수점올림)을 이용한 방식

```sql
SELECT empno, ename, job, mgr, hiredate, sal, comm, deptno
FROM (
    SELECT e.*, CEIL((ROW_NUMBER() OVER (ORDER BY empno))/3) AS page
    FROM emp e
)
WHERE page IN (1);


 EMPNO ENAME JOB      MGR  HIREDATE   SAL  COMM DEPTNO
 ----- ----- -------- ---- ---------- ---- ---- ------
  7369 SMITH CLERK    7902 1980-12-17  800 NULL     20
  7499 ALLEN SALESMAN 7698 1981-02-20 1600  300     30
  7521 WARD  SALESMAN 7698 1981-02-22 1250  500     30
```

IN을 사용하기 때문에 다음처럼도 가능하다:

```sql
SELECT empno, ename, job, mgr, hiredate, sal, comm, deptno
FROM (
    SELECT e.*, CEIL((ROW_NUMBER() OVER (ORDER BY empno))/3) AS page
    FROM emp e
)
WHERE page IN (2, 4);

 EMPNO ENAME  JOB      MGR  HIREDATE   SAL  COMM DEPTNO
 ----- ------ -------- ---- ---------- ---- ---- ------
  7566 JONES  MANAGER  7839 1981-04-02 2975 NULL     20 --2페이지
  7654 MARTIN SALESMAN 7698 1981-09-28 1250 1400     30 --2페이지
  7698 BLAKE  MANAGER  7839 1981-05-01 2850 NULL     30 --2페이지
  7844 TURNER SALESMAN 7698 1981-09-08 1500    0     30 --4페이지
  7876 ADAMS  CLERK    7788 1987-05-23 1100 NULL     20 --4페이지
  7900 JAMES  CLERK    7698 1981-12-03  950 NULL     30 --4페이지
```
