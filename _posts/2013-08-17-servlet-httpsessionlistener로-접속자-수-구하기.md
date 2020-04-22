---
layout: post
date: 2013-08-17 03:08:00 +0900
title: '[Servlet] HttpSessionListener로 접속자 수 구하기'
categories:
  - servlet
tags:
  - java
  - servlet
  - httpsessionlistener
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

### web.xml

```xml
<listener>
    <listener-class>com.util.CountManager</listener-class>
</listener>
```

### servlet

```java
public class CountManager implements HttpSessionListener {
    public static int count;

    public static int getCount() {
        return count;
    }

    public void sessionCreated(HttpSessionEvent event) {
        //세션이 만들어질 때 호출
        HttpSession session = event.getSession(); //request에서 얻는 session과 동일한 객체
        session.setMaxInactiveInterval(60*20);

        count++;

        session.getServletContext().log(session.getId() + " 세션생성 " + ", 접속자수 : " + count);
    }

    public void sessionDestroyed(HttpSessionEvent event) {
        //세션이 소멸될 때 호출
        count--;
        if(count<0)
            count=0;

        HttpSession session = event.getSession();
        session.getServletContext().log(session.getId() + " 세션소멸 " + ", 접속자수 : " + count);
    }
}
