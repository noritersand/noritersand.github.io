---
layout: post
date: 2022-02-12 00:23:23 +0900
title: '[DBMS] MariaDB, MySQL: 쪽 번호 매기기 Pagination'
categories:
  - dbms
tags:
  - dbms
  - mariadb
  - mysql
  - sql
  - pagination
---

* Kramdown table of contents
{:toc .toc}

## 개요

MariaDB, MySQL에서 쓰는 페이징 쿼리 메모

## [LIMIT](https://mariadb.com/kb/en/limit/)

```
LIMIT offset, row_count

LIMIT row_count OFFSET offset
```

- `offset`: 가져오기 시작할 데이터의 인덱스
- `row_count`: 가져올 데이터의 수

`ROWNUM` 그딴 거 안써도 된다:

```sql
SELECT *
FROM SOME_TABLE
WHERE CONDITION = TRUE
ORDER BY RES_DT DESC -- 등록일시 역순
LIMIT 3 OFFSET 0 -- 첫 번째(0)부터 3개
```

`offset` 계산은 요딴식으로 하면 됨:

```java
private int calculateOffset(int pageSize, int pageNum) {
    if (pageNum <= 0) {
        return 0;
    }
    return (pageNum - 1) * pageSize;
}
```

끗.
