---
layout: post
date: 2013-07-06 18:53:00 +0900
title: 'Oracle: DQL'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - dql
---

* Kramdown table of contents
{:toc .toc}

## SELECT-FROM

```
SELECT 컬럼명1, 컬럼명2, ... FROM 테이블명
```


###### SELECT 처리 순서:

- SELECT ----- 5
- FROM ----- 1
- WHERE ----- 2
- GROUP BY ----- 3
- HAVING ----- 4
- ORDER BY ----- 6

순서에 주의할 것. (특히 rownum을 사용할 때)

```sql
-- 테이블 조회
SELECT * FROM TAB;

-- 특정 컬럼만 조회
SELECT empno, ename, sal FROM emp;

-- 특정 컬럼과 연산값 조회
SELECT empno, ename, sal + comm FROM emp;

-- 여기서 comm이 null일 경우 연산이 불가능하므로 잘못된 쿼리로 간주된다.
SELECT empno, ename, sal + NVL(comm, 0) AS pay FROM emp;

-- 컬럼을 연결하여 출력
SELECT empno || ename AS empno_ename FROM emp;
```

### DUAL
ORACLE SYS 계정이 소유한 테이블로 하나의 행을 갖는다. 특정 데이터 혹은 연산을 위해 사용한다.

```
SELECT 출력할 컬럼(연산식이나 함수 등) FROM DUAL
```

```sql
-- 현재시각을 DUAL 테이블로 조회
SELECT SYSDATE FROM DUAL;

-- 계산식 출력
SELECT 1 + 2 + 3 + 4 + 5 FROM DUAL;
```

### ALIAS, 별칭

```
컬럼(상수, 계산값) AS 별칭
```

```sql
SELECT ename AS 이름, sal*12 AS 연봉 FROM emp;

이름                 연봉
---------- ----------
SMITH            9600
ALLEN           19200
WARD            15000
...

-- 공백이 포함된 이름은 ""
SELECT capital AS "수도 이름" FROM country;

-- AS는 생략할 수 있다.
SELECT NAME 이름, capital "수도 이름" FROM dual;

-- 테이블에도 별칭을 사용할 수 있으며 이 경우엔 AS를 생략한다.
SELECT * FROM tab_sample ex1;
```

### CASE

```sql
SELECT
    CASE 'A' -- 비교대상
      WHEN 'A' THEN 1 -- 비교대상이 'A'와 같으면 1 반환
      WHEN 'B' THEN 2 -- 비교대상이 'B'와 같으면 2 반환
      ELSE 3 -- 그 외 모든 조건에 3 반환
    END AS ex
FROM DUAL

 EX
 --
  1
```

단, 위의 경우 EQUAL 비교만 가능하다.

```sql
SELECT
    CASE WHEN 'A' > 'A'
      THEN 1 WHEN 'A' > 'B'
      THEN 2 ELSE 3
    END AS ex
FROM DUAL

 EX
 --
  3
```

#### case example: 성별과 나이 구하기

```sql
SELECT NAME as "사원명", ssn as "주민번호",
  CASE WHEN substr(ssn, 7, 1) IN ('1', '3') THEN '남' ELSE '여' END AS "성별",
  (EXTRACT(YEAR FROM SYSDATE)
  - (to_number(substr(ssn, 1, 2))
  + CASE WHEN substr(ssn, 7, 1) in ('1', '2') then 1900 else 2000 end)) + 1 as "나이"
FROM insa
```

### DISTINCT

중복되는 데이터를 표시하지 않게 하는 연산자.

```
SELECT DISTINCT 컬럼명 FROM 테이블
```

```sql
-- 중복값이 없는 부서번호 출력식
SELECT DISTINCT deptno FROM emp
```

주의: DISTINCT와 ORDER BY를 같이 사용할 땐, ORDER BY에서 사용되는 컬럼을 SELECT문에서 모두 명시해야 한다.

### ROWNUM

현재 레코드셋에서 레코드들의 순서를 매기는 역할, 유령컬럼이라고도 한다. 조건절은 포함한(ORDER BY 제외) 결과셋의 인덱스 역할을 한다.

