---
layout: post
date: 2013-07-01 03:00:00 +0900
title: 'Oracle: DML'
categories:
  - dbms
  - oracle
tags:
  - oracle
  - dml
---

## INSERT

테이블에 레코드(행)을 추가하는 명령어.

```
INSERT INTO 테이블명 (컬럼1, 컬럼2...) VALUES (값1, 값2...)
```

작성 규칙:
- 컬럼리스트는 테이블의 모든 컬럼을 기재할 필요 없다.
- 컬럼리스트는 순서가 바뀌어도 되지만 이 경우엔 값도 같은 순서가 되어야 한다.
- null값이 허용되어 있는 컬럼은 기재를 생략해도 된다. (null 입력 가능)
- default 설정이 되어있는 컬럼은 insert에서 생략을 해도 되고 명시적으로 values(default) 사용해도 된다.

```sql
INSERT INTO memo (seq, memo, category, regDate, ck, rname, rmemo)
VALUES (memo_seq.nextval, '메모입니다.', 'A', sysdate, 'n', '하하하', '꼭확인바람')
```

컬럼명을 생략할 수도 있으나 이 경우엔 값리스트의 순서와 개수는 원본 테이블의 구조와 일치해야 한다:

```sql
INSERT INTO memo
VALUES (memo_seq.nextval, '메모메모', default, default, default, null, null)
```

## INSERT INTO SELECT
INSERT INTO SELECT문은 데이터를 다른 테이블에서 참조한다. INSERT문과 달리 한 번에 여러개의 데이터가 생성될 수 있다. (생성되는 데이터의 수는 SELECT문에 따라 달라짐)

```
INSERT INTO table2 SELECT * FROM table1
INSERT INTO table2 (column_name) SELECT column_name FROM table1;
```

```sql
INSERT INTO Customers (
    CustomerName,
    Country
)
SELECT SupplierName,
    Country
FROM Suppliers
```

[SELECT INTO](https://www.w3schools.com/sql/sql_select_into.asp)와 다르다.

## UPDATE

테이블의 모든 레코드 혹은 일부 레코드의 특정 컬럼값(1개 이상)을 수정하는 명령어

```
UPDATE 테이블 SET 컬럼 = 수정값, 컬럼명 = 수정값... [where]
```

```sql
UPDATE country SET capital = '인천';

UPDATE country
SET capital = '인천', cont = 'kr', area = 999
WHERE capital = '서울';

UPDATE tbl_test
SET cnt = cnt+1
WHERE id = 'guest';
-- where절이 없는 update는 모든 레코드를 수정함
```

## DELETE

특정 레코드를 삭제하는 명령어. where절이 생략되면 해당 테이블의 모든 레코드를 삭제한다.

```
DELETE FROM 테이블명 [where]
```

```sql
DELETE FROM rent WHERE rentDate <= '2013-01-31'
```
