---
layout: post
date: 2016-11-21 00:42:00 +0900
title: '[Servlet] web.xml 없이 서블릿 설정하기'
categories:
  - servlet
tags:
  - java
  - servlet
  - servletcontextlistener
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [somewhere](somewhere)

서블릿 3.0부터는 `web.xml` 없이 서블릿을 설정할 수 있는 방법을 제공한다.

TODO 블라블라

## ServletContextListener

**근데 이건 2.3부터 되는것 같은데?**

웹 앱을 WAR로 개발할 경우엔 다음처럼 `ServletContextListener`를 구체화한 클래스를 작성하면 된다:

```java
package com.test;

import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.ServletRegistration.Dynamic;
import javax.servlet.annotation.WebListener;

@WebListener
public class WebAppInitializer implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent event) {
        Dynamic registration = event.getServletContext().addServlet("servlet", new ServletTest());
        registration.addMapping("*.do");
        registration.setLoadOnStartup(1);
    }

    @Override
    public void contextDestroyed(ServletContextEvent event) {
        // do some when tomcat shutdown
    }
}
```

위의 자바 코드는 (이전 방식인)아래의 `web.xml`과 동일하게 작동한다:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns="http://xmlns.jcp.org/xml/ns/javaee"
    xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
    id="WebApp_ID" version="3.1">
    <display-name>test</display-name>
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

    <servlet>
        <servlet-name>servlet</servlet-name>
        <servlet-class>com.test.ServletTest</servlet-class>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>servlet</servlet-name>
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
</web-app>
```

## ServletContainerInitializer

이런 것도 있는데, 클래스패스 내의 `javax.servlet.ServletContainerInitializer` 인터페이스를 구체화한 모든 클래스들을 찾아보도록 되어 있다. 

```java
package com.test;

import java.util.Set;

import javax.servlet.ServletContainerInitializer;
import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.ServletRegistration.Dynamic;
import javax.servlet.annotation.HandlesTypes;

@HandlesTypes({ServletTest.class})
public class WebAppInitializer implements ServletContainerInitializer {
    @Override
    public void onStartup(Set> arg0, ServletContext context) throws ServletException {
        Dynamic registration = context.addServlet("servlet", new ServletTest());
        registration.addMapping("*.do");
        registration.setLoadOnStartup(1);
    }
}
```

참고할 것:

- [http://goodcodes.tistory.com/entry/JSP-ServletContainerInitializer](http://goodcodes.tistory.com/entry/JSP-ServletContainerInitializer)
- [http://stackoverflow.com/questions/39218951/map-servlet-programmatically-instead-of-using-web-xml-or-annotations/](http://stackoverflow.com/questions/39218951/map-servlet-programmatically-instead-of-using-web-xml-or-annotations/)
