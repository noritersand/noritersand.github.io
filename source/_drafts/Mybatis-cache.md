---
title: 'Mybatis: cache'
categories:
  - java
  - mybatis
tags:
  - todo
  - mybatis
---

flushInterval을 명시하지 않았을 때 한 번 캐싱된 쿼리 결과는 flushCache가 true인 쿼리가 호출될 때까지 flush를 하지 않는다.
그리고 일단 flush가 트리거 되면 모든 쿼리의 캐시가 날아간다.
