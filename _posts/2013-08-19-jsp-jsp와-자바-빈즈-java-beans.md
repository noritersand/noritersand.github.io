---
layout: post
date: 2013-08-19 15:04:00 +0900
title: 'JSP: JSP와 자바 빈즈(Java Beans)'
categories:
  - java
  - jsp
tags:
  - java
  - jsp
  - java beans
---

* Kramdown table of contents
{:toc .toc}

자바 빈즈는 JSP 페이지의 로직 부분을 분리해서 코드를 재사용함으로 프로그램의 효율을 높이기 위해 사용한다. 프로그램의 모듈화는 코드를 재사용하므로 프로그램의 작성기간이 단축되고, 이미 사용되던 코드이므로 안정성이 보장되며 유지보수가 쉽다. MVC 패턴에서 자바 빈은 프로그램 로직을 소유할 수 있고 DB와 연동해서 작업을 처리한다.

## 자바 빈 작성

자바 빈(JavaBean)은 데이터를 표현하는 것을 목적으로 하는 자바 클래스다. 콤포넌트와 비슷한 의미로 사용되기도 한다. 자바빈은 자바빈 규칙을 따르며 다음과 같은 형태로 구성된다:

```java
package 패키지명;

[import 패키지명;]

public class Bean_ClassName [ implements java.io.Serializable ] {
    private String name;    // 값을 저장하는 속성 정의(필드)
    public Bean_ClassName() { }    // 기본 생성자

    public String getName() {    // 필드 값을 읽어오는 메서드
        return name;
    }
    public void setName(String name) {    // 필드 값을 저장하는 메서드
        this.name = name;
    }
}
```

￼￼￼￼￼￼java.io.Serializable 인터페이스는 생략 가능하나 빈즈 규약에 명시된 내용으로 자바 빈즈에 저장된 프로퍼티를 포함한 채로 파일시스템에 저장되거나 네트워크로 전송될 수 있도록 객체 직렬화를 제공 해야 하므로 implement 한다.

위에서 인스턴스 변수(프로퍼티)의 값을 읽고 쓰는 메서드의 이름은 일반적으로 변수명 앞에 get이나 set을 붙이지만 리턴하는 값이 boolean일 경우엔 is + 변수명으로 명명한다. (get이라 해도 그닥 문제되는건 없음)

```java
public class Temp {
    private boolean flag;

    public boolean isFlag() {
        return flag;
    }
}
```

## 작성 위치

빈즈(클래스 파일)은 `웹 어플리케이션\WEB-INF\classes` 폴더에 존재해야 한다. 따라서 일반적으로 src 폴더를 하나 더 만들고 소스는 src 폴더 작성하고 컴파일 한 후 해당 클래스 파일은 classes 폴더 아래 패키지 폴더를 만들고 만들어진 패키지 폴더에 복사한다.

예를 들어 `톰캣루트\webapps\study`에서 작업을 하는 경우 `톰캣루트\webapps\study\WEB-INF\classes` 폴더 및 `톰캣루트\webapps\study\WEB-INF\web.xml` 파일을 작성한다. web.xml 파일은 `톰캣루트\webapps\ROOT\WEB-INF` 파일을 복사한다.

폴더를 작성하지 않고 컴파일 하는 경우에는 -d 옵션을 사용하여 컴파일 하면 자동으로 폴더를 생성해 준다.

```bash
javac -d . Test.java
```

`-d` 다음의 `.`은 현재 경로 아래에 패키지에 기술된 경로를 작성하여 컴파일 하라는 의미다. 이때 컴파일 과정에서 생성된 클래스 파일은 패키지에 기술된 경로에 생성 된다.

## example 1

`톰캣루트\webapps\study\WEB-INF\classes\src` 폴더를 작성하고 HelloBean.java 파일을 작성하여 컴파일 한다.

```java
package com.test.bean;

public class HelloBean {
    private String name = "홍길동";

    public HelloBean() {}

    public void setName(String name) {
        this.name=name;
    }

    public String getName() {
        return name;
    }
}
```

`톰캣루트\webapps\study\WEB-INF\classes\com\test\bean` 폴더를 작성하고 컴파일 하여 생성된 HelloBean.class 파일을 복사한다.

JSP 파일 작성 - HelloBean.jsp(study 폴더에 저장)

