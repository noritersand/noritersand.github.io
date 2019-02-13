---
layout: post
date: 2017-06-21 14:15:00 +0900
title: 'MyBatis: log4jdbc.dump.sql.maxlinelength=0: 쿼리 예쁘게 출력'
categories:
  - mybatis
tags:
  - java
  - mybatis
  - sql
  - log
---

* Kramdown table of contents
{:toc .toc}

톰캣의 JVM 프로퍼티 추가:

```bash
-Dlog4jdbc.dump.sql.maxlinelength=0
```
