---
layout: post
date: 2013-10-11 17:05:00 +0900
title: 'Oracle: comment'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - comment
---

* Kramdown table of contents
{:toc .toc}

테이블이나 컬럼에 COMMENT을 달아두면 여러모로 편리하다. Toad 같은 경우엔 컬럼에 달린 COMMENT을 이런식으로 보여주기도 한다.

![](/images/oracle-comment.png)

## COMMENT 생성

오라클에서 COMMENT 생성은 다음처럼 우선 테이블 생성 후 별도의 명령어로 생성한다:

```sql
COMMENT ON TABLE TEST IS '테이블 코멘트';
COMMENT ON COLUMN TEST.NAME IS '컬럼 코멘트';
```

참고로 MySQL은 다음처럼 테이블 생성단계에서 코멘트를 지원함:

```sql
CREATE TABLE 'T_TEST_TESTING' (
  'TEST_NO' INT(10) UNSIGNED NOT NULL AUTO_INCREMENT,
  'TEST_NM' VARCHAR2(300) NOT NULL COMMENT '컬럼 코멘트1',
  'TEST_TP' CHAR(6) NOT NULL COMMENT '컬럼 코멘트2',
);
```

## COMMENT 조회

```sql
SELECT * FROM ALL_TAB_COMMENTS WHERE TABLE_NAME = 'TEST';  -- 테이블 코멘트 조회
SELECT * FROM ALL_COL_COMMENTS WHERE TABLE_NAME = 'TEST';  -- 컬럼 코멘트 조회
```

## COMMENT 삭제

삭제는 빈문자열을 입력하는것으로 대체한다.

```sql
COMMENT ON COLUMN TEST.NAME IS '';
```
