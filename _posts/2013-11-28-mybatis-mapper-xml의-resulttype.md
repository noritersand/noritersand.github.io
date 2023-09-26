---
layout: post
date: 2013-11-28 12:13:00 +0900
title: '[Mybatis] Mapper XML의 resultType'
categories:
  - mybatis
tags:
  - java
  - mybatis
  - sql-mapper
  - resulttype
---

* Kramdown table of contents
{:toc .toc}

#### 결과값이 하나일 경우

```xml
<select id="selectUsers" parameterType="int" resultType="java.lang.String">
  select username
  from some_table
  where id = #{id}
</select>
```

#### 결과값이 여러 개일 경우 - HashMap

```xml
<select id="selectUsers" parameterType="int" resultType="java.util.HashMap">
  select id, username, hashedPassword
  from some_table
  where id = #{id}
</select>
```

#### 결과값이 여러 개일 경우 - Plain Object

```xml
<select id="selectUsers" parameterType="int" resultType="com.someapp.model.User">
  select id, username, hashedPassword
  from some_table
  where id = #{id}
</select>
```

#### TypeAliases

```xml
<!-- In Config XML file -->
<typeAlias type="com.someapp.model.User" alias="User"/>

<!-- In SQL Mapping XML file -->
<select id="selectUsers" parameterType="int" resultType="User">
  select id, username, hashedPassword
  from some_table
  where id = #{id}
</select>
```
