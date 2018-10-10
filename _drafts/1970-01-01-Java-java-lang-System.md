---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'Java: java.lang.System'
categories:
  - java
tags:
  - java
  - byte
---

#### 참고한 글
- [http://docs.oracle.com/javase/9/docs/api/java/lang/System.html](http://docs.oracle.com/javase/9/docs/api/java/lang/System.html)

## getProperty()
```java
System.getProperty("java.version");
System.getProperty("user.language");
```
system arguments(eclipse.ini 혹은 tomcat Arguments로 지정된 값들)를 이 메서드로 조회할 수 있음.

## getProperties()
```
file.separator : \
java.class.path : C:\Users\fixalot\work\workspace\image\build\class...
java.home : C:\myapps\Java\jre7
java.vendor : Oracle Corporation
java.vendor.url : http://java.oracle.com/
java.version : 1.7.0_51
line.separator :
file.encoding=UTF-8 // platform character set

os.arch : amd64
os.name : Windows 7
os.version : 6.1
path.separator : ;
user.dir : C:\Users\fixalot\work\workspace\image
user.home : C:\Users\fixalot
user.name : fixalot
// 생략
```

## getEnv()
환경설정 값 가져오기
```java
System.getenv("path");
```

## currentTimeMillis()와 nanoTime()의 차이
[http://mussebio.blogspot.kr/2012/05/java-api.html](http://mussebio.blogspot.kr/2012/05/java-api.html)
