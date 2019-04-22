---
layout: post
date: 2017-08-09 15:30:00 +0900
title: 'Spring: mvc:default-servlet-handler'
categories:
  - spring
tags:
  - java
  - spring
  - defaultservlethandler
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#mvc-default-servlet-handler](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#mvc-default-servlet-handler)
- [http://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/DefaultServletHttpRequestHandler.html](http://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/resource/DefaultServletHttpRequestHandler.html)

개발자가 작성한 서블릿에 매핑된 URL이 아닐때 정적 자원(css, js 등)을 처리하는 기본 서블릿으로 요청을 넘기는 설정. 아주 간단하게 확장자 없는 URL을 매핑할 수 있다.

## 적용 예시

### web.xml

```xml
<servlet-mapping>
    <servlet-name>testServlet</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

### dispatcher-servlet.xml

```xml
<mvc:default-servlet-handler />
```

## 주의사항

단, 이 설정을 사용할 경우 jsp 액션태그의 page 속성값으로 정적 자원을 지정할 수 없다. 예를 들어:

```html
<jsp:include page="a.html" />
```

이렇게 하면 예외가 발생한다. (java.io.IOException: Stream closed)
