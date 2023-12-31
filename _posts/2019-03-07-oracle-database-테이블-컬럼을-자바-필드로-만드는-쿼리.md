---
layout: post
date: 2019-03-07 15:57:00 +0900
title: '[Oracle Database] 테이블 컬럼을 자바 필드로 만드는 쿼리'
categories:
  - dbms
tags:
  - dbms
  - oracle
  - java-beans
  - plain-object
  - generator
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

## 테이블 컬럼을 자바 필드로

```sql
SELECT
/*A.TABLE_NAME, A.COLUMN_NAME, B.COMMENTS, A.DATA_TYPE,*/
  '/**' || B.COMMENTS || '*/' || 'private '
  || CASE DATA_TYPE
    WHEN 'NUMBER' THEN 'Integer'
    WHEN 'DATE' THEN 'Date'
    ELSE 'String'
  END
  || ' ' || A.COLUMN_NAME || '; // ' || A.DATA_TYPE || '('
  || CASE A.DATA_TYPE
    WHEN 'NUMBER' THEN A.DATA_PRECISION || ',' || A.DATA_SCALE
    ELSE TO_CHAR(A.DATA_LENGTH)
  END
  || ')' AS "FIELD"
FROM ALL_TAB_COLUMNS A
JOIN ALL_COL_COMMENTS B ON A.COLUMN_NAME = B.COLUMN_NAME AND A.TABLE_NAME = B.TABLE_NAME
WHERE A.OWNER = 'SOME_OWNER'
AND A.TABLE_NAME = 'SOME_TABLE'
ORDER BY COLUMN_ID;
```

쿼리로 목록 추출한 다음 할 일:

- 줄 바꿈
- 편집기로 카멜케이스 변환
- 자바 데이터 타입 변경

## 곁다리: 컬럼 코멘트를 열 머리글(별칭)로

```sql
SELECT
    "COLS".COLUMN_NAME || ' AS "' || "COMM".COMMENTS || '",'
FROM ALL_TAB_COLUMNS "COLS"
JOIN ALL_COL_COMMENTS "COMM" ON "COLS".TABLE_NAME = "COMM".TABLE_NAME AND "COLS".COLUMN_NAME = "COMM".COLUMN_NAME
WHERE "COLS".TABLE_NAME = 'SOME_TABLE'
ORDER BY "COLS".COLUMN_ID;
```
