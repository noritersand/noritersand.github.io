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


## 메타 데이터 조회

- Information schema: 오라클에서 Data dictionaries 쯤 되는 것

```sql
# 테이블 목록
SELECT * FROM INFORMATION_SCHEMA.TABLES;

# 컬럼 목록
SELECT * FROM INFORMATION_SCHEMA.COLUMNS;

# 사용 가능한 엔진 목록... 인가?
SELECT * FROM INFORMATION_SCHEMA.ENGINES;
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

CREATE DATABASE MARIA_DB_TEST DEFAULT CHARACTER SET UTF8;

# CREATE SCHEMA는 CREATE DATABASE의 별칭이라서 결과는 위와 같음.
CREATE SCHEMA MARIA_DB_TEST DEFAULT CHARACTER SET UTF8;

```

```sql
-- 스키마(database) 조회
SHOW DATABASES;

-- 스키마(database) 상세 조회
SELECT * FROM INFORMATION_SCHEMA.SCHEMATA;

-- 현재 데이터베이스의 모든 테이블 보기
SHOW TABLES;
```

```sql
-- 'MARIA_DB_TEST' 데이터베이스 사용
USE MARIA_DB_TEST
```

### 로컬 접속용 유저 생성과 모든 권한 부여

권한 관련 도움말은 [여기](https://mariadb.com/kb/en/grant)를 보자.

```sql
CREATE USER 'fixalot'@'localhost' IDENTIFIED BY '1123';
GRANT ALL PRIVILEGES ON *.* TO 'fixalot'@'localhost';
FLUSH PRIVILEGES;
```

다른 유저의 권한을 참고하고 싶을 땐 `SHOW GRANTS`로 조회되는 내용을 그대로 사용하는게 편하다.

```sql
-- 모든 사용자 조회(권한 필요)
SELECT * FROM mysql.user;

-- 부여 가능한 PRIVILEGE 목록
SHOW PRIVILEGES;

-- 현재 접속한 USER의 부여된 권한 보기
SHOW GRANTS;

-- fixalot@localhost에게 부여된 권한 보기
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


## 덤프로 데이터베이스 내보내기-가져오기

- [참고한 문서](https://www.digitalocean.com/community/tutorials/how-to-import-and-export-databases-in-mysql-or-mariadb)

설치된 컴퓨터 터미널에서:

```bash
# export
mysqldump --user USERNAME --password --databases DATABASE_NAME --result-file=data-dump.sql

# import
mysql --user USERNAME --password --database=NEW_DATABASE < data-dump.sql
```

요런식으로 하면 되는데, 만약 원격지에서 실행한다면 아래처럼 `--host` 옵션으로 서버 주소를 넣어주면 됨:

```bash
mysqldump --host=DOMAIN_OR_IP_ADRESS --user USERNAME --password --database=DATABASE_NAME --result-file=dump.sql
```

데이터베이스를 지정하는 옵션과 표현식이 다르니 주의할 것: `mysql`은 `--database=DB_NAME, -D DB_NAME`, `mysqldump`는 `--databases, -B`.

그리고 `mysql`은 `--routines`, `--result-file` 옵션이 없다.

### mysqldump

`mysqldump`는 [MySQL CLI Client](https://dev.mysql.com/doc/refman/8.0/en/mysqldump.html)에 포함돼 있다.

윈도우인 경우 CLI Client는 따로 없고, MariaDB 서버를 설치해야 함. (귀찮으니 가급적 choco로 하자)

```bash
# 우분투 + 마리아DB
apt install mariadb-client
mysql --version
```

#### options

- `-h HOST_NAME` `--host=HOST_NAME`: 데이터베이스 서버의 도메인이나 IP
- `-P PORT_NUM` `--port=PORT_NUM`: 데이터베이스 서버의 포트 번호
- `-u USER_NAME` ` --user=USER_NAME`: 데이터베이스 로그인에 사용한 사용자명
- `-p[PASSWORD]` `--password[=PASSWORD]`: 명령을 실행하기 전에 비밀번호 입력을 먼저 받음
- `-B` `--databases`: `mysqldump`는 첫 번째 인수를 데이터베이스(=스키마) 이름으로, 두 번째 인수를 테이블 이름으로 처리하지만, 이 옵션이 있으면 모든 인수를 데이터베이스 이름으로 처리한다.
- `-R` `--routines`: 루틴(=함수와 프로시저)을 포함한다.
- `-r FILE_NAME` `--result-file=FILE_NAME`: 덤프 결과를 내보낼 파일의 이름. 생략하면 콘솔에 출력함.

`--host`를 생략하면 데이터베이스 인스턴스를 로컬 컴퓨터에서 찾는다.

### 권한 문제로 함수나 프로시저 생성에 실패할 때

dump 파일에서 `DEFINER`와 뒤따르는 문자들을 삭제하면 됨:

```bash
# dump.sql 파일에서 DEFINER를 삭제하면서 백업 파일 dump.sql.bak를 만듬
sed 's/\sDEFINER=`[^`]*`@`[^`]*`//g' --in-place=.bak dump.sql
```

[출처](https://stackoverflow.com/questions/44015692/access-denied-you-need-at-least-one-of-the-super-privileges-for-this-operat)

