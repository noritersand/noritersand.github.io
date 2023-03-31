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


## 문자열 연산

무적권 `CONCAT()`을 써야함:

```sql
select '2021-01-01' + ' 23:59:59'; -- 2044
select '2021-01-01' || ' 23:59:59'; -- 1
select concat('2021-01-01', ' 23:59:59'); -- 2021-01-01 23:59:59
```

연산자로도 되게 해줭... 😒


## 백틱의 의미

MariaDB에서 ``` ` ```은 [Quote Identifier](https://mariadb.com/kb/en/identifier-names/)라고 하며 테이블이나 컬럼명을 명시할 때 사용한다. 대부분의 경우 생략해도 결과는 같다.

그러나 간혹 테이블 혹은 컬럼, 별칭의 이름이 문법 에러를 발생시키는 경우가 있는데:

```sql
-- 별칭에 . 이 포함된 경우
select v.dummy.number from (select 1 as 'dummy.number') v;

-- 컬럼 이름과 테이블 이름이 예약된 키워드인 경우
select column from table;
```

이럴 때 백틱으로 단어를 감싸면 해결됨:

```sql
select v.`dummy.number` from (select 1 as 'dummy.number') v;

select `column` from `table`;
```


## 메타 데이터 조회

- Information schema: 오라클에서 Data dictionaries 쯤 되는 것

```sql
# 테이블 목록
select * from information_schema.tables;

# 컬럼 목록
select * from information_schema.columns;

# 사용 가능한 엔진 목록... 인가?
select * from information_schema.engines;
```


## 불리언 타입 Boolean

사실 MariaDB에는 불리언 타입이 따로 존재하지 않는다. `bool` 혹은 `boolean` 키워드는 `tinyint(1)`의 별칭(synonym)이다. `true`와 `false` 키워드도 각각 0과 1의 별칭이다.

```sql
create table test_table4 (
    num boolean
);

select data_type, column_type
from information_schema.columns where table_name = 'test_table4';

+---------+-----------+
|DATA_TYPE|COLUMN_TYPE|
+---------+-----------+
|tinyint  |tinyint(1) |
+---------+-----------+
```

그리고 1 혹은 0의 동등비교는 마치 자바스크립트에서 불리언 타입을 다루는 것처럼 생략이 가능하다.

```sql
# num이 0이 아니면 'ture', 0이면 'false' 출력
select if(num, 'true', 'false')
from test_table4
```

```sql
select *
from test_table3
where num1 # num1 != 0과 같음
and not num2 # num2 = 0과 같음
```

\* 주의: 불리언 타입이 실제로 존재하는 게 아니라는 것을 기억해야 한다. 코멘트에도 써놨지만 조건식에서 `= 1`의 생략은 해당 컬럼이 **0이 아닌** 데이터를 찾으라는 뜻이다. 만약 실제 값이 2 혹은 65536이어도 true로 간주된다.


## 로컬 서버 설정

### root로 접속

설치 경로에서 `mariadb.exe -u root -p` 실행:

```bash
PS C:\Program Files\MariaDB 10.7\bin> .\mariadb.exe -u root -p
```

혹은 같이 설치된 MySQL Client 실행.

### 데이터베이스 생성/보기/선택

```bash
> status # 현재 상태 보기
```

```sql
create database maria_db_test default character set utf8;

# create schema는 create database의 별칭이라서 결과는 위와 같음.
create schema maria_db_test default character set utf8;
```

```sql
-- 스키마(database) 조회
show databases;

-- 스키마(database) 상세 조회
select * from information_schema.schemata;

-- 현재 데이터베이스의 모든 테이블 보기
show tables;
```

```sql
-- 'maria_db_test' 데이터베이스 사용
use maria_db_test
```

### 로컬 접속용 유저 생성과 모든 권한 부여

권한 관련 도움말은 [여기](https://mariadb.com/kb/en/grant)를 보자.

```sql
create user 'fixalot'@'localhost' identified by '1123';
grant all privileges on *.* to 'fixalot'@'localhost';
flush privileges;
```

다른 유저의 권한을 참고하고 싶을 땐 `SHOW GRANTS`로 조회되는 내용을 그대로 사용하는게 편하다.

```sql
-- 모든 사용자 조회(권한 필요)
select * from mysql.user;

-- 부여 가능한 privilege 목록
show privileges;

-- 현재 접속한 user의 부여된 권한 보기
show grants;

-- fixalot@localhost에게 부여된 권한 보기
show grants for fixalot@localhost;
```


## 로우 넘버

ROWNUM 출력 방법

```sql
select @rownum := @rownum + 1 as rownum
from test_table, (select @rownum := 0) r
```


## Entity 클래스 만들기

```sql
/* 테이블 + 컬럼 코멘트 */
set @table_name = 'table_name';
set @table_schema = 'schema_name';

select 0 as ordinal_position, 'it\'s table' as column_name, '' as column_type, concat('/**', table_name, ' ', table_comment, ' 테이블 클래스', '*/\r\r') as str
from information_schema.tables
where table_name = @table_name
and table_schema = @table_schema
union
select ordinal_position, column_name, column_type, concat('\t/**', column_comment, '*/\r\tprivate ',
  case
    when column_type like 'int%' then 'Integer'
    when column_type like 'varchar%' then 'String'
    when column_type like 'date%' then 'java.time.LocalDate'
    when column_type like 'time%' then 'java.time.LocalTime'
    when column_type like 'datetime%' then 'java.time.LocalDateTime'
    when column_type like 'tinyint%' then 'Integer'
    when column_type like 'smallint%' then 'Integer'
    when column_type like 'mediumint%' then 'Integer'
    when column_type like 'bigint%' then 'Long'
    when column_type like 'float%' then 'Float'
    when column_type like 'double%' then 'Double'
    when column_type like 'decimal%' then 'BigDecimal'
    when column_type like 'text%' then 'String'
    when column_type like 'blob%' then 'String'
    when column_type like 'binary%' then 'String'
    when column_type like 'char%' then 'String'
    when column_type like 'enum%' then 'String'
    when column_type like 'set%' then 'String'
    when column_type like 'bool%' then 'Boolean'
    when column_type like 'boolean%' then 'Boolean'
    when column_type like 'tinyblob%' then 'String'
    when column_type like 'tinytext%' then 'String'
    when column_type like 'mediumblob%' then 'String'
    when column_type like 'mediumtext%' then 'String'
    when column_type like 'longblob%' then 'String'
    when column_type like 'longtext%' then 'String'
  end
  , ' ', column_name, ';') as str
