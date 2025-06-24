---
layout: post
date: 2022-05-16 18:06:38 +0900
title: '[MariaDB] 날짜와 시간'
categories:
  - mariadb
tags:
  - mariadb
  - dbms
  - sql
  - code-snippet
  - date
  - datetime
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [MariaDB \| STR_TO_DATE](https://mariadb.com/kb/en/str_to_date/)
- [MariaDB \| DATE_FORMAT](https://mariadb.com/kb/en/date_format/)
- [MariaDB \| DATETIME](https://mariadb.com/kb/en/datetime/)

#### 테스트 환경 정보

- MariaDB 10.5.17
- MySQL 8.0.25


## 개요

시간을 다루는 데이터 타입 `DATE`, `DATETIME`, `TIME`과 관련 함수 사용법 정리 글.


## 타입 변환

문자열을 날짜로:

```sql
select str_to_date('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s')
```

날짜를 문자열로:

```sql
select date_format(d.dummy, '%Y-%m-%d %H:%i:%s') as iso_string
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
    dd.dummy as date_default,
    date_format(dd.dummy, '%Y-%m-%d %H:%i:%s') as iso_string,
    date_format(dd.dummy, '%Y-%m-%dT%H:%i:%s.%f') as iso_string2,
    date_format(dd.dummy, '%Y-%m-%d') as iso_string3,
    date_format(dd.dummy, '%Y') as year_of_era,
    date_format(dd.dummy, '%m') as month_of_year,
    date_format(dd.dummy, '%d') as day_of_month,
    date_format(dd.dummy, '%H') as hours,
    date_format(dd.dummy, '%i') as minutes,
    date_format(dd.dummy, '%s') as seconds,
    date_format(dd.dummy, '%w') as day,
    date_format(dd.dummy, '%W') as day_string,
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

| date_default        | iso_string          | iso_string2                | iso_string3 | year_of_era | month_of_year | day_of_month | hours | minutes | seconds | day | day_string | betweeeeeen |
|---------------------|---------------------|----------------------------|-------------|-------------|---------------|--------------|-------|---------|---------|-----|------------|-------------|
| 2022-05-16 01:30:55 | 2022-05-16 01:30:55 | 2022-05-16T01:30:55.000000 | 2022-05-16  | 2022        | 05            | 16           | 01    | 30      | 55      | 1   | Monday     | 1           |
| 2022-05-17 05:30:55 | 2022-05-17 05:30:55 | 2022-05-17T05:30:55.000000 | 2022-05-17  | 2022        | 05            | 17           | 05    | 30      | 55      | 2   | Tuesday    | 1           |
| 2022-05-18 11:30:55 | 2022-05-18 11:30:55 | 2022-05-18T11:30:55.000000 | 2022-05-18  | 2022        | 05            | 18           | 11    | 30      | 55      | 3   | Wednesday  | 0           |
| 2022-05-19 13:30:55 | 2022-05-19 13:30:55 | 2022-05-19T13:30:55.000000 | 2022-05-19  | 2022        | 05            | 19           | 13    | 30      | 55      | 4   | Thursday   | 0           |
| 2022-05-20 16:30:55 | 2022-05-20 16:30:55 | 2022-05-20T16:30:55.000000 | 2022-05-20  | 2022        | 05            | 20           | 16    | 30      | 55      | 5   | Friday     | 0           |
| 2022-05-21 20:30:55 | 2022-05-21 20:30:55 | 2022-05-21T20:30:55.000000 | 2022-05-21  | 2022        | 05            | 21           | 20    | 30      | 55      | 6   | Saturday   | 0           |
| 2022-05-22 23:30:55 | 2022-05-22 23:30:55 | 2022-05-22T23:30:55.000000 | 2022-05-22  | 2022        | 05            | 22           | 23    | 30      | 55      | 0   | Sunday     | 0           |


## 날짜 포맷

지원하는 모든 날짜 포맷은 [요 페이지](https://mariadb.com/kb/en/date_format/)를 보자.

자주 쓰이는 건 요 정도:

- `%Y`: year of era
- `%m`: month of year
- `%d`: day of month
- `%H`: hours
- `%i`: minutes
- `%s`: seconds
- `%w`: day
- `%W`: day string

ISO 같은 잘 알려진 포맷은 [`GET_FORMAT()`](https://mariadb.com/kb/en/get_format/) 함수로 얻을 수 있다:

```sql
select get_format(date, 'iso'), get_format(datetime, 'iso')
```

| get_format(date, 'iso') | get_format(datetime, 'iso') |
|-------------------------|-----------------------------|
| %Y-%m-%d                | %Y-%m-%d %H:%i:%s           |

ℹ️ 여기서 `date`, `datetime`은 날짜값을 의미하는 변수나 컬럼이 아니라 [유닛](https://mariadb.com/kb/en/date-and-time-units/)이다.


## 현재 날짜/시간 구하기

함수를 활용한다:

- `CURDATE()`: 공식 문서 설명에 따르면 단순히 `YYYY-MM-DD` 혹은 `YYYYMMDD` 포맷의 현재 날짜값을 반환한다. 동의어로 `current_date`가 있다.
- `CURTIME([precision])`: `HH:MM:SS` 혹은 `HHMMSS.uuuuuu` 포맷의 현재 시간값을 반환한다. 동의어로 `current_time`이 있다. `precision`은 선택사항이다. 마이크로초 정밀도를 의미하며 0에서 6까지의 값을 입력할 수 있다.
- `NOW([precision])`: `YYYY-MM-DD HH:MM:SS` 혹은 `YYYYMMDDHHMMSS.uuuuuu` 포맷의 시간과 날짜값을 반환한다. 동의어는 `localtime`, `localtimestamp`, `current_timestamp`. `precision`은 마찬가지로 선택사항이며, 0-6 사이의 마이크로초 정밀도다.

```sql
select now()
```


## 특정 조건의 날짜를 반환하는 함수들

- `LAST_DAY(date)`: `date`가 속한 달의 마지막 날을 반환


## 날짜/시간의 연산(더하고 빼기)

```
DATE_ADD(date, INTERVAL expr unit)
DATE_SUB(date, INTERVAL expr unit)
```

`ADDDATE()`, `SUBDATE()`, `ADDTIME()`, `SUBTIME()`, `ADD_MONTHS()` 등 비슷한 함수가 많이 있는데 그냥 위 두 개로 웬만하면 됨.

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
# UTC에서 KST로 변환(단순히 9시간을 더한 것과 같음)
select convert_tz(now(), 'UTC', 'Asia/Seoul')
```

이 함수는 `dt`가 시간만 있거나(TIME) 날짜만 있는(DATE) 값이어도 날짜 + 시간 형태로 반환한다:

```sql
select
  now() as now,
  convert_tz(now(), 'UTC', 'Asia/Seoul') as now_kst,
  convert_tz(now(), 'UTC', 'America/Los_Angeles') as now_la,
  curtime() as curtime,
  convert_tz(curtime(), 'UTC', 'UTC') as curtime_utc,
  convert_tz(curtime(), 'UTC', 'Asia/Seoul') as curtime_kst,
  convert_tz(curtime(), 'UTC', 'America/Los_Angeles') as curtime_la,
  curdate() as curdate,
  convert_tz(curdate(), 'UTC', 'UTC') as curdate_utc,
  convert_tz(curdate(), 'UTC', 'Asia/Seoul') as curdate_kst,
  convert_tz(curdate(), 'UTC', 'America/Los_Angeles') as curdate_la,
  str_to_date('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s') as std,
  convert_tz(str_to_date('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s'), 'UTC', 'Asia/Seoul') as std_kst,
  convert_tz(str_to_date('2022-05-05 08:00:00', '%Y-%m-%d %H:%i:%s'), 'UTC', 'America/Los_Angeles') as std_la
```

타임존이 UTC인 데이터베이스에서 `2022-11-09 07:57:31` 시각에 실행하면 이렇게 됨:

| COLUMN       | VALUE               |
|--------------|---------------------|
| now          | 2022-11-09 01:39:30 |
| now_kst      | 2022-11-09 10:39:30 |
| now_la       | 2022-11-08 17:39:30 |
| curtime      | 01:39:30            |
| curtime_utc  | 2022-11-09 01:39:30 |
| curtime_kst  | 2022-11-09 10:39:30 |
| curtime_la   | 2022-11-08 17:39:30 |
| curdate      | 2022-11-09          |
| curdate_utc  | 2022-11-09 00:00:00 |
| curdate_kst  | 2022-11-09 09:00:00 |
| curdate_la   | 2022-11-08 16:00:00 |
| std          | 2022-05-05 08:00:00 |
| std_kst      | 2022-05-05 17:00:00 |
| std_la       | 2022-05-05 01:00:00 |

`curdate()`는 타임존을 변환해도 현재 시간을 무시하고 현재 날짜 + 00시 기준으로 변환되기 때문에 의도대로 작동하지 않으니 주의할 것.


## Date에서 특정 단위 추출

### EXTRACT()

- [https://mariadb.com/kb/en/extract/](https://mariadb.com/kb/en/extract/)
- [https://mariadb.com/kb/en/date-and-time-units/](https://mariadb.com/kb/en/date-and-time-units/)

```
EXTRACT(unit FROM date)
```

```sql
select
  date_format(v.current_ts, '%Y-%m-%dT%H:%i:%s.%f') as 'current_timestamp',
  extract(year from v.current_ts) as 'year',
  extract(year_month from v.current_ts) as 'year_month',
  extract(month from v.current_ts) as 'month',
  extract(day from v.current_ts) as 'day',
  extract(day_hour from v.current_ts) as 'day_hour',
  extract(day_minute from v.current_ts) as 'day_minute',
  extract(day_second from v.current_ts) as 'day_second',
  extract(day_microsecond from v.current_ts) as 'day_microsecond',
  extract(hour from v.current_ts) as 'hour',
  extract(hour_minute from v.current_ts) as 'hour_minute',
  extract(hour_second from v.current_ts) as 'hour_second',
  extract(hour_microsecond from v.current_ts) as 'hour_microsecond',
  extract(minute from v.current_ts) as 'minute',
  extract(minute_second from v.current_ts) as 'minute_second',
  extract(minute_microsecond from v.current_ts) as 'minute_microsecond',
  extract(second from v.current_ts) as 'second',
  extract(second_microsecond from v.current_ts) as 'second_microsecond',
  extract(microsecond from v.current_ts) as 'microsecond',
  extract(week from v.current_ts) as 'week',
  extract(quarter from v.current_ts) as 'quarter'
from (
  select now(6) as current_ts /*마이크로초 여섯 자리까지*/
) v
```

| **UNIT**            | **VALUE**                   |
| ------------------- | --------------------------- |
| current_timestamp   | 2022-12-14T05:51:06.312446  |
| year                | 2022                        |
| year_month          | 202212                      |
| month               | 12                          |
| day                 | 14                          |
| day_hour            | 1405                        |
| day_minute          | 140551                      |
| day_second          | 14055106                    |
| day_microsecond     | 14055106312446              |
| hour                | 5                           |
| hour_minute         | 551                         |
| hour_second         | 55106                       |
| hour_microsecond    | 55106312446                 |
| minute              | 51                          |
| minute_second       | 5106                        |
| minute_microsecond  | 5106312446                  |
| second              | 6                           |
| second_microsecond  | 6312446                     |
| microsecond         | 312446                      |
| week                | 50                          |
| quarter             | 4                           |

### 그 외 함수들

날짜:

- `YEAR()`: 주어진 날짜의 연도를 네 자릿수로 반환
- `MONTH()`: 주어진 날짜의 월을 1-12 사이의 값으로 반환
- `DAYOFMONTH()` `DAY()`: 주어진 날짜의 날짜를 1-31 사이의 값으로 반환
- `DAYOFWEEK()`: 주어진 날짜가 해당 주의 몇 번째 날인지 반환한다. (1=일요일, 2=월요일, ..., 7=토요일)
- `WEEKDAY()`: 주어진 날짜가 해당 주의 몇 번째 날인지 반환한다. (0=월요일, 1=화요일, ..., 6=일요일)

시간:

- `HOUR(time)`: 0-23 반환
- `MINUTE(time)`: 0-59 반환
- `SECOND(time)`: 0-59 반환
- `MICROSECOND(expr)`: 0-999999 반환


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
    a.start_date,
    a.end_date,
    a.compare_me1, if(a.compare_me1 between a.start_date and a.end_date, 'true', 'false') as is_in_range1,
    a.compare_me2, if(a.compare_me2 between a.start_date and a.end_date, 'true', 'false') as is_in_range2,
    a.compare_me3, if(a.compare_me3 between a.start_date and a.end_date, 'true', 'false') as is_in_range3,
    a.compare_me4, if(a.compare_me4 between a.start_date and a.end_date, 'true', 'false') as is_in_range4,
    a.compare_me5, if(a.compare_me5 between a.start_date and a.end_date, 'true', 'false') as is_in_range5
from (
    select
        str_to_date('2018-01-01', '%Y-%m-%d') as start_date,
        str_to_date('2018-01-03', '%Y-%m-%d') as end_date,
        str_to_date('2018-01-01 00:00:00', '%Y-%m-%d %H:%i:%s') as compare_me1,
        str_to_date('2018-01-02 23:59:59', '%Y-%m-%d %H:%i:%s') as compare_me2,
        str_to_date('2018-01-03 00:00:00', '%Y-%m-%d %H:%i:%s') as compare_me3,
        str_to_date('2018-01-03 00:30:00', '%Y-%m-%d %H:%i:%s') as compare_me4,
        str_to_date('2018-01-03 23:59:59', '%Y-%m-%d %H:%i:%s') as compare_me5
) a
```

실행해 보면 `is_in_range4`와 `is_in_range5`만 false인데, `2018-01-03`을 DATE 타입으로 만들면 시분초가 `00:00:00`으로 설정된다는 것을 추측할 수 있음.

따라서 조건으로 대입된 날짜의 23시 59분 59초 까지로 지정하고 싶으면, DATE를 문자열로 만들고 다시 DATETIME으로 변환하면서 시분초를 붙이는 방법을 고려할 수 있다.

결국 이런 모양이 나옴:

```sql
select
    b.start_date, b.end_date_time,
    b.compare_me1, if(compare_me1 between b.start_date and b.end_date_time, 'true', 'false') as is_in_range1,
    b.compare_me2, if(compare_me2 between b.start_date and b.end_date_time, 'true', 'false') as is_in_range2,
    b.compare_me3, if(compare_me3 between b.start_date and b.end_date_time, 'true', 'false') as is_in_range3
from (
    select
        start_date,
        convert(concat(date_format(a.end_date, '%Y-%m-%d'), ' 23:59:59'), datetime) as end_date_time,
        str_to_date('2018-01-01 00:00:00', '%Y-%m-%d %H:%i:%s') as compare_me1,
        str_to_date('2018-01-03 00:00:00', '%Y-%m-%d %H:%i:%s') as compare_me2,
        str_to_date('2018-01-03 23:59:59', '%Y-%m-%d %H:%i:%s') as compare_me3
    from (
        select
            str_to_date('2018-01-01', '%Y-%m-%d') as start_date,
            str_to_date('2018-01-03', '%Y-%m-%d') as end_date
    ) a
) b
```


## 23시 59분 59초

위에 작성한 쿼리 중 `DATE`에 시분초를 더해 `DATETIME`을 만드는 부분이 있는데:

```sql
convert(concat(date_format(a.end_date, '%Y-%m-%d'), ' 23:59:59'), datetime)
```

가독성이 별로다 싶으면 하루를 더하고 1초를 빼는 방법도 있다:

```sql
date_sub(date_add('2022-12-24', interval 1 day), interval 1 second)
-- 혹은
'2022-12-24' + interval 1 day - interval 1 second
```

문자열 리터럴은 안쓰는 게 좋으니 가급적 이걸 사용하도록 하자. 😏


## 세션의 시간대 변경하기

MariaDB 서버 인스턴스의 시간대를 변경할 수 없을 때, 세션의 시간대만 변경하는 방법이다. 데이터소스의 URL에 아래를 덧붙이면 된다:

```
?serverTimezone=UTC&useLegacyDatetimeCode=false&sessionVariables=time_zone='+09:00'
```

관련 글 링크: [https://jira.mariadb.org/browse/CONJ-433](https://jira.mariadb.org/browse/CONJ-433)

만약 애플리케이션 데이터소스에 이 설정을 더해줬을 때 9시간이 아니라 18시간을 더한 값이 반환된다면 `serverTimezone=UTC` 혹은 `sessionVariables=time_zone='+09:00'` 둘 중 하나를 빼야 한다. 기준 시간이 무엇인지를 정하는 매개변수들 같은데 정확한 건 아직 모름.

ℹ️ `time_zone`은 [공식 도움말](https://mariadb.com/kb/en/server-system-variables/#time_zone)이 있지만 `serverTimezone`은 없음.
