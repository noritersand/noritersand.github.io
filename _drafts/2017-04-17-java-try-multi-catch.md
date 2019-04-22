---
layout: post
date: 2017-04-17 15:08:00 +0900
title: 'Java: try-multi-catch'
categories:
  - java
tags:
  - java
  - try catch
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [somewhere](/somewhere)


JDK 몇 부터 추가된거더라?

```java
try {
	// do something normal
} catch (NotFoundException | IOException e) {
	log.error(e.getMessage(), e);
}
```
