---
layout: post
date: 2013-08-14 04:41:00 +0900
title: '[JSP] JSP, Java Server Page 기본'
categories:
  - jsp
tags:
  - java
  - jsp
  - basic
---

* Kramdown table of contents
{:toc .toc}

## comment, 코멘트

HTML 코멘트: 클라이언트로 전달된다.

```html
<!-- 코멘트 내용 -->
```

JSP 코멘트: 클라이언트로 전달하지 않는다.

```java
<%-- 코멘트 내용 --%>
```

스크립릿에서의 코멘트:

```java
<%
    /** 코멘트 내용 */
    /* 코멘트 내용 */
    // 코멘트 내용
%>
```

## Directive, 디렉티브(지시어)

디렉티브는 페이지에 대한 설정 정보를 지정하거나 클래스의 속성을 변경한다.

```java
<%@ 디렉티브_이름 속성1="값1" 속성2="값2" ... %>
```

### page 디렉티브와 속성

page는 페이지에 대한 기본 정보 입력(생성하는 문서의 타입, 출력 버퍼의 크기, 에러 페이지등. 현재 문서를 나타내는 객체다.

- `language`: 스크립트 코드에서 사용되는 프로그래밍 언어 지정. 기본 값은 "java"
- `contentType`: 생성할 문서 타입. 기본 값: "text/html" 여기서 지정하는 charset은 HTTP 응답 케릭터셋을 의미한다.
- `import`: 사용할 자바 클래스 지정
- `session`: 세션 사용 여부 지정("true": 사용, "false": 미사용). 기본 값은 "true"
- `buffer`: 출력 버퍼 크기 지정("none": 미사용, "12kb": 출력 버퍼 12kb 사용). 기본 값은 "8kb"
- `autoFlush`: 출력 버퍼가 다 찼을 경우 자동으로 버퍼에 있는 데이터를 출력 스트림에 보내고 비울지의 여부. true일 땐 버퍼의 내용을 웹 브라우저에 보 낸 후 버퍼 비우고 false는 에러를 발생시킨다. 기본값은 true
- `info`: 페이지에 대한 설명
- `errorPage`: 실행 도중 에러 발생 시 보여줄 페이지 지정 `<%@ page errorPage="e.jsp"%>`
- `isErrorPage`: 현재 패이지가 에러가 발생할 때 보여 지는 페이지 인지의 여부("true": 에러, "false": 에러아님). 기본 값은 false
- `pageEncoding`: JSP 파일 자체의 캐릭터 인코딩 지정
- `isELIgnored`: JSP 2.0에 새롭게 추가된 내용 ("true": EL 무시, "false": EL 사용). 기본 값: web.xml 파일이 사용하는 JSP 버전 및 설정에 따라 다름
- `extends`: JSP 페이지가 Servlet 소스로 변환되는 시점에서 자신이 상속 받을 클래스를 지정할때 사용. 거의 사용되지 않는 속성
- `isThreadSafe`: 하나의 JSP 페이지가 동시에 여러 브라우저의 요청을 처리할 수 있는지 여부를설정. 기본 값은 true
- `trimDirectiveWhitespaces`: JSP 2.1 버전에서는 page 디렉티브에 새롭게 추가된 속성으로 불필요하게 생성되는 줄 바꿈 공백 문자를 제거 할 수 있다. 기본 값은 false

### 주로 쓰이는 설정

```java
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"
        trimDirectiveWhitespaces="true"%>
<%@ page import="java.util.Calendar"%>
```

- `taglib`: 태그 라이브러리(tag library). 사용자가 만든 태그 모음이다. taglib는 JSTL 항목 참조
- `include`: 다른 문서를 포함하는 기능. include 지시어는 JSP에서 서블릿 코드를 생성할 때 텍스트나코드를 jsp 파일 안으로 포함 시키며 포함되는 파일의 내용은 include 지시어가 있는 위치에 삽입된다.

```java
<%@ include file="URL" %>
```

include 디렉티브의 처리과정은 정적으로 include 지시자를 사용한 JSP 페이지가 컴파일 되는 과정에서 include 되는 JSP 페이지의 소스 내용을 그대로 포함해서 컴파일한다. 즉, 마치 복사&붙여넣기 한 것처럼 두개의 파일이 하나의 파일로 구성된 후 같이 컴파일 된다.

액션태그 include와 달리 디렉티브의 경우 하나의 자바파일로 컴파일되기 때문에 변수를 공유하는 특징이 있다. 또한 액션태그처럼 매 요청시마다 매번 재컴파일하지 않는다.

```html
<!-- my.jsp -->>
<% String name = "홍길동";%>
<%=name%>
<!-- out.print(name)와 같다 -->
```

위와 같은 JSP가 같은 경로 내에 있을때 아래와 같은 식으로 include한다:

```html
<body>
    <%@ include file="my.jsp"%>
    <%=name%>님
</body>
```

경로는 파일 시스템의 일반적인 규칙을 따르며 해당 경로에 물리적인 파일이 존재해야 한다:

```java
<%@ include file="company.jsp"%> <!-- 같은 폴더의 company.jsp  -->
<%@ include file="./company.jsp"%> <!-- 같은 폴더의 company.jsp -->
<%@ include file="../cart/cart.jsp"%> <!-- 한 단계 상위 폴더에서 cart/cart.jsp 탐색 -->
<%@ include file="../../jsp/cs/notice.jsp"%> <!-- 두 단계 상위 폴더에서 jsp/cs/notice.jsp 탐색 -->
<%@ include file="/WEB-INF/jsp/company/agreement.jsp"%> <!-- root에서 탐색한다. ex) WebContent -->
```

## Declaration, 선언부

스크립릿이나 표현식에서 사용할 수 있는 메서드나 변수를 정의한다. 선언부의 변수는 전역 변수(클래스 변수나 인스턴스 변수)처럼 사용된다. 또한 `_jspInit()`, `_jspDestory()`와 같은 생명주기 운영을 위한 메서드를 재정의 할 수 있다. 선언부의 변수와 스크립릿의 변수는 생존주기가 다른데 가령 스크립릿 변수는 `service()` 메서드의 지역변수이며 페이지가 로딩될 때마다 초기화된다.

```java
<%! 자바 메서드 정의 %>
<%! 자료형 변수명 [= 초기)값]; %>
```

```java
<%! int a = 0;%>
<%! int a,  b; double  c;%>
<%!
    public int add(int a, int b) {
        return a + b;
    }
%>
<html>
    <head><title>선언부 예시</title></head>
    <body>
        10 * 25 = <%=mutiply(10, 25)%>
    </body>
    .
    .
    .
```

## Scriptlet, 스크립릿

JSP문서 내에 자바코드를 작성하는 부분이다. 스크립릿에 선언 된 변수는 해당 JSP 내에서 로컬 변수로 사용된다. (실제로 JSP Servlet의 `service()` 메서드 내 지역변수로 포함된다)

```java
<% 자바코드1; 자바코드2; 자바코드3; ... %>
```

```java
<%
    // 스크립릿은 java 파일로 변경되면서 service() 메서드 안에 추가된다.
    int sum = 0;
    i = 1;
    while (i <= 100) {
        sum += i;
        i++;
    }

    out.print(sum);
    out.print("<tr bgcolor='#ffffff' height='25'>");
%>
```

## Expression, 표현식

HTML 문서 결과 값에 포함시키고자 할 때 사용 한다. 최근에는 소스 관리의 이유로 가급적 사용하지 않는다.

```java
<%= expression %>
```

```html
<%
    String str = "메롱";
%>
<html>
<head>
</head>
<body>
    <%=str%>
</body>
</head></html>
```

## example: form 파라미터 처리

```java
<%
    request.setCharacterEncoding("utf-8");
    //클라이언트에서 전송되는 문자셋 지정. 설정하지 않으면 한글 깨짐
    //데이터를 전송받기 전에 반드시 해야한다.
    String name = request.getParameter("name");
    String gender = request.getParameter("gender");
    //String hobby = request.getParameter("hobby");

    //동일한 이름을 가진 파라미터를 getParameter() 메서드로 전달받으면 오직 하나만 받을 수 있다.
    String hobby = "";
    String[] h = request.getParameterValues("hobby");
    if (h! = null) {
        for (String s: h) {
            hobby+=s+", ";
        }
        hobby = hobby.substring(0, hobby.lastIndexOf(", ")); //마지막 쉼표 제거
    }

    String hak = request.getParameter("hak");
    String bigo = request.getParameter("bigo");

    bigo = bigo.replaceAll("\n", "<br/>"); //엔터 -> 태그
    bigo = bigo.replaceAll(" ", " "); //공백 -> 태그
%>
```

```html
<html>
<head></head>
<body>
출력 첫번째 방법: <% out.print(bigo);%>
출력 두번째 방법: <%=bigo%>
</body>
</html>
```
