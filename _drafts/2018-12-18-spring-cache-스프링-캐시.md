---
layout: post
date: 2018-12-18 10:48:00 +0900
title: '[Spring] Cache 스프링 캐시'
categories:
  - spring
tags:
  - java
  - spring
  - spring-boot
  - cache
  - tag me
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://spring.io/guides/gs/caching/](https://spring.io/guides/gs/caching/)
- [https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-caching.html](https://docs.spring.io/spring-boot/docs/current/reference/html/boot-features-caching.html)
- [https://www.baeldung.com/spring-cache-tutorial](https://www.baeldung.com/spring-cache-tutorial)
- [https://www.logicbig.com/tutorials/spring-framework/spring-integration/cache-evict.html](https://www.logicbig.com/tutorials/spring-framework/spring-integration/cache-evict.html)

@Cacheable

@CacheConfig

@CacheEvict

```java
@AutoWired @Qualifier("defaultCacheManager")
private CacheManager defaultCacheManager;
```
