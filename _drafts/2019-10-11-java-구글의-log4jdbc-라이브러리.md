---
layout: post
date: 2019-10-11 14:59:00 +0900
title: 'Java: 구글의 log4jdbc 라이브러리'
categories:
  - etc
tags:
  - tag me
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [somewhere](somewhere)

com.googlecode의 log4jdbc 라이브러리를 사용할 때는 아래처럼:

```xml
<dependency>
    <groupId>com.googlecode.log4jdbc</groupId>
    <artifactId>log4jdbc</artifactId>
    <version>1.2</version>
    <exclusions>
        <exclusion>
            <artifactId>slf4j-api</artifactId>
            <groupId>org.slf4j</groupId>
        </exclusion>
    </exclusions>
</dependency>
```

slf4j-api를 사용하지 않도록 설정해야 레거시를 무시한다. 왜 그러는지는 아직 몲.
