---
layout: post
date: 2019-02-20 19:36:00 +0900
title: '[Mybatis] 쿼리 매퍼에서 boolean 타입으로 받기(?)'
categories:
  - mybatis
tags:
  - java
  - mybatis
  - boolean
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [somewhere](somewhere)

**TODO**: 제목 이상하니 수정할 것.

그냥 요딴식으로:

```xml
  <!-- 중략 -->
  <result property="allowNotification" column="allowNotification" />
  <!-- 중략 -->
```

```sql
SELECT
  CASE some-column
    WHEN 'some-condition' THEN 1
    ELSE 0
  END AS "allowNotification"
FROM DUAL
```

1 혹은 0을 반환하면 boolean으로 자동 매핑된다.

끗.
