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

#### 관련 문서

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
    when column_type like 'datetime%' then 'java.time.LocalDateTime'
    when column_type like 'date%' then 'java.time.LocalDate'
    when column_type like 'time%' then 'java.time.LocalTime'
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
order by a.txt
```

의 결과는:

| txt |
| :--- |
| 1 |
| 2 |
| 3 |
| a |
| A |
| b |
| ㄱ |
| 가 |
| 히 |
| ㅔ |

### 맴대로 정렬하기

만약 기본 정렬 이외의 요건이 있으면 `case` 등으로 데이터에 따라 임의의 서수를 부여하고 정렬한다.

아래는 '영문 > 한글 > 숫자 순으로 정렬'이라는 요건을 구현한 쿼리다. 첫 글자만 잘라 ASCII 코드로 숫자인지, 영문인지, 한글인지를 구분한다:

```sql
# 영문, 한글, 숫자 순으로 정렬하기
select a.txt,
    convert(a.txt using utf8) as utf8,
    ascii(a.txt) as ascii,
    substr(a.txt, 1, 1) as "first-letter",
    substr(a.txt, 2, 1) as "second-letter",
    ascii(substr(a.txt, 1, 1)) as "ascii-of-first-letter",
    ascii(substr(a.txt, 2, 1)) as "ascii-of-second-letter",
    convert(a.txt, unsigned) as "converted-number",
    case
        when ascii(a.txt) between 48 and 57 then 2 /*number*/
        when ascii(a.txt) between 65 and 90 then 0 /*alphabet capital*/
        when ascii(a.txt) between 97 and 122 then 0 /*alphabet small letter*/
        else 1 /*unicode*/
    end as sortOrder
from (
    select '111반' as txt
    union all
    select '23' as txt
    union all
    select '222' as txt
    union all
    select '24' as txt
    union all
    select '333' as txt
    union all
    select '0000' as txt
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
order by sortOrder, convert(a.txt, unsigned), a.txt
```

실행 결과:

| txt | utf8 | ascii | first-letter | second-letter | ascii-of-first-letter | ascii-of-second-letter | converted-number | sortOrder |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| abcdf | abcdf | 97 | a | b | 97 | 98 | 0 | 0 |
| Aqwe | Aqwe | 65 | A | q | 65 | 113 | 0 | 0 |
| ba뀨sdf | ba뀨sdf | 98 | b | a | 98 | 97 | 0 | 0 |
| Zsadf | Zsadf | 90 | Z | s | 90 | 115 | 0 | 0 |
| zx뿅zcv | zx뿅zcv | 122 | z | x | 122 | 120 | 0 | 0 |
| ㄱ | ㄱ | 227 | ㄱ |  | 227 | 0 | 0 | 1 |
| 가 | 가 | 234 | 가 |  | 234 | 0 | 0 | 1 |
| ㅎ | ㅎ | 227 | ㅎ |  | 227 | 0 | 0 | 1 |
| 히 | 히 | 237 | 히 |  | 237 | 0 | 0 | 1 |
| ㅏ | ㅏ | 227 | ㅏ |  | 227 | 0 | 0 | 1 |
| ㅔ | ㅔ | 227 | ㅔ |  | 227 | 0 | 0 | 1 |
| 0000 | 0000 | 48 | 0 | 0 | 48 | 48 | 0 | 2 |
| 23 | 23 | 50 | 2 | 3 | 50 | 51 | 23 | 2 |
| 24 | 24 | 50 | 2 | 4 | 50 | 52 | 24 | 2 |
| 111반 | 111반 | 49 | 1 | 1 | 49 | 49 | 111 | 2 |
| 222 | 222 | 50 | 2 | 2 | 50 | 50 | 222 | 2 |
| 333 | 333 | 51 | 3 | 3 | 51 | 51 | 333 | 2 |

`convert(a.txt, unsigned)`는 숫자로 이뤄진 문자를 제대로 정렬하지 못하는 문제를 해소하기 위한 정렬 조건이다.


## 테이블 조인 JOIN

[https://mariadb.com/kb/en/join-syntax](https://mariadb.com/kb/en/join-syntax)

### outer join 중 inner join을 먼저 수행하고 싶을 때

가령 t1 테이블과 t2 테이블을 outer join 할 때, 먼저 t2와 t3, t4 테이블의 inner join을 먼저 하고 싶다면?

```sql
# t2, t3, t4를 inner join하고 t1과 outer join
select * from t1 left join (t2, t3, t4)
                 on (t2.a=t1.a and t3.b=t1.b and t4.c=t1.c)

# 위와 같음
select * from t1 left join (t2 cross join t3 cross join t4)
                 on (t2.a=t1.a and t3.b=t1.b and t4.c=t1.c)
```


## WITH

[https://mariadb.com/kb/en/with/](https://mariadb.com/kb/en/with/)

`WITH`는 *Common Table Expression (CTE)*를 나타내는 키워드다. MariaDB 10.2.1에 소개되었다. 

서브쿼리의 일종인데, 쿼리 실행 시간 동안만 존재하는 임시 테이블을 만들어 여러번 참조할 수 있게 하는 기능이라 대충 이해하면 된다.

이렇게 쓴다:

```sql
with v1 as (
    select a, b
    from t1 
    where b >= 'c'
),
v2 as (
    select a, c
    from t2
    where somecondition
)
select * 
from t3
join v1 on v1.a = t3.a
join v2 on v2.a = t3.a
```

CTE는 Non-Recursive와 Recursive 두 종류가 있다. 

Recursive CTE는 `WITH RECURSIVE` 키워드로 표현하고(MariaDB 10.2.2부터 지원) 재귀적 결과 집합을 생성할 때 사용한다.

아래는 특정 날짜부터 오늘까지의 날짜 데이터를 임시 테이블로 생성하는 쿼리다:

```sql
with recursive dates(date) as (
    select '2023-03-11'
    union all
    select date_add(date, interval 1 day)
    from dates
    where date < curdate()
    # where date < last_day('2023-05-01') # 2023년 5월의 마지막 날까지
)
select * from dates
```


## DELETE와 TRUNCATE의 차이

- `TRUNCATE`는 `WHERE` 절을 사용할 수 없다.
- `TRUNCATE`는 `DELETE`보다 빠르게 작동한다. `TRUNCATE`는 테이블 전체를 잠그고 데이터를 삭제하는 반면, `DELETE`는 각 행을 스캔하여 삭제하기 때문
- `TRUNCATE`는 삭제된 행의 수를 반환하지 않는다. 또한 테이블의 auto-increment 값도 초기값으로 재설정된다.


## LAST_INSERT_ID()

가장 최근에 실행된 `INSERT` 문의 결과에서 `AUTO_INCREMENT` 속성 컬럼으로 할당된 자동 생성 값을 반환한다. 동시성 문제가 있어보이는데, 테스트 해보니 세션 혹은 트랜잭션 기준으로 반환하는 걸로 추정된다.

```sql
select last_insert_id()
```