from information_schema.columns
where table_name = @table_name
and table_schema = @table_schema
order by ordinal_position asc
```

`set` 까지 한 번에 실행하면 된다. `str` 컬럼을 Java 파일에 붙여놓고 (필요한 경우) 카멜케이스 변환만 해주면 끗.

DBMS 툴에 따라 쌍따옴표가 붙을 수 있는데 그냥 전부 지우면 됨.


## data concatenation

[https://www.mariadbtutorial.com/mariadb-aggregate-functions/mariadb-group_concat/](https://www.mariadbtutorial.com/mariadb-aggregate-functions/mariadb-group_concat/)

한 건 이상의 데이터를 하나의 문자열로 연결해 표현하는 방법을 말함. `GROUP_CONCAT()` 함수를 쓴다.

기본 사용법:

```sql
select group_concat(member_name)
from some_member_table
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


## 정렬 order by

오름차순(ascendent) 기준으로 우선순위는 다음과 같다:

- 작은 수
- 큰 수
- 알파벳 빠른 순서의 소문자
- 알파벳 빠른 순서의 대문자
- 자음
- 조합된 한글의 가나다 순
- 모음

예를 들면:

```sql
select a.txt
from (
    select 1 as txt
    union all
    select 2 as txt
    union all
    select 3 as txt
    union all
    select 'a' as txt
    union all
    select 'b' as txt
    union all
    select 'A' as txt
    union all
    select '가' as txt
    union all
    select 'ㅔ' as txt
    union all
    select 'ㄱ' as txt
    union all
    select '히' as txt
) a
order by a.txt asc
```

의 결과는:

+---+
|txt|
+---+
|1  |
|2  |
|3  |
|a  |
|A  |
|b  |
|ㄱ  |
|가  |
|히  |
|ㅔ  |
+---+

### 맴대로 정렬하기

만약 기본 정렬 이외의 요건이 있으면 `case` 등으로 데이터에 따라 임의의 서수를 부여하고 정렬한다.

아래는 '영문 > 한글 > 숫자 순으로 정렬'이라는 요건을 구현한 쿼리다. 첫 글자만 잘라 ASCII 코드로 숫자인지, 영문인지, 한글인지를 구분한다:

```sql
# 영문, 한글, 숫자 순으로 정렬하기
select a.txt,
    convert(a.txt using utf8) as utf8,
    substr(a.txt, 1, 1) as substr,
    ascii(a.txt) as ascii,
    case
        when ascii(a.txt) between 48 and 57 then 2 /*number*/
        when ascii(a.txt) between 65 and 90 then 0 /*alphabet*/
        when ascii(a.txt) between 97 and 122 then 0 /*alphabet*/
        else 1 /*unicode*/
    end as sortOrder
from (
    select 111 as txt
    union all
    select 222 as txt
    union all
    select 333 as txt
    union all
    select 0000 as txt
    union all
    select 'abcdf' as txt
    union all
    select 'ba뀨sdf' as txt
    union all
    select 'Aqwe' as txt
    union all
    select 'zx뿅zcv' as txt
    union all
    select 'Zsadf' as txt
    union all
    select 'ㅏ' as txt
    union all
    select '가' as txt
    union all
    select 'ㅔ' as txt
    union all
    select 'ㄱ' as txt
    union all
    select '히' as txt
    union all
    select 'ㅎ' as txt
) a
order by sortOrder
```

## 타입 변환

### convert()

[https://mariadb.com/kb/en/convert/](https://mariadb.com/kb/en/convert/)

```
CONVERT(expr, type)
CONVERT(expr USING transcoding_name)
```

`expr`을 `type`으로 변환한다. 혹은 `expr`을 기본 케릭터 셋에서 `transcoding_name` 케릭터 셋으로 변환한다.

`type`에 올수 있는 키워드는 다음과 같다:

- binary
- char
- date
- datetime
- decimal[(M[,D])]
- double
- float
- integer
- signed [INTEGER]
- unsigned [INTEGER]
- time
- varchar

```sql
select convert(now(), date);
```


## 테이블 조인 join

[https://mariadb.com/kb/en/join-syntax](https://mariadb.com/kb/en/join-syntax)

### outer join 중 inner join을 먼저 수행하고 싶을 때

가령 t1 테이블과 t2 테이블을 outer join 할 때, 먼저 t2와 t3, t4 테이블의 inner join을 먼저 하고 싶다면?

```sql
# t2, t3, t4를 inner join하고 t1과 outer join
SELECT * FROM t1 LEFT JOIN (t2, t3, t4)
                 ON (t2.a=t1.a AND t3.b=t1.b AND t4.c=t1.c)

# 위와 같음
SELECT * FROM t1 LEFT JOIN (t2 CROSS JOIN t3 CROSS JOIN t4)
                 ON (t2.a=t1.a AND t3.b=t1.b AND t4.c=t1.c)
```

