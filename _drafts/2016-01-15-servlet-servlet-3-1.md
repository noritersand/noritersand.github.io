---
layout: post
date: 2016-01-15 13:55:00 +0900
title: '[Servlet] Servlet 3.1'
categories:
  - servlet
tags:
  - java
  - servlet
  - tomcat
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://tomcat.apache.org/tomcat-8.0-doc/index.html](http://tomcat.apache.org/tomcat-8.0-doc/index.html)
- [http://tomcat.apache.org/tomcat-8.0-doc/servletapi/index.html](http://tomcat.apache.org/tomcat-8.0-doc/servletapi/index.html)
- [http://start.goodtime.co.kr/2014/02/%ED%86%B0%EC%BA%A3-8-%EC%86%8C%EA%B0%9C/](http://start.goodtime.co.kr/2014/02/%ED%86%B0%EC%BA%A3-8-%EC%86%8C%EA%B0%9C/)

![](/images/tomcat-which-version.png)

이미지 출처: [http://tomcat.apache.org/whichversion.html](http://tomcat.apache.org/whichversion.html)

다른 얘기지만 스프링 3.2부터 사용 가능한 `AbstractAnnotationConfigDispatcherServletInitializer`는 `Dispatcher-servlet.xml`을 대체하는 자바 코드 기반의 스프링 설정파일인데 자바코드로 서블릿을 설정할 때 사용하며 `ServletContainerInitializer`(정확히는 `WebApplicationInitializer`)를 구체화하고 있다.
