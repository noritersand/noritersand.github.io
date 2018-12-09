---
layout: post
date: 2016-02-12 17:31:00 +0900
title: 'Spring: Spring tag library'
categories:
  - java
  - spring
tags:
  - java
  - spring
  - tag
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

-

```java
public class CodeUtil implements InitializingBean {
//    @Autowired
//    CommonCodeUtilService commonCodeUtilService;

    @Override
    public void afterPropertiesSet() throws Exception {
        // initialize
    }

    // 생략
```

`afterPropertiesSet()`: 스프링에 의해 Bean이 초기화 된 이후 실행되는 메서드.

아래는 dispatcher-servlet의 일부

```xml
    <bean name="CodeUtil" class="framework.util.beans.CodeUtil"/>

    <!-- 혹은 -->

    <bean name="CodeUtil" class="framework.util.beans.CodeUtil">
        <property name="prop1" ref="value1"/>
    </bean>
```

아니면 그냥

```xml
<bean name="CodeUtil" class="framework.util.beans.CodeUtil" init-method="init"/>
```
