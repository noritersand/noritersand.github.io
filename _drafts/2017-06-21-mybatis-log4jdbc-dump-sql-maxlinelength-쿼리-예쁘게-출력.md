---
layout: post
date: 2017-06-21 14:15:00 +0900
title: '[Mybatis] log4jdbc.dump.sql.maxlinelength=0: 쿼리 예쁘게 출력'
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

이거 그냥 되는게 아니고, 로거 중에 자동으로 줄을 바꿔버리는걸 단순히 한 라인에 모두 나오도록 하는 설정이기 때문에 근본적인 해결이 아니며 로거에 따라 소용이 없을 수도 있음.
