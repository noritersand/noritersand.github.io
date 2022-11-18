---
layout: post
date: 2022-01-07 18:28:23 +0900
title: '[DBMS] MariaDB 노트'
categories:
  - dbms
tags:
  - dbms
  - mariadb
  - sql
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://mariadb.com/kb/en/](https://mariadb.com/kb/en/)
- [https://dbschema.com/documentation/MariaDb/#introduction](https://dbschema.com/documentation/MariaDb/#introduction)


## 개요

MariaDB 관련 내용 아무거나 적음.


## 백틱의 의미

MariaDB에서 ``` ` ```은 [Quote Identifier](https://mariadb.com/kb/en/identifier-names/)라고 하며 테이블이나 컬럼명을 명시할 때 사용한다. 대부분의 경우 생략해도 결과는 같다.

그러나 간혹 테이블 혹은 컬럼, 별칭의 이름이 문법 에러를 발생시키는 경우가 있는데:

```sql
-- 별칭에 . 이 포함된 경우
SELECT V.DUMMY.NUMBER FROM (SELECT 1 AS 'DUMMY.NUMBER') V;

-- 컬럼 이름과 테이블 이름이 예약된 키워드인 경우
SELECT COLUMN FROM TABLE;
```

이럴 때 백틱으로 단어를 감싸면 해결됨:

```sql
SELECT V.`DUMMY.NUMBER` FROM (SELECT 1 AS 'DUMMY.NUMBER') V;

SELECT `COLUMN` FROM `TABLE`;
```


## 로컬 서버 설정

### root로 접속

설치 경로에서 `mariadb.exe -u root -p` 실행:

```bash
PS C:\Program Files\MariaDB 10.7\bin> .\mariadb.exe -u root -p
```

혹은 같이 설치된 MySQL Client 실행.

### 데이터베이스 생성/보기/선택

```sql
-- 현재 상태 보기
status

CREATE DATABASE maria_db_test;
```

```sql
-- 데이터베이스 조회
SHOW DATABASES;

-- 현재 데이터베이스의 모든 테이블 보기
SHOW TABLES;
```

```sql
-- 'maria_db_test' 데이터베이스 사용
use maria_db_test
```

### 로컬 접속용 유저 생성과 모든 권한 부여

```sql
CREATE USER 'fixalot'@'localhost' IDENTIFIED BY '1123';
GRANT ALL PRIVILEGES ON *.* TO 'fixalot'@'localhost';
FLUSH PRIVILEGES;
```

```sql
-- 부여 가능한 권한 보기
SHOW PRIVILEGES

-- fixalot@localhost의 부여된 권한 보기
SHOW GRANTS FOR fixalot@localhost;
```


## 로우 넘버

ROWNUM 출력 방법

```sql
SELECT @ROWNUM := @ROWNUM + 1 AS ROWNUM
FROM SAMPLE, (SELECT @ROWNUM := 0) R
```


## VO(DTO, Entity) 만들기

```sql
/* 테이블 + 컬럼 코멘트 */
SET @TABLE_NAME = 'TABLE_NAME';
SELECT CONCAT('/**', TABLE_NAME, ' ', TABLE_COMMENT, ' 테이블 VO', '*/\r\r') AS STR
FROM INFORMATION_SCHEMA.TABLES
WHERE TABLE_NAME = @TABLE_NAME
UNION
SELECT CONCAT('\t/**', COLUMN_COMMENT, '*/\r\tprivate ',
  CASE
    WHEN COLUMN_TYPE LIKE 'int%' THEN 'Integer'
    WHEN COLUMN_TYPE LIKE 'varchar%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'date%' THEN 'Date'
    WHEN COLUMN_TYPE LIKE 'time%' THEN 'Timestamp'
    WHEN COLUMN_TYPE LIKE 'datetime%' THEN 'DateTime'
    WHEN COLUMN_TYPE LIKE 'tinyint%' THEN 'Integer'
    WHEN COLUMN_TYPE LIKE 'smallint%' THEN 'Integer'
    WHEN COLUMN_TYPE LIKE 'mediumint%' THEN 'Integer'
    WHEN COLUMN_TYPE LIKE 'bigint%' THEN 'Long'
    WHEN COLUMN_TYPE LIKE 'float%' THEN 'Float'
    WHEN COLUMN_TYPE LIKE 'double%' THEN 'Double'
    WHEN COLUMN_TYPE LIKE 'decimal%' THEN 'BigDecimal'
    WHEN COLUMN_TYPE LIKE 'text%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'blob%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'binary%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'char%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'enum%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'set%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'bool%' THEN 'Boolean'
    WHEN COLUMN_TYPE LIKE 'boolean%' THEN 'Boolean'
    WHEN COLUMN_TYPE LIKE 'tinyblob%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'tinytext%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'mediumblob%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'mediumtext%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'longblob%' THEN 'String'
    WHEN COLUMN_TYPE LIKE 'longtext%' THEN 'String'
  END
  , ' ', COLUMN_NAME, ';') AS STR
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = @TABLE_NAME
```

`SET` 까지 한 번에 실행하면 된다. Java 파일에 붙여놓고 카멜케이스 변환만 해주면 끟.


## data concatenation

[https://www.mariadbtutorial.com/mariadb-aggregate-functions/mariadb-group_concat/](https://www.mariadbtutorial.com/mariadb-aggregate-functions/mariadb-group_concat/)

한 건 이상의 데이터를 하나의 문자열로 연결해 표현하는 방법을 말함. `GROUP_CONCAT()` 함수를 쓴다.

기본 사용법:

```sql
SELECT GROUP_CONCAT(MEMBER_NAME)
FROM SOME_MEMBER_TABLE
```

응용하면 1:N 관계의 데이터를 하나의 로우로 이어붙이는 게 가능한데, [여기에](https://www.mariadbtutorial.com/mariadb-aggregate-functions/mariadb-group_concat/) 잘 설명돼있음.
