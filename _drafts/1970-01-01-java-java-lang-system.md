---
layout: post
date: 1970-01-01 00:00:00 +0900
title: '[Java] java.lang.System'
categories:
  - java
tags:
  - java
  - system
---

#### 참고한 문서

- [http://docs.oracle.com/javase/9/docs/api/java/lang/System.html](http://docs.oracle.com/javase/9/docs/api/java/lang/System.html)

## getProperty(String)

```java
System.getProperty("java.version");
System.getProperty("user.language");
```

system arguments(eclipse.ini 혹은 tomcat Arguments로 지정된 값들)를 이 메서드로 조회할 수 있음.

## getProperties()

```java
Properties props = System.getProperties();
Enumeration<?> names = props.propertyNames();
while (names.hasMoreElements()) {
    final String propname = (String) names.nextElement();
    logger.debug("{}={}", propname, props.getProperty(propname));
}
```

## getenv(String)

특정 환경설정 가져오기

```java
System.getenv("path");
```

## getenv()

모든 환경설정 값 가져오기

```java
Map<String, String> env = System.getenv();
Set<String> keySet = env.keySet();
for (String envname : keySet) {
    logger.debug("{}={}", envname, env.get(envname));
}
```

## currentTimeMillis()와 nanoTime()의 차이

[http://mussebio.blogspot.kr/2012/05/java-api.html](http://mussebio.blogspot.kr/2012/05/java-api.html)

## load(String)

네이티브 라이브러리 로딩: 파일명으로 지정

```java
System.load("c:/some-file-name");
```

## loadLibrary(String)

네이티브 라이브러리 로딩: 라이브러리 이름으로 지정

```java
System.loadLibrary("some-library-name");
```
