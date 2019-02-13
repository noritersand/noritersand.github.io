---
layout: post
date: 2013-08-16 04:29:00 +0900
title: 'Servlet: ServletConfig, ServletContext'
categories:
  - servlet
tags:
  - java
  - servlet
  - ServletConfig
  - ServletContext
---

* Kramdown table of contents
{:toc .toc}

ServletConfig와 ServletContext는 web.xml 문서를 조작하여 서블릿에 정보를 전달하기 위한 용도로 사용되는 인터페이스다.

#### ServletConfig

지정된 서블릿에서만 사용할 수 있다.

```xml
<servlet>
    <servlet-name> </servlet-name>
    <servlet-class> </servlet-class>
    <init-param>
        <param-name> </param-name>
        <param-value> </param-value>
    </init-param>
</servlet>
```

#### ServletContext

동일 웹 애플리케이션 내 모든 서블릿에서 사용 할 수 있다.

```xml
<context-param>
    <param-name> </param-name>
    <param-value> </param-value>
</context-param>
```

## example

```xml
<servlet>
    <servlet-name>test</servlet-name>
    <servlet-class>com.test1.TestServlet</servlet-class>
    <init-param>
        <param-name>name</param-name>
        <param-value>han</param-value>
    </init-param>
    <init-param>
        <param-name>age</param-name>
        <param-value>20</param-value>
    </init-param>
</servlet>

<servlet-mapping>
    <servlet-name>test</servlet-name>
    <url-pattern>/test</url-pattern>
</servlet-mapping>

<context-param>
    <param-name>city</param-name>
    <param-value>seoul</param-value>
</context-param>
```

```java
protected void process(HttpServletRequest req, HttpServletResponse resp)
        throws ServletException, IOException {
    // getServletConfig는 GenericServlet의 메서드다.
    ServletConfig config = getServletConfig(); // 해당 서블릿에서만 사용가능
    ServletContext context = getServletContext(); // 동일한 웹어플리케이션 어디서든 접근가능

    String name = config.getInitParameter("name");
    String age = config.getInitParameter("age");

    String city = context.getInitParameter("city");

          //로그설정
    context.log("로그를 출력합니드아");

    resp.setContentType("text/html;charset=utf-8");
    PrintWriter out = resp.getWriter();

    out.print("name:" + name + "<br/>");
    out.print("age:" + age + "<br/>");
    out.print("city:" + city + "<br/>");
}
```
