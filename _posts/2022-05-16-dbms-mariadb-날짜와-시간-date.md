---
layout: post
date: 2022-05-16 18:06:38 +0900
title: '[DBMS] MariaDB 날짜와 시간'
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

시간을 다루는 데이터 타입 `DATE`, `DATETIME`, `TIME`과 관련 함수 사용법 정리 글.


## 타입 변환

문자열을 날짜로:

```sql
select str_to_date('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s')
```

날짜를 문자열로:

```sql
select date_format(d.dummy, '%Y-%m-%d %H:%i:%s') as isoString
from (
  select '2022-05-16 01:30:55' as dummy
) d
```

`DATETIME`을 `DATE` 혹은 `TIME`으로:

```sql
select date(now());
select time(now());
```

`convert()` 함수로 변환:

```sql
select convert(now(), date);
select convert(now(), time);
```

### DATE_FORMAT(), STR_TO_DATE()

`DATE_FORMAT()`은 날짜 데이터를 특정 포맷의 문자열로 변환한다. `STR_TO_DATE()`는 반대로 문자열을 날짜 데이터로 변환한다.

```sql
select
    dd.dummy as dateDefault,
    date_format(dd.dummy, '%Y-%m-%d %H:%i:%s') as isoString,
    date_format(dd.dummy, '%Y-%m-%dT%H:%i:%s.%f') as isoString2,
    date_format(dd.dummy, '%Y-%m-%d') as isoString3,
    date_format(dd.dummy, '%Y') as yearOfEra,
    date_format(dd.dummy, '%m') as monthOfYear,
    date_format(dd.dummy, '%d') as dayOfMonth,
    date_format(dd.dummy, '%H') as hours,
    date_format(dd.dummy, '%i') as minutes,
    date_format(dd.dummy, '%s') as seconds,
    date_format(dd.dummy, '%w') as day,
    date_format(dd.dummy, '%W') as dayString,
    dd.dummy between str_to_date('2022-05-16 01:30:55', '%Y-%m-%d %H:%i:%s') and str_to_date('2022-05-17 05:30:55', '%Y-%m-%d %H:%i:%s') as betweeeeeen
from (
  select str_to_date(d.dummy, '%Y-%m-%d %H:%i:%s') as dummy /* 포맷: yyyy-MM-dd HH:mm:dd */
  from (
    select '2022-05-16 01:30:55' as dummy
    union
    select '2022-05-17 05:30:55' as dummy
    union
    select '2022-05-18 11:30:55' as dummy
    union
    select '2022-05-19 13:30:55' as dummy
    union
    select '2022-05-20 16:30:55' as dummy
    union
    select '2022-05-21 20:30:55' as dummy
    union
    select '2022-05-22 23:30:55' as dummy
  ) d
) dd
```

+-------------------+-------------------+--------------------------+----------+---------+-----------+----------+-----+-------+-------+---+---------+-----------+
|dateDefault        |isoString          |isoString2                |isoString3|yearOfEra|monthOfYear|dayOfMonth|hours|minutes|seconds|day|dayString|betweeeeeen|
+-------------------+-------------------+--------------------------+----------+---------+-----------+----------+-----+-------+-------+---+---------+-----------+
|2022-05-16 01:30:55|2022-05-16 01:30:55|2022-05-16T01:30:55.000000|2022-05-16|2022     |05         |16        |01   |30     |55     |1  |Monday   |1          |
|2022-05-17 05:30:55|2022-05-17 05:30:55|2022-05-17T05:30:55.000000|2022-05-17|2022     |05         |17        |05   |30     |55     |2  |Tuesday  |1          |
|2022-05-18 11:30:55|2022-05-18 11:30:55|2022-05-18T11:30:55.000000|2022-05-18|2022     |05         |18        |11   |30     |55     |3  |Wednesday|0          |
|2022-05-19 13:30:55|2022-05-19 13:30:55|2022-05-19T13:30:55.000000|2022-05-19|2022     |05         |19        |13   |30     |55     |4  |Thursday |0          |
|2022-05-20 16:30:55|2022-05-20 16:30:55|2022-05-20T16:30:55.000000|2022-05-20|2022     |05         |20        |16   |30     |55     |5  |Friday   |0          |
|2022-05-21 20:30:55|2022-05-21 20:30:55|2022-05-21T20:30:55.000000|2022-05-21|2022     |05         |21        |20   |30     |55     |6  |Saturday |0          |
|2022-05-22 23:30:55|2022-05-22 23:30:55|2022-05-22T23:30:55.000000|2022-05-22|2022     |05         |22        |23   |30     |55     |0  |Sunday   |0          |
+-------------------+-------------------+--------------------------+----------+---------+-----------+----------+-----+-------+-------+---+---------+-----------+

