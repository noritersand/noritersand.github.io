---
layout: post
date: 2015-07-23 23:37:00 +0900
title: '[Oracle] merge'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - merge
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://docs.oracle.com/cd/B28359_01/server.111/b28286/statements_9016.htm#SQLRF01606](https://docs.oracle.com/cd/B28359_01/server.111/b28286/statements_9016.htm#SQLRF01606)
- https://ko.wikipedia.org/wiki/Merge_(SQL)

### syntax

```
MERGE INTO 메인-테이블 USING 서브-테이블 ON (조건)
WHEN MATCHED THEN
  UPDATE SET 컬럼1 = 값1 [, 컬럼2 = 값2 ...]
WHEN NOT MATCHED THEN
  INSERT (컬럼1 [, 컬럼2 ...]) VALUES (값1 [, 값2 ...])
```

UPDATE문에서 WHERE절은 생략할 수 있다. WHERE절을 생략하면 UPDATE문 실행 시 USING-ON의 조건을 그대로 적용하고, WHERE절을 명시하면 USING-ON의 조건에 추가로 조건을 더하는 방식이다.

### example

#### MEMBER 테이블에 회원번호가 10인 데이터가 있으면 UPDATE, 없으면 INSERT

```sql
MERGE INTO MEMBER
USING DUAL -- 단일 테이블일 때 DUAL 사용
ON (MBR_NO = 10) -- 회원번호가 10인 데이터가 있는지
WHEN MATCHED THEN -- 있을경우 UPDATE
  UPDATE
  SET MOD_DTIME = SYSDATE
WHEN NOT MATCHED THEN -- 없으면 INSERT
  INSERT (
    MBR_NO,
    REG_DTIME,
    MOD_DTIME
  ) VALUES (
    10,
    SYSDATE,
    SYSDATE
  )
```