```sql
SELECT * FROM emp WHERE ROWNUM <= 10;

     EMPNO ENAME      JOB              MGR HIREDATE        SAL       COMM     DEPTNO
---------- ---------- --------- ---------- -------- ---------- ---------- ----------
      7369 SMITH      CLERK           7902 80/12/17        800                    20
      7499 ALLEN      SALESMAN        7698 81/02/20       1600        300         30
      7521 WARD       SALESMAN        7698 81/02/22       1250        500         30
      7566 JONES      MANAGER         7839 81/04/02       2975                    20
...


SELECT ROWNUM AS seq, NAME, ssn
FROM (SELECT NAME, ssn FROM insa WHERE city = '서울' ORDER BY NAME)
WHERE ROWNUM <= 5;
```

#### ROWNUM을 이용한 특정 범위 조회:

```sql
-- 특정 번호부터 특정 번호까지 출력
SELECT *
FROM (
    SELECT ROWNUM as rnum, tb.*
    FROM (
        SELECT num, name, content FROM guest ORDER BY num DESC
    ) tb
)
WHERE rnum >= 3 AND rnum <= 7;

-- 특정 데이터 바로 위의 레코드 출력
SELECT *
FROM (
    SELECT num, name, content, created FROM GUEST WHERE num > # ORDER BY num ASC
)
WHERE ROWNUM=1;

-- 특정 데이터 바로 아래의 레코드 출력
SELECT *
FROM (
    SELECT num, name, content, created FROM GUEST WHERE num < # ORDER BY num DESC
)
WHERE ROWNUM=1;
```

ROWNUM이 생성되는 시점에 주의하지 않으면 의도치 않은 값이 출력될 수 있으니 주의해야 한다.

## WHERE

```
... WHERE 조건
```

### Operator, 연산자

- 산술: `+`, `-`, `*`, `/`
- 비교: `>`, `>=`, `<`, `<=`, `=`, `<>`, `!=`
- 논리: `and`, `or`, `not`
- 결합: `||`
- SQL연산자: `in`, `not in`, `between`, `like`, `is null`, `is not null`, `any`, `all` 등
- 연산자는 경우에 따라 SELECT 절에서 사용할 수 있다.

#### 비교연산

```
... WHERE 대상컬럼 비교연산자 값
```

```sql
-- 아시아에 속하지 않는 국가명과 대륙을 출력
SELECT NAME AS 국가명, cont AS 대륙 FROM country WHERE cont<>'AS';

-- 아시아에 속한 국가명과 대륙 출력
SELECT NAME, cont FROM country WHERE cont='AS';

-- 고용일이 1997년 08월 17일인 직원 이름?
SELECT first_name || ' ' || last_name AS 이름 FROM employees WHERE hire_date='1997/08/17'
```

#### BETWEEN, 범위조건

최소값과 최대값을 지정해 그 사이의 값을 조건으로 사용

```
... WHERE 컬럼명 BETWEEN 최소값 AND 최대값
... WHERE 컬럼명 NOT BETWEEN 최소값 AND 최대값
```

```sql
-- 인구가 5000만~1억 사이의 나라명, 인구 가져오기
SELECT NAME, popu FROM country WHERE popu BETWEEN 5000 AND 10000;
WHERE popu >= 5000 AND popu <= 10000

-- 고용일이 1981년인 사람? (=)
SELECT ename, hiredate FROM emp WHERE hiredate BETWEEN '1981-01-01' AND '1981-12-31'
WHERE hiredate >= '1981-01-01' AND hiredate <= '1981-12-31';
```

#### 논리연산

```
... WHERE 컬럼명 조건식 OR 컬럼명 조건식 ...
... WHERE 컬럼명 조건식 AND 컬럼명 조건식 ...
```

```sql
-- 유럽 혹은 아시아
SELECT NAME, cont FROM country WHERE cont = 'EU' OR cont = 'AS';

-- 유럽 포함 호주
SELECT NAME, cont FROM country WHERE cont = 'EU' AND cont = 'AS';
```

#### IN, 열거형 조건

```
... WHERE 컬럼명 IN ( 열거형 값 리스트 )
... WHERE 컬럼명 NOT IN ( 열거형 값 리스트 )
```

```sql
-- 유럽과 호주대륙
SELECT NAME, cont FROM country WHERE cont IN('EU', 'AU');
```

#### LIKE, 패턴조건

```
... WHERE 컬렴명 LIKE 패턴
... WHERE 컬렴명 NOT LIKE 패턴
```