## 날짜 포맷

지원하는 모든 날짜 포맷은 [요 페이지](https://mariadb.com/kb/en/date_format/)를 보자.

자주 쓰이는 건 요 정도:

- `%Y`: yearOfEra
- `%m`: monthOfYear
- `%d`: dayOfMonth
- `%H`: hours
- `%i`: minutes
- `%s`: seconds
- `%w`: day
- `%W`: dayString

ISO 같은 잘 알려진 포맷은 `GET_FORMAT()` 함수로 얻을 수 있다:

```sql
select get_format(date,'iso'), get_format(datetime,'iso')
```

+----------------------+--------------------------+
|get_format(date,'iso')|get_format(datetime,'iso')|
+----------------------+--------------------------+
|%Y-%m-%d              |%Y-%m-%d %H:%i:%s         |
+----------------------+--------------------------+


## 그 밖의 날짜 관련 함수들

### 현재 날짜/시간 구하기

- `curdate()`: 공식 문서 설명에 따르면 단순히 'YYYY-MM-DD' 혹은 YYYYMMDD 포맷의 현재 날짜값을 반환한다. 동의어로 `current_date`가 있다.
- `curtime([precision])`: 'HH:MM:SS' 혹은 HHMMSS.uuuuuu 포맷의 현재 시간값을 반환한다. 동의어로 `current_time`이 있다. `precision`은 선택사항이다. 마이크로초 정밀도를 의미하며 0에서 6까지의 값을 입력할 수 있다.
- `now([precision])`: 'YYYY-MM-DD HH:MM:SS' 혹은 YYYYMMDDHHMMSS.uuuuuu 포맷의 시간과 날짜값을 반환한다. 동의어는 `localtime`, `localtimestamp`, `current_timestamp`. `precision`은 마찬가지로 선택사항이며, 0-6 사이의 마이크로초 정밀도다.

```sql
select now()
```

### 특정 조건의 날짜를 반환하는 함수들

- `last_day(date)`: `date`가 속한 달의 마지막 날을 반환

### 날짜/시간의 연산(더하고 빼기)

```
date_add(date, interval expr unit)
date_sub(date,interval expr unit)
```

`adddate()`, `subdate()`, `addtime()`, `subtime()`, `add_months()` 등 비슷한 함수가 많이 있는데 그냥 위 두개로 웬만하면 됨.

```sql
select 
  date_add(now(), interval 1 day), 
  date_sub(now(), interval 2 month)
```

`unit` 자리에 올 수 있는 키워드는 [이 문서에 있고](https://mariadb.com/kb/en/date-and-time-units/), 요약하면 아래와 같다:

- `MICROSECOND`: Microseconds
- `SECOND`: Seconds
- `MINUTE`: Minutes
- `HOUR`: Hours
- `DAY`: Days
- `WEEK`: Weeks
- `MONTH`: Months
- `QUARTER`: Quarters
- `YEAR`: Years
- `SECOND_MICROSECOND`: Seconds.Microseconds
- `MINUTE_MICROSECOND`: Minutes.Seconds.Microseconds
- `MINUTE_SECOND`: Minutes.Seconds
- `HOUR_MICROSECOND`: Hours.Minutes.Seconds.Microseconds
- `HOUR_SECOND`: Hours.Minutes.Seconds
- `HOUR_MINUTE`: Hours.Minutes
- `DAY_MICROSECOND`: Days Hours.Minutes.Seconds.Microseconds
- `DAY_SECOND`: Days Hours.Minutes.Seconds
- `DAY_MINUTE`: Days Hours.Minutes
- `DAY_HOUR`: Days Hours
- `YEAR_MONTH`: Years-Months

사실 함수를 쓰지 않고 그냥 연산자로 해결하는 방법도 있다.

```sql
select 
    current_date() + interval 3 month,
    current_date() - interval 3 year
```


## 타임존 변환

- [https://mariadb.com/kb/en/convert_tz/](https://mariadb.com/kb/en/convert_tz/)

```
CONVERT_TZ(dt, from_tz, to_tz)
```

```sql
select convert_tz(now(), 'UTC', 'Asia/Seoul')
```

이 함수는 `dt`가 시간만 있거나(TIME) 날짜만 있는(DATE) 값이어도 날짜 + 시간 형태로 반환한다:

```sql
select
  now() as now,
  convert_tz(now(), 'UTC', 'Asia/Seoul') as nowKst,
  convert_tz(now(), 'UTC', 'America/Los_Angeles') as nowLa,
  curtime() as curtime,
  convert_tz(curtime(), 'UTC', 'UTC') as curtimeUtc,
  convert_tz(curtime(), 'UTC', 'Asia/Seoul') as curtimeKst,
  convert_tz(curtime(), 'UTC', 'America/Los_Angeles') as curtimeLa,
  curdate() as curdate,
  convert_tz(curdate(), 'UTC', 'UTC') as curdateUtc,
  convert_tz(curdate(), 'UTC', 'Asia/Seoul') as curdateKst,
  convert_tz(curdate(), 'UTC', 'America/Los_Angeles') as curdateLa,
  str_to_date('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s') as std,
  convert_tz(str_to_date('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s'), 'UTC', 'Asia/Seoul') as stdKst,
  convert_tz(str_to_date('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s'), 'UTC', 'America/Los_Angeles') as stdLa
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


## Date에서 특정 단위 추출

- [https://mariadb.com/kb/en/extract/](https://mariadb.com/kb/en/extract/)
- [https://mariadb.com/kb/en/date-and-time-units/](https://mariadb.com/kb/en/date-and-time-units/)

```sql
select
  date_format(v.currentTs, '%Y-%m-%dT%H:%i:%s.%f') AS currentTimestamp,
  extract(year from v.currentTs) AS year,
  extract(year_month from v.currentTs) AS yearMonth,
  extract(month from v.currentTs) AS month,
  extract(day from v.currentTs) AS day,
  extract(day_hour from v.currentTs) AS dayHour,
  extract(day_minute from v.currentTs) AS dayMinute,
  extract(day_second from v.currentTs) AS daySecond,
  extract(day_microsecond from v.currentTs) AS dayMicrosecond,
  extract(hour from v.currentTs) AS hour,
  extract(hour_minute from v.currentTs) AS hourMinute,
  extract(hour_second from v.currentTs) AS hourSecond,
  extract(hour_microsecond from v.currentTs) AS hourMicrosecond,
  extract(minute from v.currentTs) AS minute,
  extract(minute_second from v.currentTs) AS minuteSecond,
  extract(minute_microsecond from v.currentTs) AS minuteMicrosecond,
  extract(second from v.currentTs) AS second,
  extract(second_microsecond from v.currentTs) AS secondMicrosecond,
  extract(microsecond from v.currentTs) AS microsecond,
  extract(week from v.currentTs) AS week,
  extract(quarter from v.currentTs) AS quarter
from (
  select now(6) as currentTs /*마이크로초 여섯 자리까지*/
) v
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


## DATETIME을 DATE로 BETWEEN 비교

먼저 BETWEEN 비교가 조건항을 포함하는지를 보자:

```sql
select
    if(0 between 0 and 2, 'true', 'false'), -- true
    if(1 between 0 and 2, 'true', 'false'), -- true
    if(2 between 0 and 2, 'true', 'false')  -- true
```

포함한다.

```sql
select
    a.startDate,
    a.endDate,
    a.compareMe1, if(a.compareMe1 between a.startDate and a.endDate, 'true', 'false') as isInRange1,
    a.compareMe2, if(a.compareMe2 between a.startDate and a.endDate, 'true', 'false') as isInRange2,
    a.compareMe3, if(a.compareMe3 between a.startDate and a.endDate, 'true', 'false') as isInRange3,
    a.compareMe4, if(a.compareMe4 between a.startDate and a.endDate, 'true', 'false') as isInRange4,
    a.compareMe5, if(a.compareMe5 between a.startDate and a.endDate, 'true', 'false') as isInRange5
from (
    select
        str_to_date('2018-01-01', '%Y-%m-%d') as startDate,
        str_to_date('2018-01-03', '%Y-%m-%d') as endDate,
        str_to_date('2018-01-01 00:00:00', '%Y-%m-%d %H:%i:%s') as compareMe1,
        str_to_date('2018-01-02 23:59:59', '%Y-%m-%d %H:%i:%s') as compareMe2,
        str_to_date('2018-01-03 00:00:00', '%Y-%m-%d %H:%i:%s') as compareMe3,
        str_to_date('2018-01-03 00:30:00', '%Y-%m-%d %H:%i:%s') as compareMe4,
        str_to_date('2018-01-03 23:59:59', '%Y-%m-%d %H:%i:%s') as compareMe5
) a
```

실행해 보면 `isInRange4`와 `isInRange5`만 false인데, `2018-01-03`을 DATE 타입으로 만들면 시분초 값이 모두 0으로 설정된다는 것을 추측할 수 있음.

따라서 조건으로 대입된 날짜의 23:59:59(23시 59분 59초)까지라고 지정하고 싶으면, DATE를 문자열로 만들고 다시 DATETIME으로 변환하면서 시분초를 붙이는 방법을 고려할 수 있다.

결국 이런 모양이 나옴:

```sql
select
    b.startDate, b.endDateTime,
    b.compareMe1, if(compareMe1 between b.startDate and b.endDateTime, 'true', 'false') as isInRange1,
    b.compareMe2, if(compareMe2 between b.startDate and b.endDateTime, 'true', 'false') as isInRange2,
    b.compareMe3, if(compareMe3 between b.startDate and b.endDateTime, 'true', 'false') as isInRange3
from (
    select
        startDate,
        convert(concat(date_format(a.endDate, '%Y-%m-%d'), ' 23:59:59'), datetime) as endDateTime,
        str_to_date('2018-01-01 00:00:00', '%Y-%m-%d %H:%i:%s') as compareMe1,
        str_to_date('2018-01-03 00:00:00', '%Y-%m-%d %H:%i:%s') as compareMe2,
        str_to_date('2018-01-03 23:59:59', '%Y-%m-%d %H:%i:%s') as compareMe3
    from (
        select
            str_to_date('2018-01-01', '%Y-%m-%d') as startDate,
            str_to_date('2018-01-03', '%Y-%m-%d') as endDate
    ) a
) b
```

위에서 시분초를 더하는 부분은:

```sql
convert(concat(date_format(a.endDate, '%Y-%m-%d'), ' 23:59:59')
```

하루를 더하고 1초를 빼는 방법으로 바꿔도 된다:

```sql
date_sub(date_add('2022-12-24', interval 1 day), interval 1 second)
# 혹은
'2022-12-24' + interval 1 day - interval 1 second
```

문자열 리터럴은 안쓰는 게 좋으니 가급적 이걸 사용하도록 하자. 😏
