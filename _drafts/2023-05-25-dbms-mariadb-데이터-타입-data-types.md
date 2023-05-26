---
layout: post
date: 2023-05-25 12:00:16 +0900
title: '[DBMS] MariaDB 데이터 타입'
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

- [somewhere](somewhere)

#### 버전 정보

- x.x.x


## 개요

MariaDB의 데이터 타입 관련 모음. [날짜와 시간을 다루는 타입은 따로 작성함](/dbms/dbms-mariadb-date-날짜와-시간/).


## 불리언 타입 Boolean

[https://mariadb.com/kb/en/boolean/](https://mariadb.com/kb/en/boolean/)

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


## JSON 타입

[https://mariadb.com/kb/en/json-data-type/](https://mariadb.com/kb/en/json-data-type/)

TODO


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
