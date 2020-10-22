---
layout: post
date: 2017-06-12 21:31:00 +0900
title: '[Java] classpth* vs classpath'
categories:
  - java
tags:
  - java
  - classpath
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://blog.carbonfive.com/2007/05/17/using-classpath-vs-classpath-when-loading-spring-resources/](http://blog.carbonfive.com/2007/05/17/using-classpath-vs-classpath-when-loading-spring-resources/)
- [https://stackoverflow.com/questions/3294423/spring-classpath-prefix-difference](https://stackoverflow.com/questions/3294423/spring-classpath-prefix-difference)
- [https://okky.kr/article/286428](https://okky.kr/article/286428)


#### classpath

프로젝트가 실행 되었을 때 현재 classloader에 해당하는 경로의 리소스만 선택

#### classpath*

프로젝트가 실행 되었을 때 현재 classloader의 경로 뿐만 아니라 상위 classloader를 모두 검색하여 해당 경로의 리소스를 선택

#### class loader

클래스 로더는 하나가 아니며 로더마다 기준이 되는 클래스 패스는 달라질 수 있다.
