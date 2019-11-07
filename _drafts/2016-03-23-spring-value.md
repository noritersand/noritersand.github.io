---
layout: post
date: 2016-03-23 14:09value:00 +0900
title: '[Spring] @Value'
categories:
  - spring
tags:
  - java
  - spring
  - value
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/beans/factory/annotation/Value.html

context.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
    xmlns:p="http://www.springframework.org/schema/p" xmlns:util="http://www.springframework.org/schema/util"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.1.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.1.xsd
        http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.1.xsd">

    <util:properties id="infoProp" location="classpath:/info.xml"></util:properties>
</beans>
```

info.xml

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <comment>@value sample</comment>
    <entry key="shopinfo.s1.postNumber">11111</entry>
</properties>
```

ResourceTest.java

```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import static org.junit.Assert.*;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("/test-resource-context.xml")
public class ResourceTest {
    @Value("#{infoProp['shopinfo.s1.postNumber']}")
    private String shopProvinceName;

    @Test
    public void test(){
        assertEquals(shopProvinceName,"11111");
    }
}
```
