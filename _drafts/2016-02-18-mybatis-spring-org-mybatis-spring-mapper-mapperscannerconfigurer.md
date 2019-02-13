---
layout: post
date: 2016-02-18 17:25:00 +0900
title: 'MyBatis-Spring: org.mybatis.spring.mapper.MapperScannerConfigurer'
categories:
  - mybatis
tags:
  - java
  - mybatis
  - spring
  - mapperscannerconfigurer
---

* Kramdown table of contents
{:toc .toc}

다음처럼 설정하면:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.2.xsd">

    <bean id="dataSource" class="org.springframework.jdbc.datasource.SimpleDriverDataSource">
        <property name="driverClass" value="net.sf.log4jdbc.DriverSpy" />
        <property name="url" value="jdbc:log4jdbc:oracle:thin:@27.1.10.238:1521:icdb" />
        <property name="username" value="icuser" />
        <property name="password" value="icuser" />
    </bean>

    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="configLocation" value="/WEB-INF/config/mybatis/mybatis-config.xml"></property>
        <property name="mapperLocations">
            <array>
                <value>classpath*:/com/ecbase/sql/**/*.xml</value>
            </array>
        </property>
    </bean>

...

    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionTemplate">
        <constructor-arg ref="sqlSessionFactory"></constructor-arg>
    </bean>

    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.ecbase.dao" />
    </bean>
</beans>
```

인터페이스만 있어도:

```java
public interface MemberDao {
    public PaginatedList<MemberBaseModel> getMemberList(MemberSearchParam memberSearchParam);

...
```

요놈에 접근 가능:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ecbase.dao.member.MemberDao">
    <select id ="getMemberList" parameterType="com.ecbase.param.member.MemberSearchParam" resultMap="baseResultMap">
        SELECT  PAGE_T.*
              , MOBL_DIV_NO || MOBL_TXNO || MOBL_ENDNO AS mobileNumber
              , TEL_RGNO || TEL_TXNO || TEL_ENDNO AS telephoneNumber
              , POST_ADDR || ' ' || DTL_ADDR AS address

...
```

~~이 때 parameterType은 생략할 수 있다. (resultType은 생략하면 에러남) 원래 생략 가능한건지 확인할 것. ~~

마이바티스는 원래 parameterType 생략 가능
