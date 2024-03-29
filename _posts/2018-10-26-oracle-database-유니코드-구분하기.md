---
layout: post
date: 2018-10-26 15:40:00 +0900
title: '[Oracle Database] 유니코드 구분하기'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - sql
  - code-snippet
---

```sql
SELECT
    WORD,
    ASCIISTR(WORD) AS "TO ASCII",
    LENGTH(ASCIISTR(WORD)) AS "LENGTH OF ASCII",
    CASE
        WHEN LENGTH(ASCIISTR(WORD)) > LENGTH(WORD) THEN 'IS UNICODE'
        ELSE 'IS ASCII'
    END AS "RESULT"
FROM (
    SELECT 'ABCD' WORD FROM DUAL
    UNION
    SELECT '홀롤롤로' WORD FROM DUAL
);
```

ASCIISTR 함수는 Non-ASCII 문자를 `\xxxx` 형식의 UTF-16 코드로 반환한다. 이를 이용해서 유니코드인지 아스키인지 구분하는 쿼리.
