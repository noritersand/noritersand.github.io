---
layout: post
date: 2015-08-16 03:53:00 +0900
title: '[Servlet] web.xml'
categories:
  - servlet
tags:
  - java
  - servlet
  - tomcat
  - config
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [뀨](뀨)
- [https://docs.oracle.com/cd/E13222_01/wls/docs92/webapp/configureservlet.html](https://docs.oracle.com/cd/E13222_01/wls/docs92/webapp/configureservlet.html)
- [https://docs.oracle.com/javase/jndi/tutorial/beyond/env/source.html](https://docs.oracle.com/javase/jndi/tutorial/beyond/env/source.html)

web.xml은 WAS의 실행환경에 대한 정보를 담당하는 '환경설정' 파일이다. 각종 servlet의 설정과 servlet 매핑, 필터, 인코딩 등을 담당하며 WAS에 있는 모든 web application의 기본설정을 정의한다.

web.xml은 각 application이 deploy될 때 각 application의 `WEB-INF/web.xml` deployment descripter에 따라서 처리가 된다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.1"
        xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd">
    <display-name>web app name</display-name>
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
        <welcome-file>index.jsp</welcome-file>
        <welcome-file>default.html</welcome-file>
        <welcome-file>default.htm</welcome-file>
        <welcome-file>default.jsp</welcome-file>
    </welcome-file-list>
</web-app>
```
▲ servlet spec 3.1의 web.xml

## JESSIONID 이름 바꾸기

JAVA WAS는 세션을 구분하기 위해 사용자 브라우저에 'JESSIONID'라는 쿠키를 생성한다. 만약 이 쿠키의 이름을 바꾸고 싶다면 다음을 web.xml에 추가한다:

```xml
<session-config>
    <cookie-config>
        <name>MYJSESSIONID</name>
    </cookie-config>
</session-config>
```

## VM arguments에 접근

```
<filter>
    <filter-name>encodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>${file.encoding}</param-value>
    </init-param>
</filter>
```

**TODO**: `${}` 표현식으로 VM arguments(JVM의 `-D` 옵션)에 설정한 값을 가져올 수 있다.

## Loader

### ~~virtualClasspath~~

```xml
<Context docBase="backoffice" path="/" reloadable="false" source="org.eclipse.jst.jee.server:backoffice">
    <Resource auth="Container" driverClassName="oracle.jdbc.OracleDriver" ... />
    <Loader className="org.apache.catalina.loader.VirtualWebappLoader" virtualClasspath="C:/somewhere" />
</Context>
```

이런식으로 특정 경로를 런타임 클래스패스로 지정하는 옵션이 있었는데 Tomcat 7 까지만 쓰이고 지금은 없어졌다. 굳이 한 마디 덧붙이면, 클래스패스를 이런식으로 관리하면 매우 거시기하다.

## load-on-startup

## init-param

## HttpServlet 오버라이드 메서드들

## 태그 종류

- `icon`: 웹 애플리케이션을 나타내기 위해 IDE나 GUI 툴에서 사용 되는 하나 또는 두 개의 이미지 파일들의 위치를 지정하는데 사용한다.
- `display-name`: GUI 툴이 웹 애플리케이션을 표시하기 위해 사용하는 이름을 지정 하는데 사용
- `description`: 웹 어플리케이션에 대한 설명을 나타낸다.
- `distributable`: distributable 요소가 있다는 것은 웹 어플리케이션이다 중 서버 간에 분산 배치 될 수 있다는 것을 의미한다.
- `context-param`: 어플리케이션의 초기화 파라미터를 선언하는데 사용
- `filter`: 서블릿이나 jsp 페이지로 들어오는 요청정보를 사전에 걸러내는 기능
- `filter-mapping`: 필터를 지정했다면 filter-mapping을 지정하여 하나 이상의 서블릿과 연결함
- `listener`: 서블릿2.3 버전으로부터 세션이나 서블릿 컨텍스트가 생성 또는 수정 되거나 소멸되는 것을 알려주는 이벤트 리스너
- `servlet`: 서블릿이나 jsp 페이지에 초기화 파라미터나 사용자url들을 할당 할 때 사용되는 서블릿이나 jsp 이름을 지정
- `servlet-mapping`: 상대 url  경로를 좀  더 쉽게  다루기 위해  기본 url를  변경 할  때 사용
- `session-config`: 일정 시간 동안 세션으로 접근이 없을 경우 서버는 메모리를 절약하기 위해 사용하지 않는 메모리를 삭제한다. 세션의 시간 유지 기능
- `mime-mapping`: 특정한 mime형을 가진 파일을 웹 어플리케이션에  넣어 두고 싶은  경우 사용
- `welcome-file-list`: 요청 URL이 파일명이 아닌 특정 디렉터리명으로 끝나는 경우 사용할 default 파일명을 지정하는데 사용
- `error-page`: http 상태코드가 반환되거나 예외가 발생했을 때 그 내용을 출력하는 페이지
- `tag-lib`: 태그라이브러리 설명자 파일의 별칭을 지정하는데 사용
- `resource-envref`: 자원(resource)과  연관되어 관리되는 객체를 선언하는 역할을 한다.
- `resource-ref`: resource-ref 요소는 외부에서 참조해야 할 자원을 선언할  때 사용
- `security-constraint`: url이 보호 되도록 지정하는 역할을  한다. login-config와  연결되어 사용
- `login-config`: 보안된 페이지로 들어가려는 사용자에 대한 서버의 인증 방식을 지정해 준다.
- `security-role`: 통합개발 환경에서 보안정보를 좀 더 조작하기 쉽게 만들어준다.
- `env-entry`: 웹 어플리케이션의 환경항목을 선언한다.
- `ejb-ref`: 엔터프리이즈 빈의 홈에 대한 레퍼런스를 선언
- `ejb-local-ref`: 엔터프라이즈 빈의 로컬 홈에 대한 레퍼런스를 선언

## web.xml을 작성할때 elements의 순서

Tomcat 5.0 이하에서는  elements의 순서 순서를 지키지 않으면 web.xml 에서 에러가 발생 할 수 있다.

1. `<icon>`
1. `<display-name>`
1. `<description>`
1. `<distributable>`
1. `<context-param>`
1. `<filter>`
1. `<filter-mapping>`
1. `<listener>`
1. `<servlet>`
1. `<servlet-mapping>`
1. `<session-config>`
1. `<mime-mapping>`
1. `<welcome-file-list>`
1. `<error-page>`
1. `<taglib>`
1. `<resource-env-ref>`
1. `<resource-ref>`
1. `<security-constraint>`
1. `<login-config>`
1. `<security-role>`
1. `<env-entry>`
1. `<ejb-ref>`
1. `<ejb-local-ref>`

예를 들어 `<servlet>` 태그들을 모두 먼저 작성하고 `<servlet-mapping>` 태그를 작성해야 한다:

```xml
<servlet>
<servlet-mapping>
<servlet>
<servlet-mapping> <!-- 비권장 -->

<servlet>
<servlet>
<servlet-mapping>
<servlet-mapping> <!-- 권장 -->
```
