---
layout: post
date: 2016-12-23 13:00:00 +0900
title: 'JSP: 커스텀 태그 Custom tags'
categories:
  - java
  - jsp
tags:
  - java
  - jsp
  - custom tags
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://docs.oracle.com/javaee/5/tutorial/doc/bnalj.html](http://docs.oracle.com/javaee/5/tutorial/doc/bnalj.html)
- [http://gangzzang.tistory.com/entry/JSP-%EC%BB%A4%EC%8A%A4%ED%85%80-%ED%83%9C%EA%B7%B8Custom-Tag](http://gangzzang.tistory.com/entry/JSP-%EC%BB%A4%EC%8A%A4%ED%85%80-%ED%83%9C%EA%B7%B8Custom-Tag)


커스텀 태그는 JSP 스펙 2.0부터 지원되는 기능이다. JSTL이 기본으로 제공되는 표준 태그 라이브러리라면, 이쪽은 사용자가 만드는 태그 라이브러리라고 보면 된다.

커스텀 태그를 적용하는 방법은 세 가지가 있는데 첫 번째는 JSP taglib 디렉티브에서 tagdir 속성으로 설정하는 방법, 두 번째는 JSP taglib 디렉티브에서 uri 속성으로 설정하는 방법  세 번째는 web.xml에서 jsp-config 항목으로 설정하는 방법이 있다.

## taglib 디렉티브의 tagDir로 설정

지정된 폴더 아래에 있는 확장자가 tag인 파일을 커스텀 태그로 사용한다.

JSP 파일 상단에 디렉티브를 선언하고:

```
<%@ taglib prefix="접두어" tagdir="태그파일경로" %>
```

HTML 본문 내에 태그파일이 위치할 곳에 `<tags>`를 명시한다:

```
<접두어:태그파일명 />
```

#### index.jsp

```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="tags" tagdir="/WEB-INF/tags" %>
<!DOCTYPE>
<html>
<head>
<meta charset="UTF-8">
<title>custom tags test</title>
</head>
<body>
<tags:example />
</body>
</html>
```

#### example.tag

```html
<%@ tag language="java" pageEncoding="UTF-8"%>
<h2>hello world!</h2>
```

### `<jsp:include>`와의 차이점

파싱된 java 파일을 보면 `<tags>`는 다음처럼:

```java
if (_jspx_meth_tags_005fexample_005f0(_jspx_page_context))
    return;
```

`<jsp:include>`는 다음처럼 되어있다:

```java
org.apache.jasper.runtime.JspRuntimeLibrary.include(request,
        response, "/WEB-INF/tags/example.tag", out, false);
```

### tag 파일에 속성 전달

#### 컨트롤러

```java
private void process(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    req.setAttribute("attr", "이야아아");
    req.getRequestDispatcher("/WEB-INF/jsp/" + uri + ".jsp").forward(req, resp);
}
```

#### jsp

```html
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="tags" tagdir="/WEB-INF/tags" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<h2>HEL로 world드</h2>

<c:set var="attr2" value="56778" scope="request" />

<tags:example tagattr="one" />
<tags:example tagattr="two" />
</body>
</html>
```

#### tag

```html
<%@ tag language="java" pageEncoding="UTF-8" %>
<%@ attribute name="tagattr" %>
<h3>난 태그라오</h3>
<p>jsp에서 설정한 속성: ${tagattr}</p>
<p>컨트롤러에서 설정한 리퀘스트 속성: ${attr}</p>
<p>jsp에서 설정한 리퀘스트 속성: ${attr2}</p>
```

## taglib 디렉티브의 uri로 설정

방법설명 TODO

## web.xml의 jsp-config로 설정

방법설명 TODO