- `LIKE 'A%'`: A로 시작하는 값을 조회
- `LIKE '%A'`: A로 끝나는 값을 조회
- `LIKE '%A%'`: A를 포함하는 값조회

```sql
-- 데이터가 '국'을 포함
SELECT NAME FROM country WHERE NAME LIKE '%국%';
```

#### NULL 조건

```
... WHERE 컬럼명 IS NULL
... WHERE 컬럼명 IS NOT NULL
```

#### ESCAPE

특정 예약어를 문자 그대로 해석하도록 지정.

```sql
SELECT * FROM TAB_TEST
WHERE NAME LIKE 'A!%' ESCAPE '!'
```
A% 를 검색하려는 경우 `!`(혹은 다른 특수문자)를 escape 문자로 지정한다.

## ORDER BY

특정 컬럼을 기준으로 레코드를 정렬한다. SELECT 이후에 처리되기 때문에 SELECT 에서 부여한 별칭이나 컬럼순서을 의미하는 숫자를 사용할 수 있다.

```
... ORDER BY 컬럼명 [ASC|DESC]
```
정렬기준을 명시하지 않을 경우 ASC(오름차순)이 기본값.

```sql
-- EMP 테이블의 직원을 급여순으로 내림차순 정렬
SELECT empno, ename, sal FROM emp ORDER BY sal DESC;

-- EMP 테이블에서 급여가 1000 이하인 직원을 오름차순 정렬
SELECT empno, ename, sal FROM emp WHERE sal <= 1000 ORDER BY sal;

-- 다중정렬
SELECT * FROM insa ORDER BY buseo , city DESC;

-- 별칭으로 정렬
SELECT first_name || ' ' || last_name AS NAME FROM employees ORDER BY NAME

-- 컬럼 순서로 정렬
SELECT * FROM insa ORDER BY 1 ASC, 3 ASC;
```

## GROUP BY

특정 컬럼값이 동일한 값을 갖는 레코드를 그룹으로 묶는 역할. 같은 그룹끼리의 집계함수를 적용하기 위해 사용한다. (같은 그룹의 통계값 구하기 같은...)
GROUP BY 절에 명시된 컬럼 혹은 집합함수 외에는 SELECT 절에서 사용할 수 없는 점에 주의할 것.

```
... GROUP BY 컬럼명
```

```sql
-- INSA 테이블에서 부서별 최고급여
SELECT buser, MAX(basicpay) FROM insa GROUP BY buseo;

-- INSA 테이블에서 부서별 최고급여를 구하되 부서별 평균급여로 오름차순 정렬
SELECT buseo, MAX(basicpay) FROM insa GROUP BY buseo ORDER BY AVG(basicpay)

-- EMP 테이블에서 부서번호 10, 20 부서의 부서별 평균 급여 구하기
SELECT deptno, AVG(sal) FROM emp WHERE deptno IN(10, 20) GROUP BY deptno;
```

## HAVING

GROUP BY의 조건문 - 그룹이 갖는 집계함수를 조건으로 사용.

```
... HAVING 집합함수 조건 값
```

```sql
-- 부서별 평균월급이 150만원 이상인 부서의 이름 + 평균월급
SELECT buseo, AVG(basicpay) FROM insa GROUP BY buseo
HAVING AVG(basicpay) >= 1500000;

-- employees 테이블에서 부서별 그룹 후 각 그룹별 직원 수, 부서 출력. 단, 그룹별 직원 수가 다섯명 이상
SELECT department_id, COUNT(department_id) AS 직원수 FROM employees
GROUP BY department_id HAVING COUNT(*) >= 5;

-- emp 테이블에서 부서번호 10, 20 부서 중 부서별 평균 급여가 2500 이상 부서의 평균 구하기
SELECT deptno, AVG(sal) FROM emp WHERE deptno IN(10, 20) GROUP BY deptno
HAVING AVG(sal) >= 2500;

-- HAVING은 WHERE와 유사하나 단일컬럼의 조건이 아닌 그룹화 된 컬럼을 조건으로 사용함
-- 직원의 월급이 150만원 이상인 직원을 대상으로 부서명 + 평균 월급 출력
SELECT buseo, ROUND(AVG(basicpay)) AS 평균월급 FROM insa
WHERE basicpay >= 1500000 GROUP BY buseo;
```
