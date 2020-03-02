---
layout: post
date: 2017-01-01 17:53:21 +0900
title: '[Java] regex 정규 표현식 모음'
categories:
  - java
tags:
  - java
  - regex
  - regexp
  - 코드모음
---

## String.replaceAll()

### 언더바`_` 이후의 문자 제거

```java
String str = "AA1062329819_123";
str.replaceAll("_\\w+", ""); // AA1062329819
```

## 언더바`_` 후의 문자만 추출

```java
String str = "AA1062329819_12A3";
str.replaceAll("\\w+_", ""); // 12A3
```
