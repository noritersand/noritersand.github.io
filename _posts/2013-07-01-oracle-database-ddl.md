---
layout: post
date: 2013-07-01 02:00:00 +0900
title: '[Oracle Database] DDL'
categories:
  - dbms
tags:
  - dbms
  - oracle
  - ddl
---

* Kramdown table of contents
{:toc .toc}

## CREATE

```
CREATE TABLE 테이블명 (
    컬럼명1 자료형(크기),
    컬럼명2 자료형(크기) default 기본값,
    ...
)
```

### CONSTRAINT

테이블 레벨에서 제약조건 설정

```
CREATE 테이블명 (
    컬럼명1 자료형(크기),
    컬럼명2 자료형(크기),
    CONSTRAINT 제약조건이름 제약조건( 컬럼명 [, 컬럼명 ]),
    ...
)
```

```sql
CREATE TABLE test2 (
    num NUMBER,
    NAME VARCHAR2(30) not null,
    id VARCHAR2(20),
    CONSTRAINT pk_test2 PRIMARY KEY(num),
    CONSTRAINT fk_test2 FOREIGN KEY(num) REFERENCES test1(num) [ON DELETE CASCADE],
    CONSTRAINT uk_test2 UNIQUE(ID)
)
```

#### 제약조건의 종류

- `primary key(PK)`: 테이블에 반드시 존재, not null(NN), unique(ND)
- `foreign key(FK)`: 부모테이블의 PK값을 참조해 사용가능, null가능
- `not null`: 컬럼값 필수
- `unique key(UK)`: 유일키, 테이블내에서 중복값을 포함할 수 없다. 단, null 가능
- `check(CK)`: 열거형 or 범위값, 제시된 형태의 값이 아니면 넣을 수 없다. (유효성검사)

#### 컬럼 레벨에서 제약조건 설정

```sql
CREATE TABLE test1 (
  num NUMBER PRIMARY KEY, /* 기본키 */
  id VARCHAR2(30) not null unique
)

CREATE TABLE test2 (
  num NUMBER REFERENCES test1(num), /* 참조키 */
  address VARCHAR2(100) not null
)
```

#### 제약조건 조회

```sql
SELECT
    constraint_name, table_name, r_constraint_name, constraint_type, search_condition
FROM user_constraints
```

#### ON DELETE CASCADE

레코드를 삭제할 때 해당 레코드를 참조하는 다른 테이블의 레코드가 있다면 같이 삭제한다.

#### CHECK, CK

데이터 추가 시 유효성검사를 실행하는 제약조건

```sql
CREATE TABLE test5 (
    city VARCHAR2(2) NOT NULL,
    CONSTRAINT test5_city_ck CHECK (city IN('서울', '인천', '부산')),  --열거형
    age NUMBER NOT NULL,
    CONSTRAINT test5_age_ck CHECK (age between 1 AND 99)    --범위형
)
```

### SEQUENCE

지정된 조건의 서수를 생성한다.

```sql
CREATE SEQUENCE GUEST_SEQ
INCREMENT BY 1        -- 증가값
START WITH 1          -- 초깃값
NOMAXVALUE            -- 최댓값을 지정한다.
NOCYCLE               -- 최댓값 이후 초깃값으로 돌아갈지 여부
NOCACHE               -- 캐시 사용 안함
```

```sql
CREATE TABLE Test2 (
    seq number NOT NULL PRIMARY KEY,
    … …
);

CREATE SEQUENCE Test2_seq;

INSERT INTO Test2 (seq, name, age)
VALUES (Test2_seq.nextval, '홍길동', 10);
```

### DEFAULT

기본값 지정. 데이터 생성 시 아무런 값도 입력되지 않으면 default로 지정한 값이 사용된다.

```sql
CREATE TABLE test (
    regDate DATE DEFAULT SYSDATE,
    num NUMBER DEFAULT 100,
    str VARCHAR(50) DEFAULT '기본값'
);

CREATE TABLE test10 (
    seq NUMBER PRIMARY KEY,
    num NUMBER DEFAULT 1,
    city NVARCHAR2(2) DEFAULT '대전',
    CONSTRAINT test10_num_ck CHECK(num BETWEEN 1 AND 10),
    CONSTRAINT test10_city_ck CHECK(city IN('서울', '부산', '인천'))
);
```

### AS

as는 위치에 따라 의미가 다르다.

#### CREATE 테이블 다음에 오는 AS

동일한 테이블을 생성(복제)한다.

```sql
CREATE TABLE test12
AS
select seq, data from test11 where 1 = 0
```

#### 컬럼 선언부에서 컬럼명 다음에 오는 AS

default 값을 설정한다.

```sql
CREATE TABLE tbl_auction (
  ...,
  startday DATE DEFAULT SYSDATE,
  endday AS (startday + 3)
  -- startday 컬럼의 데이터타입을 따라가며 default로 startday + 3의 값을 자동으로 갖는다.
)
```

단, default 값 설정은 11g 에서만 가능하며 10g 이하에서는 다음처럼 작성한다.

