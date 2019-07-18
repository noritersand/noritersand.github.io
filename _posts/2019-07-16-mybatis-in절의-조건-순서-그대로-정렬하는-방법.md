---
layout: post
date: 2019-07-16 16:23:00 +0900
title: 'Mybatis: IN절의 조건 순서 그대로 정렬하는 방법'
categories:
  - mybatis
tags:
  - java
  - mybatis
  - dbms
---

* Kramdown table of contents
{:toc .toc}

### MySQL 혹은 MariaDB

```xml
SELECT PRD_ID
FROM   PRODUCT
WHERE PRD_ID IN (<foreach item="item" collection="prdIds" separator=",">#{item}</foreach>)
ORDER BY FIELD(PRD_ID, <foreach item="item" collection="prdIds" separator=",">#{item}</foreach>)
```

### Oracle

```xml
SELECT PRD_ID
FROM   PRODUCT
WHERE PRD_ID IN (<foreach item="item" collection="prdIds" separator=",">#{item}</foreach>)
ORDER BY CASE PRD_ID <foreach item="item" collection="prdIds" separator=" " index="index">WHEN #{item} THEN #{index}</foreach> END
```

끗.
