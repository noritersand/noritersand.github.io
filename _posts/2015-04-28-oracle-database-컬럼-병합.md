---
layout: post
date: 2015-04-28 21:21:00 +0900
title: '[Oracle Database] 컬럼 병합'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - column-merge
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

### 방법 1. FULL JOIN

```sql
WITH TEMP1 AS (
    SELECT 1 AS NO, 'AAA' AS NAME FROM DUAL UNION
    SELECT 2 AS NO, 'BBB' AS NAME FROM DUAL UNION
    SELECT 3 AS NO, 'CCC' AS NAME FROM DUAL
), TEMP2 AS (
    SELECT 3 AS NO, 'C@C.COM' AS MAIL FROM DUAL UNION
    SELECT 4 AS NO, 'D@D.COM' AS MAIL FROM DUAL UNION
    SELECT 5 AS NO, 'E@E.COM' AS MAIL FROM DUAL
)
SELECT NVL(TEMP1.NO, TEMP2.NO) AS NO, NAME, MAIL
FROM TEMP1 FULL JOIN TEMP2 ON TEMP1.NO = TEMP2.NO
ORDER BY TEMP1.NO
```

### 방법 2. UNION

```sql
WITH TEMP1 AS (
    SELECT 1 AS NO, 'AAA' AS NAME FROM DUAL UNION
    SELECT 2 AS NO, 'BBB' AS NAME FROM DUAL UNION
    SELECT 3 AS NO, 'CCC' AS NAME FROM DUAL
), TEMP2 AS (
    SELECT 3 AS NO, 'C@C.COM' AS MAIL FROM DUAL UNION
    SELECT 4 AS NO, 'D@D.COM' AS MAIL FROM DUAL UNION
    SELECT 5 AS NO, 'E@E.COM' AS MAIL FROM DUAL
)
SELECT NO, MAX(NAME), MAX(MAIL)
FROM (
    SELECT NO, NAME, '' AS MAIL
    FROM TEMP1
    UNION
    SELECT NO, '' AS NAME, MAIL
    FROM TEMP2
)
GROUP BY NO
ORDER BY NO
```

### 결과 예시

대상 1:

![](/images/oracle-column-merge-1.png)

대상 2:

![](/images/oracle-column-merge-2.png)

결과:

![](/images/oracle-column-merge-3.png)
