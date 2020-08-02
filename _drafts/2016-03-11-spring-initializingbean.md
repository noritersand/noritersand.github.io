---
layout: post
date: 2016-03-11 14:46:00 +0900
title: '[Spring] InitializingBean'
categories:
  - spring
tags:
  - java
  - spring
  - initializingbean
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

-

```html
<%@ taglib prefix="spring" uri="http://www.springframework.org/tags" %>
<spring:message code="MC01604" text="코드가 없으면 노출되는 문자"/>
```

이런게 됨.

근데 jstl:fmt 와 기능이 겹치는거 같다 어째

적용하려면 다음처럼:

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.2.xsd">

    <bean id="messageSource" class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
        <property name="defaultEncoding" value="UTF-8" />
        <property name="basename" value="/WEB-INF/msg/messages" />
        <property name="fallbackToSystemLocale" value="false"/>
        <property name="cacheSeconds" value="60"/>
    </bean>

    <bean id="localeResolver" class="org.springframework.web.servlet.i18n.SessionLocaleResolver">
       <property name="defaultLocale" value="ko" />
    </bean>
</beans>
```

해주면 아래와 같은 xml을 검색하는 모양:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
<comment>dictionary KR</comment>
<entry key="MC01604">상품담당자</entry>
<entry key="MC01603">배송담당자</entry>
```
