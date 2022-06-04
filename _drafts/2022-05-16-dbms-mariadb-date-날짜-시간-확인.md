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

`DATE`, `DATETIME`, `TIME` 타입 관련 테스트 쿼리 걍 붙여넣음.

## 쿼리

```sql
SELECT
    DD.DUMMY,
    DATE_FORMAT(DD.DUMMY, '%Y-%m-%dT%H:%i:%s.%f') AS ISO_FORMAT_LIKE,
    DATE_FORMAT(DD.DUMMY, '%w') AS DAY,
    DATE_FORMAT(DD.DUMMY, '%W') AS DAY_STRING
FROM (
  SELECT STR_TO_DATE(D.DUMMY, '%Y-%m-%d' '%H:%i:%s') AS DUMMY /* 포맷: yyyy-MM-dd HH:mm:dd */
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
) DD;
```