```html
<%@ page contentType="text/html;charset=utf-8"%>
<jsp:useBean id="time" class="java.util.Date" scope="page" />
<jsp:useBean id="myBean" class="com.test.bean.HelloBean" scope="page" />

<html>
<head><title>빈즈 테스트</title></head>
<body>
<h3>빈즈 테스트</h3>
<hr/>
<%= myBean.getName()%><br/>
<% myBean.setName("이순신");%><%=myBean.getName()%><br/>
현재시간 : <%= time.toLocaleString()%>
</body>
</html>
```

## example 2

`톰캣루트\webapps\study\WEB-INF\classes\src` 폴더에 MemBean.java 파일을 작성하여 컴파일 한다.

```java
package com.test.bean;

public class MemBean {
    private String id;
    private String name;
    private String email;

    public MemBean() { }

    public void setId(String id) {
        this.id=id;
    }

    public String getId() {
        return id;
    }

    public void setName(String name) {
        this.name=name;
    }

    public String getName() {
        return name;
    }

    public void setEmail(String email) {
        this.email=email;
    }

    public String getEmail() {
        return email;
    }
}
```

`톰캣루트\webapps\study\WEB-INF\classes\com\test\bean` 폴더를 작성하고 컴파일 하여 생성된 MemBean.class 파일을 복사한다.

회원 가입 폼 작성 - propertyTest.html(study 폴더에 저장)

```html
<html>
<head>
<title>폼 예시</title>
<meta http-equiv="content-type" content="text/html; charset=utf-8">
</head>
<body>
<h2>회원 가입</h2>
<form name='myForm' method='post' action='propertyTest.jsp'>
    아이디 : <input type='text' name='id' size='20' maxlength='10'/><br/>
    이 름 : <input type='text' name='name' size='20' maxlength='10'/><br/>
    이메일 : <input type='text' name='email' size='20' maxlength='10'/><br/>
    <input type='submit' value=' 회원 가입 '/>
    <input type='reset' value=' 다시 입력 ' onClick='document.myForm.id.focus();'/>
    <br/>
</form>
</body>
</html>
```

JSP 파일 작성 - propertyTest.jsp(study 폴더에 저장)

```html
<%@ page contentType="text/html;charset=utf-8"%>
<% request.setCharacterEncoding("utf-8");%>
<jsp:useBean id="myBean" class="com.test.bean.MemBean" scope="page" />
<jsp:setProperty name="myBean" property="id"/>
<jsp:setProperty name="myBean" property="name"/>
<%    myBean.setEmail(request.getParameter("email"));%>
<html>
<head><title>빈즈 테스트</title></head> <body>
<h3>빈즈 테스트</h3>
아이디 : <%= myBean.getId()%><br/>
이름 : <%= myBean.getName()%><br/>
이메일 : <%= myBean.getEmail()%> <br/>
</body>
</html>
```

## 그 외

다음 코드는 Form 태그에서 넘어온 id 필드명과 같은 자바 빈의 `setId()` 메서드를 검색하여 프로퍼티에 값을 할당한다.

```html
<jsp:setProperty name="myBean" property="id"/>
```

만약 폼 필드의 이름이 "username"이고 자바 빈의 프로퍼티가 "name"인 경우에는:

```html
<jsp:setProperty name="myBean" property="name"/>
```

위 처럼 작성할 수 없고 이런 경우에는 다음과 같이 param 속성을 이용한다:

```html
<jsp:setProperty name="myBean" property="name" param="username"/>
```

만약 폼 필드의 이름과 자바 빈 프로퍼티의 이름이 모두 일치한다면 다음처럼 3, 4, 5번 라인은 다음처럼 한줄로 처리할 수 있다:

```html
<jsp:setProperty name="myBean" property="*"/>
```

이는 각 폼 필드의 이름에 해당하는 `setXxx()` 메서드를 찾아 프로퍼티를 설정하라는 의미이다.

빈즈의 name 프로퍼티를 출력하기 위해서는 다음과 같이 기술 한다:

```html
<% out.println("이름 : " + myBean.getName());%>
```

위의 내용은 `<jsp:getProperty>` 액션 태그를 이용하여 다음과 같이 작성 할 수 있다:

```html
이름: <jsp:getProperty name="myBean" property="name"/>
```

폼에서 넘어온 데이터가 아닌 JSP 페이지에서 값을 자바 빈의 프로퍼티에 설정하기 위해서는 `<jsp:setProperty>` 태그에서 value 속성을 이용한다:

```html
<% String name = "홍길동";%>
<jsp:setProperty name="myBean" property="name" value="<%=name%>"/>
```