```sql
..., endday DATE DEFAULT (SYSDATE + 3) )
```

### INDEX

```sql
CREATE INDEX index_name ON table_name (column_name);
CREATE INDEX index_name ON table_name (name1, name2, ...); -- 복합 인덱스
```

### VIEW

DB상의 하나 이상의 테이블에서 원하는 값들을 모아서 접근을 편하게 하기 위한 일종의 가상 테이블. 복사된 테이블(create as)과 유사해 보이지만 '동일한 객체를 참조(주소값만 저장)'하는 테이블이다. 반복되는 select문의 구문을 줄이기 위한 수단이며 읽기 전용이다.

수직 뷰 vertical view:

```sql
CREATE OR REPLACE VIEW vwVerticalInsa
AS
SELECT name, basicpay, buseo
FROM insa
```

수평 뷰 horizontal view:

```sql
CREATE OR REPLACE VIEW vwHorizontalInsa
AS
SELECT *
FROM insa
WHERE buseo = '영업부'
```

계정에 뷰 생성 권한이 없을 경우:

```sql
console> sqlplus sys as sysdba
SQL> grant create view to scott;
```

## ALTER

### ADD

컬럼 추가.

```
ALTER TABLE 테이블 ADD (컬럼명1 자료형 [제약조건], 컬럼명2 자료형...);

혹은

ALTER TABLE 테이블 ADD 컬럼명 자료형 [제약조건]; --추가할 컬럼이 하나일 땐 괄호는 생략할 수 있다.
```

```sql
ALTER TABLE test11 ADD firstName VARCHAR2(10) null, lastName VARCHAR2(10) null;
ALTER TABLE test11 ADD age number(3) default 10 not null;
--alter ~ add 에서 not null은 해당 테이블이 비어있거나 default 값 할당 시에만 가능함
```

### MODIFY

컬럼 구조 혹은 제약조건 수정.

```sql
-- 컬럼 속성 수정
ALTER TABLE 테이블명 MODIFY (컬럼명1 자료형(크기), 컬럼명2 자료형(크기));
ALTER TABLE 테이블명 MODIFY 컬럼명 자료형(크기);

-- 제약조건 수정
ALTER TABLE 테이블명 MODIFY (컬럼명 CONSTRAINT 제약조건명 제약조건(컬럼명 조건식));
ALTER TABLE 테이블명 MODIFY 컬럼명 CONSTRAINT 제약조건명 제약조건(컬럼명 조건식);

-- 컬럼 속성 수정
ALTER TABLE test11 MODIFY data VARCHAR2(9);
ALTER TABLE memo MODIFY when VARCHAR2(30) SYSDATE;
ALTER TABLE employees MODIFY first_name VARCHAR2(50);
--이미 데이터가 입력되어 있는 경우엔 해당 데이터와 수정하려는 컬럼의 구조가 적합한지를 주의해야한다.

-- 제약조건 수정
ALTER TABLE test MODIFY num CONSTRAINT ck2_test1 CHECK(num BETWEEN 1 AND 5);
-- 단 여기서 제약조건명은 이전과 다르게 변경되어야 함
```

### RENAME TO

식별자수정.

```
ALTER TABLE 테이블명 RENAME COLUMN 컬럼명1 TO 컬럼명2 -- 컬럼명 수정
RENAME 테이블명1 TO 테이블명2;     -- 테이블명 수정
```

```sql
ALTER TABLE tbl_test RENAME COLUMN data TO name;
RENAME tbl_test1 TO tbl_test2;
```

## DROP


```
ALTER TABLE 테이블명 DROP COLUMN 컬럼명;        -- 컬럼 삭제
DROP TABLE 테이블명;        -- 테이블 삭제
VEIW 뷰이름;                    -- 뷰 삭제
SEQUENCE 시퀀스명;          -- 시퀀스 삭제
TRUNCATE TABLE 테이블명;  -- 테이블의 모든 레코드 삭제(테이블의 구조는 삭제되지 않음) 자동 commit, roll back 불가능. 주의
```

제약조건이 있는 컬럼 제거하기:

```sql
ALTER TABLE 테이블명 DROP PRIMARY KEY;        -- 기본키 제거
ALTER TABLE 테이블명 DROP CONSTRAINT 제약조건이름;

ALTER TABLE 테이블명 MODIFY 컬러명 NULL;        -- NOT NULL 제거
ALTER TABLE 테이블명 DROP CONSTRAINT 제약조건이름;

ALTER TABLE 테이블명 DROP CONSTRAINT 제약조건이름;        -- 기타 제약조건 제거
```

휴지통:

```sql
SELECT object_name, original_name, droptime FROM recyclebin;         --휴지통 확인

FLASHBACK TABLE 지운테이블명 TO BEFORE DROP;         --휴지통 복원
FLASHBACK TABLE "지운테이블_objectName" TO BEFORE DROP;

SELECT * FROM "지운테이블_objectName";         --휴지통 테이블내용확인

PURGE RECYCLEBIN;        --휴지통 비우기
```
