---
layout: post
date: 2013-08-14 13:47:00 +0900
title: '[JSP] JSTL, JSP Standard Tag Library'
categories:
  - jsp
tags:
  - java
  - jsp
  - jstl
---

* Kramdown table of contents
{:toc .toc}

JSP는 XML처럼 사용자가 태그를 정의해 사용하는 것이 가능하며, 이런 태그를 사용자 정의 태그라 하는데 이들 중 자주 사용하는 것을 표준으로 만들어 놓은것이 JSTL이다. JSTL은 일반적인 웹 애플리케이션 기능인 반복과 조건, 데이터관리, 포맷, XML조작, 데이터베이스 엑세스를 구현하는 라이브러리를 제공하고 있다.

JSTL은 태생이 커스텀태그이기 때문에 jsp와 밀접하게 관계가 있다. application, session, request, response, pageContext 등의 내장객체에 쉽게 접근하며, 그 외에도 파라미터, 헤더, 쿠키 등을 복잡한 코드를 사용하지 않고, 쉽게 직관적으로 사용할 수 있다. 또한 객체의 비교를 `.equals()`로 처리하는 대신 `==` 와 같이 간단한 연산자로 가능하도록 구현했으며, 조건, 반복, 이동에 대한 태그를 지원하기 때문에 태그만으로도 반복 기능을 구현할 수 있다.

```
< 접두어:태그명 속성1:값1, 속성:값2, ... >
```

액션태그가 그렇듯 XML기반에서 작성되었기 때문에 모든 태그는 시작태그과 종료태그를 쌍으로 작성해야한다.

#### Body 없는 경우

```java
<c:if test="testCondition" var="varName"  [scope="{page|request|session|application}"]/>
```

#### Body 있는 경우

```java
<c:if test="testCondition" [var="varName"] [scope="{page|request|session|application}"]>
    blah blah blah
</c:if>
```

## 태그의 종류

- 코어(Core)
  - 기능: 변수지원, 흐름제어, URL처리
  - 접두어(prefix): c
- XML
  - 기능: XML 코어, 흐름 제어, XML 변환
  - 접두어: x
- 국제화
  - 기능: 지역, 메시지 형식, 숫자 및 날짜형식
  - 접두어: fmt
- 데이터베이스
  - 기능: SQL
  - 접두어: sql
- 함수(Functions)
  - 기능: 맵/컬렉션 처리, String 처리
  - 접두어: fn


## 환경설정

기존의 컨텍스트에서 JSTL을 사용하려면 웹 어플리케이이션의 WEB-INF/lib 디렉터리에 필요한 라이브러리를 복사한다. JSTL의 주된 라이브러리 파일은 jstl.jar, standard.jar이고, XML에서 지원되는 기능을 사용하려면 jaxen-full.jar, saxpath.jar, jaxp-api.jar 파일 등이 필요하다.

[http://tomcat.apache.org/taglibs/standard/](http://tomcat.apache.org/taglibs/standard/)

혹은 `톰캣설치폴더/webapps/examples/WEB-INF/lib` 에서 가져온다.

1.2 버전은 톰캣 6.0부터, 1.1버전은 톰캣 5.5부터 사용가능.

### JSP에 taglib 디렉티브 추가

```java
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml"%>
<%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt"%>
<%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql"%>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
```
