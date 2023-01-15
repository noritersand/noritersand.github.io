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

이 함수는 `dt`가 시간만 있거나(TIME) 날짜만 있는(DATE) 값이어도 날짜 + 시간 형태로 반환한다:

```sql
SELECT
  NOW() AS NOW,
  CONVERT_TZ(NOW(), 'UTC', 'Asia/Seoul') AS NOW_KST,
  CONVERT_TZ(NOW(), 'UTC', 'America/Los_Angeles') AS NOW_LA,
  CURTIME() AS CURTIME,
  CONVERT_TZ(CURTIME(), 'UTC', 'UTC') AS CURTIME_UTC,
  CONVERT_TZ(CURTIME(), 'UTC', 'Asia/Seoul') AS CURTIME_KST,
  CONVERT_TZ(CURTIME(), 'UTC', 'America/Los_Angeles') AS CURTIME_LA,
  CURDATE() AS CURDATE,
  CONVERT_TZ(CURDATE(), 'UTC', 'UTC') AS CURDATE_UTC,
  CONVERT_TZ(CURDATE(), 'UTC', 'Asia/Seoul') AS CURDATE_KST,
  CONVERT_TZ(CURDATE(), 'UTC', 'America/Los_Angeles') AS CURDATE_LA,
  STR_TO_DATE('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s') AS STD,
  CONVERT_TZ(STR_TO_DATE('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s'), 'UTC', 'Asia/Seoul') AS STD_KST,
  CONVERT_TZ(STR_TO_DATE('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s'), 'UTC', 'America/Los_Angeles') AS STD_LA
```

타임존이 UTC인 데이터베이스에서 2022-11-09 07:57:31 에 실행하면 이렇게 됨:

| COLUMN      | VALUE               |
|-------------|---------------------|
| NOW         | 2022-11-09 01:39:30 |
| NOW_KST     | 2022-11-09 10:39:30 |
| NOW_LA      | 2022-11-08 17:39:30 |
| CURTIME     | 01:39:30            |
| CURTIME_UTC | 2022-11-09 01:39:30 |
| CURTIME_KST | 2022-11-09 10:39:30 |
| CURTIME_LA  | 2022-11-08 17:39:30 |
| CURDATE     | 2022-11-09          |
| CURDATE_UTC | 2022-11-09 00:00:00 |
| CURDATE_KST | 2022-11-09 09:00:00 |
| CURDATE_LA  | 2022-11-08 16:00:00 |
| STD         | 2022-05-05 08:00:00 |
| STD_KST     | 2022-05-05 17:00:00 |
| STD_LA      | 2022-05-05 01:00:00 |

`CURDATE()`는 타임존을 변환해도 현재 시간을 무시하고 현재 날짜 + 00시 기준으로 변환되기 때문에 의도대로 작동하지 않으니 주의할 것.


## TIMESTAMP에서 특정 단위 추출

- [https://mariadb.com/kb/en/extract/](https://mariadb.com/kb/en/extract/)
- [https://mariadb.com/kb/en/date-and-time-units/](https://mariadb.com/kb/en/date-and-time-units/)

```sql
SELECT
  DATE_FORMAT(V.currentTS, '%Y-%m-%dT%H:%i:%s.%f') AS `CURRENT_TIMESTAMP`,
  EXTRACT(YEAR FROM V.currentTS) AS `YEAR`,
  EXTRACT(YEAR_MONTH FROM V.currentTS) AS `YEAR_MONTH`,
  EXTRACT(MONTH FROM V.currentTS) AS `MONTH`,
  EXTRACT(DAY FROM V.currentTS) AS `DAY`,
  EXTRACT(DAY_HOUR FROM V.currentTS) AS `DAY_HOUR`,
  EXTRACT(DAY_MINUTE FROM V.currentTS) AS `DAY_MINUTE`,
  EXTRACT(DAY_SECOND FROM V.currentTS) AS `DAY_SECOND`,
  EXTRACT(DAY_MICROSECOND FROM V.currentTS) AS `DAY_MICROSECOND`,
  EXTRACT(HOUR FROM V.currentTS) AS `HOUR`,
  EXTRACT(HOUR_MINUTE FROM V.currentTS) AS `HOUR_MINUTE`,
  EXTRACT(HOUR_SECOND FROM V.currentTS) AS `HOUR_SECOND`,
  EXTRACT(HOUR_MICROSECOND FROM V.currentTS) AS `HOUR_MICROSECOND`,
  EXTRACT(MINUTE FROM V.currentTS) AS `MINUTE`,
  EXTRACT(MINUTE_SECOND FROM V.currentTS) AS `MINUTE_SECOND`,
  EXTRACT(MINUTE_MICROSECOND FROM V.currentTS) AS `MINUTE_MICROSECOND`,
  EXTRACT(SECOND FROM V.currentTS) AS `SECOND`,
  EXTRACT(SECOND_MICROSECOND FROM V.currentTS) AS `SECOND_MICROSECOND`,
  EXTRACT(MICROSECOND FROM V.currentTS) AS `MICROSECOND`,
  EXTRACT(WEEK FROM V.currentTS) AS `WEEK`,
  EXTRACT(QUARTER FROM V.currentTS) AS `QUARTER`
FROM (
  SELECT NOW(6) AS currentTS /*마이크로초 여섯 자리까지*/
) V
```

| **UNIT**            | **VALUE**                   |
| ------------------- | --------------------------- |
| CURRENT\_TIMESTAMP  | 2022-12-14T05:51:06.312446  |
| YEAR                | 2022                        |
| YEAR\_MONTH         | 202212                      |
| MONTH               | 12                          |
| DAY                 | 14                          |
| DAY\_HOUR           | 1405                        |
| DAY\_MINUTE         | 140551                      |
| DAY\_SECOND         | 14055106                    |
| DAY\_MICROSECOND    | 14055106312446              |
| HOUR                | 5                           |
| HOUR\_MINUTE        | 551                         |
| HOUR\_SECOND        | 55106                       |
| HOUR\_MICROSECOND   | 55106312446                 |
| MINUTE              | 51                          |
| MINUTE\_SECOND      | 5106                        |
| MINUTE\_MICROSECOND | 5106312446                  |
| SECOND              | 6                           |
| SECOND\_MICROSECOND | 6312446                     |
| MICROSECOND         | 312446                      |
| WEEK                | 50                          |
| QUARTER             | 4                           |
