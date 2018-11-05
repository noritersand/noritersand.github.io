---
layout: post
date: 2015-03-31 18:03:00 +0900
title: 'MyBatis-Spring 마이바티스 스프링 연동'
categories:
  - mybatis
tags:
  - java
  - mybatis
  - spring
---

#### 관련 문서

- [https://mybatis.github.io/spring/ko/getting-started.html](https://mybatis.github.io/spring/ko/getting-started.html)

```java
public interface UserMapper {
    @Select("SELECT * FROM users WHERE id = #{userId}")
    User getUser(@Param("userId") String userId);
}
```

~~아닛!? 신세계로구나!~~
