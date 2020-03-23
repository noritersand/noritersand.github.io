---
layout: post
date: 2015-04-09 20:07:00 +0900
title: '[Oracle] connect by로 날짜 데이터 만들기'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - connect-by
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

### 날짜범위 지정

```sql
SELECT
    TO_CHAR(TO_DATE('20150401','yyyymmdd') + LEVEL - 1, 'yyyymmdd') AS SOLAR_DATE,
    TO_CHAR(TO_DATE('20150401','yyyymmdd') + LEVEL - 1, 'D') AS WEEK_DAY
FROM DUAL
CONNECT BY LEVEL <= TO_DATE('20150530','yyyymmdd') - TO_DATE('20150401','yyyymmdd') + 1
```

### 오늘 기준 30일 전후

```sql
SELECT
    TO_CHAR((SYSDATE - 30) + LEVEL - 1, 'yyyymmdd') AS SOLAR_DATE,
    TO_CHAR((SYSDATE - 30) + LEVEL - 1, 'D') AS WEEK_DAY
FROM DUAL
CONNECT BY LEVEL <= (SYSDATE + 30) - (SYSDATE - 30) + 1
```

### 오늘 기준 마지막 날까지 출력

```sql
SELECT SYSDATE + LEVEL -1 AS DAYS
FROM DUAL
CONNECT BY SYSDATE + LEVEL -1 <= LAST_DAY(SYSDATE)
```
