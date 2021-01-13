---
layout: post
date: 2017-03-27 13:40:00 +0900
title: '[Tomcat] webapp.root'
categories:
  - tomcat
tags:
  - was
  - tomcat
  - property
---

* Kramdown table of contents
{:toc .toc}

```bash
정보: Starting Servlet Engine: Apache Tomcat/8.5.8
6월 08, 2017 7:42:47 오후 org.apache.jasper.servlet.TldScanner scanJars
정보: At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.
6월 08, 2017 7:42:47 오후 org.apache.catalina.core.ApplicationContext log
정보: No Spring WebApplicationInitializer types detected on classpath
6월 08, 2017 7:42:47 오후 org.apache.catalina.core.ApplicationContext log
정보: Set web app root system property: 'webapp.root' = [C:\project\workspace\.metadata\.plugins\org.eclipse.wst.server.core\tmp1\wtpwebapps\frontweb\]
DEBUG: Initializing filter 'encodingFilter'
DEBUG: Filter 'encodingFilter' configured successfully
```

```java
${webapp.root}
```

이게 그냥 설정되진 않고 뭔가를 해야 되는거 같은데...
로깅 프레임웤에서 설정한다는 말도 있고.

web.xml을 쓰지 않고 @WebListener(혹은 ServletContextListener의 구현체)를 사용할땐 안되는것 같다.

톰캣 플러그인에서 띄운건 저 로그가 뜨는데, 톰캣으로 직접 띄우면 없다.

이걸 해야 하는것 같기도 하고 말이징

```xml
<context-param>
    <param-name>webAppRootKey</param-name>
    <param-value>ecbase.root</param-value>
</context-param>
```

어차피 저 값은 웹루트 경로에 해당하고 이 경로를 구하는 값이 저것만 있지 않으니 몰라도 되지 않을까? 싶어도 로그백 설정 파일에서 필요할 수 있다.

```java
String root = req.getSession().getServletContext().getRealPath("/"); // 이 값과 webapp.root는 같음
```

쏘 미스테리
