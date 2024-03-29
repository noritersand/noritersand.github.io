---
layout: post
date: 2017-05-03 19:31:00 +0900
title: '[Oracle Database] 년, 월을 입력받아 달력 출력'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

```sql
SELECT
  MIN (DECODE (TO_CHAR (DAYS, 'D'), 1, TO_CHAR (DAYS, 'FMDD'))) AS SUN,
  MIN (DECODE (TO_CHAR (DAYS, 'D'), 2, TO_CHAR (DAYS, 'FMDD'))) AS MON,
  MIN (DECODE (TO_CHAR (DAYS, 'D'), 3, TO_CHAR (DAYS, 'FMDD'))) AS TUE,
  MIN (DECODE (TO_CHAR (DAYS, 'D'), 4, TO_CHAR (DAYS, 'FMDD'))) AS WED,
  MIN (DECODE (TO_CHAR (DAYS, 'D'), 5, TO_CHAR (DAYS, 'FMDD'))) AS THU,
  MIN (DECODE (TO_CHAR (DAYS, 'D'), 6, TO_CHAR (DAYS, 'FMDD'))) AS FRI,
  MIN (DECODE (TO_CHAR (DAYS, 'D'), 7, TO_CHAR (DAYS, 'FMDD'))) AS SAT
FROM (
  SELECT
    BASE_MON + LEVEL - 1 AS DAYS,
    (TRUNC(BASE_MON + LEVEL - 1, 'D') - TRUNC(TRUNC(BASE_MON + LEVEL - 1, 'Y'), 'D')) / 7 + 1 AS WEEK_GRP
  FROM (
    SELECT
      TO_DATE (NVL(2017, TO_CHAR(SYSDATE, 'YYYY')) || NVL(12, TO_CHAR(SYSDATE, 'MM')), 'YYYYMM') AS BASE_MON
    FROM
      DUAL
  )
  CONNECT BY
    BASE_MON + LEVEL - 1 <= LAST_DAY (BASE_MON)
)
GROUP BY WEEK_GRP
ORDER BY WEEK_GRP
```
