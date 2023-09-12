---
layout: post
date: 2022-02-12 00:23:23 +0900
title: '[MariaDB] MariaDB: 쪽 번호 매기기'
categories:
  - mariadb
tags:
  - mariadb
  - dbms
  - mysql
  - sql
  - pagination
---

* Kramdown table of contents
{:toc .toc}

#### 테스트 환경 정보

- MariaDB 10.5.17
- MySQL 8.0.25


## 개요

MariaDB, MySQL에서 쓰는 페이징 쿼리 저장


## [LIMIT](https://mariadb.com/kb/en/limit/)

```
LIMIT offset, row_count

LIMIT row_count OFFSET offset
```

- `offset`: 가져오기 시작할 데이터의 인덱스
- `row_count`: 가져올 데이터의 수

`ROWNUM` 안써도 됨:

```sql
select *
from some_table
where condition = true
order by res_dt desc -- 등록일시 역순
limit 3 offset 0 -- 첫 번째(0)부터 3개
```

만약 전체 데이터 개수로 뭔가를 해야 하면 `COUNT() OVER()`를 같이 쓰면 됨:

```sql
select
  count(1) over() as entirerows,
  st.*
from some_table st
limit 5 offset 10 -- 열한 번째부터 5개
```


## 계산식

OFFSET 계산은 요딴식으로:

```java
/** OFFSET 계산 */
calculateOffset(pageSize, pageNum) {
    if (pageNum <= 1) {
        return 0;
    }
    return (pageNum - 1) * pageSize;
}

/** 남은 로우의 수 계산 */
getRestRows(entireDataCount, pageSize, pageNum) {
    if (pageNum <= 0) {
        throw new IllegalArgumentException("pageNum must be greater than 0");
    }
    rest = entireDataCount - pageSize * pageNum;
    return rest > 0 ? rest : 0;
}

/** 다음 페이지 번호 계산 */
getNextPageNum(entireDataCount, pageSize, currentPageNum) {
    if (currentPageNum <= 0) {
        throw new IllegalArgumentException("currentPageNum must be greater than 0");
    }
    rest = getRestRows(entireDataCount, pageSize, currentPageNum);
    return rest > 0 ? currentPageNum + 1 : 0;
}
```

끗.
