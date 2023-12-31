---
layout: post
date: 2016-08-09 11:15:00 +0900
title: '[Oracle Database] 개행문자 변환'
categories:
  - oracle
tags:
  - dbms
  - oracle
  - replace
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

```sql
REPLACE( REPLACE( DTL_DESC, CHR(10) ), CHR(13), '|')
```
