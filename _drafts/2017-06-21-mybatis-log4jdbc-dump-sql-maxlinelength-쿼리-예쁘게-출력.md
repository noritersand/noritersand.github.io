---
layout: post
date: 2017-06-21 14:15:00 +0900
title: 'MyBatis: log4jdbc.dump.sql.maxlinelength=0: 쿼리 예쁘게 출력'
categories:
  - java
  - mybatis
tags:
  - mybatis
  - sql mapper
---

* Kramdown table of contents
{:toc .toc}

톰캣 환경 변수 추가:

```
-Dlog4jdbc.dump.sql.maxlinelength=0
```
