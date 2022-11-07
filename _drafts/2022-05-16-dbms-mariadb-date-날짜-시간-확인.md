---
layout: post
date: 2022-05-16 18:06:38 +0900
title: '[DBMS] MariaDB 날짜/시간 확인'
categories:
  - dbms
tags:
  - dbms
  - mariadb
  - sql
  - code-snippet
  - date
  - datetime
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MariaDB\] STR_TO_DATE](https://mariadb.com/kb/en/str_to_date/)
- [\[MariaDB\] DATE_FORMAT](https://mariadb.com/kb/en/date_format/)
- [\[MariaDB\] DATETIME](https://mariadb.com/kb/en/datetime/)

#### 버전 정보

- MariaDB 10.x


## 개요

`DATE`, `DATETIME`, `TIME` 관련 정리.


## 쿼리

```sql
SELECT
    DD.DUMMY AS DATE_DEFAULT,
    DATE_FORMAT(DD.DUMMY, '%Y-%m-%d %H:%i:%s') AS ISO_STRING,
    DATE_FORMAT(DD.DUMMY, '%Y-%m-%dT%H:%i:%s.%f') AS ISO_STRING_2,
    DATE_FORMAT(DD.DUMMY, '%Y') AS YEAR_OF_ERA,
    DATE_FORMAT(DD.DUMMY, '%m') AS MONTH_OF_YEAR,
    DATE_FORMAT(DD.DUMMY, '%d') AS DAY_OF_MONTH,
    DATE_FORMAT(DD.DUMMY, '%H') AS HOURS,
    DATE_FORMAT(DD.DUMMY, '%i') AS MINUTES,
    DATE_FORMAT(DD.DUMMY, '%s') AS SECONDS,
    DATE_FORMAT(DD.DUMMY, '%w') AS DAY,
    DATE_FORMAT(DD.DUMMY, '%W') AS DAY_STRING,
    DD.DUMMY BETWEEN STR_TO_DATE('2022-05-16 01:30:55', '%Y-%m-%d %H:%i:%s') AND STR_TO_DATE('2022-05-17 05:30:55', '%Y-%m-%d %H:%i:%s') AS BETWEEEEEEN
FROM (
  SELECT STR_TO_DATE(D.DUMMY, '%Y-%m-%d %H:%i:%s') AS DUMMY /* 포맷: yyyy-MM-dd HH:mm:dd */
  FROM (
    SELECT '2022-05-16 01:30:55' AS DUMMY
    UNION
    SELECT '2022-05-17 05:30:55' AS DUMMY
    UNION
    SELECT '2022-05-18 11:30:55' AS DUMMY
    UNION
    SELECT '2022-05-19 13:30:55' AS DUMMY
    UNION
    SELECT '2022-05-20 16:30:55' AS DUMMY
    UNION
    SELECT '2022-05-21 20:30:55' AS DUMMY
    UNION
    SELECT '2022-05-22 23:30:55' AS DUMMY
  ) D
) DD
```


## 타입 변환

문자열을 날짜로:

```sql
SELECT STR_TO_DATE('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s')
```

날짜를 문자열로:

```sql
SELECT DATE_FORMAT(DD.DUMMY, '%Y-%m-%d %H:%i:%s') AS ISO_STRING
```

DATETIME을 DATE로:

```sql
SELECT DATE(NOW())
```


## 타임존 변환

- [https://mariadb.com/kb/en/convert_tz/](https://mariadb.com/kb/en/convert_tz/)

```
CONVERT_TZ(dt, from_tz, to_tz)
```

```sql
SELECT CONVERT_TZ(NOW(), 'UTC', 'Asia/Seoul')
```

테스트용 쿼리:

```sql
SELECT
  NOW() AS NOW,
  CONVERT_TZ(NOW(), 'UTC', 'Asia/Seoul') AS NOW_KST,
  CONVERT_TZ(NOW(), 'UTC', 'America/Los_Angeles') AS NOW_LA,
  CURTIME() AS CT,
  CONVERT_TZ(CURTIME(), 'UTC', 'Asia/Seoul') AS CT_KST,
  CONVERT_TZ(CURTIME(), 'UTC', 'America/Los_Angeles') AS CT_LA,
  CURDATE() AS CD,
  CONVERT_TZ(CURDATE(), 'UTC', 'Asia/Seoul') AS CD_KST,
  CONVERT_TZ(CURDATE(), 'UTC', 'America/Los_Angeles') AS CD_LA,
  STR_TO_DATE('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s') AS STR,
  CONVERT_TZ(STR_TO_DATE('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s'), 'UTC', 'Asia/Seoul') AS STR_KST,
  CONVERT_TZ(STR_TO_DATE('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s'), 'UTC', 'America/Los_Angeles') AS STR_LA
```
