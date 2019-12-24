---
layout: post
date: 2013-08-17 01:57:00 +0900
title: '[Spring] Spring Framework: MVC 작성방법 (non-annotation)'
categories:
  - spring
tags:
  - java
  - spring
  - spring-mvc
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://spring.io/guides/gs/serving-web-content/](https://spring.io/guides/gs/serving-web-content/)

Spring MVC의 주요 구성 요소:

- DispatcherServlet: 클라이언트의 요청을 전달받는다. Controller에게 클라이언트의 요청을 전달하고, Controller가 반환한 결과 값을 View에 전달하여 알맞은 응답을 생성하도록 한다.
- HandlerMapping: 클라이언트의 요청 URL을 어떤 Controller가 처리할지를 결정한다.
- Controller: 클라이언트의 요청을 처리한 뒤, 그 결과를 DispatcherServlet에 알려준다. 스트럿츠의 Action과 동일한 역할을 수행한다.
- ViewResolver: Commander의 처리 결과를 보여줄 View를 결정한다.
- View: Commander의 처리 결과를 보여줄 응답을 생성한다.

![](/images/spring-mvc.jpg)

## HandlerMapping

클라이언트의 요청을 Spring의 DispatcherServlet이 처리하도록 설정했다면, 다음으로 해야 할 작업은 어떤 HandlerMapping을 사용할지의 여부를 지정하는 것이다. HandlerMapping은 클라이언트의 요청을 어떤 Controller가 수행할 지의 여부를 결정해주는데, 구체화 클래스는 다음과 같다.

- BeanNameUrlHandlerMapping: 요청 URL와 동일한 이름을 가진 Controller 빈을 매핑 한다.
- SimpleUrlHandlerMapping: 패턴과 컨트롤러의 이름을 비교 URL가 패턴에 매칭될 때 지정한 컨트롤러를 사용 한다.
- ControllerClassNameHandlerMapping: URL와 매칭 되는 클래스 이름을 갖는 빈을 컨트롤러로 사용. 잘 쓰이지 않음
- DefaultAnnotationHandlerMapping: @RequestMapping 어노테이션을 이용하여 요청을 처리할 컨트롤러를 구한다.

### BeanNameUrlHandlerMapping

BeanNameUrlHandlerMapping은 요청 URL와 동일한 이름을 갖는 Controller 빈으로 하여금 클라이언트의 요청을 처리하도록 한다. 예를 들어, `http://host/hello.action` 과 같은 요청 URL에 대해 `/hello.action`라는 이름을 가진 Controller 빈이 해당 요청을 처리하도록 한다.

다음과 같이 alwaysUseFullPath를 true로 설정 하고 빈을 설정 한 경우:

```xml
<bean id="beanNameUrlMapping" class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping">
     <property name="alwaysUseFullPath" value="true"/>
</bean>
<bean name="/content/**/*.action" class="..."/>
<bean name="/hello/hello.action" class="..."/>
```

이 경우 request를 처리하는 컨트롤 빈은 다음과 같이 매핑 된다.

```
'/content/**/*.action':
-> /content/1/test.action
-> /content/top/exam.action

'/hello/hello.action':
-> /hello/hello.action
```

### SimpleUrlHandlerMapping

SimpleUrlHandlerMapping은 패턴 매칭을 이용해서 다양한 URL 경로를 컨트롤러에 매핑 시켜준다.

다음처럼 mappings 프로퍼티를 이용하여 패턴과 컨트롤러 사이의 매핑을 지정한다:

```xml
<bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
   <property name="alwaysUseFullPath" value="true">
      <prop key="/content/**/*.action">contentController</prop>
      <prop key="/hello/hello.action">helloController</prop>
   </property>
</bean>
<bean name="contentController" class="..."/>
<bean name="helloController" class="..."/>
```

SimpleUrlHandlerMapping의 mappings 프로퍼티는 java.util.Properties 타입이다. mappings 프로퍼티의 값에 전달되는 `<prop>`의 key는 요청 URL과 매칭 될 경로 패턴을 입력하며 `<prop>`의 값에는 매핑될 컨트롤러의 이름을 입력 한다. (패턴은 ant pettern을 사용한다.)

경로 패턴 문자:

- `?`: 1개의 문자와 매칭
- `*`: 0개 이상의 문자와 매칭
- `**`: 0개 이상의 디렉터리와 매칭

## Controller

### Controller 인터페이스

Controller를 구현하는 가장 간단한 방법은 Controller 인터페이스를 implements 하는 것이지만, Controller 인터페이스를 직접적 implements 하기 보다는, Controller 인터페이스를 implements 하고 몇 가지 추가적인 기능을 구현하고 있는 클래스들을 상속받아 Controller를 구현하는 것이 일반적이다.

org.springframework.web.servlet.mvc.Controller 인터페이스는 다음과 같이 정의되어 있다.

```java
public interface Controller {
    ModelAndView handleRequest( HttpServletRequest req, HttpServletResponse resp ) [throws Exception]
}
```

### AbstractController 추상 클래스

Controller 인터페이스를 구현한 추상 클래스로 단순히 클라이언트의 요청을 처리한 뒤 ModelAndView를 반환할 경우에 AbstractController 클래스를 상속 받아 컨트롤러를 구현한다.

AbstractController 클래스는 다음과 같이 handleRequestInternal 추상 메서드를 선언하고 있으며 AbstractController 클래스를 상속받는 컨트롤러 클래스는 이 메서드를 구현해야 한다.

```java
protected ModelAndView handleRequestInternal(HttpServletRequest req, HttpServletResponse resp) [throws Exception]
```

### AbstractController 추상 클래스 적용 예시

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>spring3</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>

    <!-- 스프링 환경 설정 시작 -->
    <!-- ContextLoaderListener: 서로 다른 DispatcherServlet이 공통으로 사용될 빈 설정  -->
    <!-- context-param 태그로 설정파일을 지정하지 않으면 applicationContext.xml이 설정 파일이 된다.  -->
    <!--
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
                /WEB-INF/service.xml,
                /WEB-INF/common.xml
        </param-value>
    </context-param>
    -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- 스프링 컨트롤러 -->
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>
                /WEB-INF/dispatcher-servlet.xml,
                /WEB-INF/front.xml
            </param-value>
        </init-param>
        -->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>

    <!-- 인코딩 필터 -->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

dispatcher-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:util="http://www.springframework.org/schema/util"
    xmlns:task="http://www.springframework.org/schema/task"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.1.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd
        http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-3.1.xsd
        http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-3.1.xsd">

    <bean id="beanNameUrlHandlerMapping" class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping">
        <property name="order" value="1"/>
    </bean>

    <bean id="helloService" class="com.hello.HelloService"/>
    <bean name="/hello.action" class="com.hello.HelloController">
        <property name="helloService" ref="helloService"/>
    </bean>

    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/" />
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```

com.hello.HelloService

```java
package com.hello;

public class HelloService {
    public String getMessage() {
        String msg = null;

        int h = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);

        if(h>=6 && h<9)
            msg = "좋은 아침";
        else if(h>=9 && h<13)
            msg = "오전 수업 시간";
        else if(h>=13 && h<14)
            msg = "점심";
        else if(h>=14 && h<18)
            msg = "오후수업시간";
        else
            msg = "자유시간";
        return msg;
    }
}
```

com.hello.HelloController

```java
package com.hello;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.AbstractController;

public class HelloController extends AbstractController {
    private HelloService helloService;
    public void setHelloService(HelloService helloService) {
        this.helloService = helloService;
    }

    @Override
    protected ModelAndView handleRequestInternal(HttpServletRequest req,
            HttpServletResponse resp) throws Exception {
        ModelAndView mav = new ModelAndView();
        mav.setViewName("hello/result");
        mav.addObject("msg", helloService.getMessage());

        return mav;

                //혹은
        req.setAttribute("msg", helloService);
        return new ModelAndView("hello/result");

    }
}
```

WEB-INF/hello/result.jsp

```html
<%@ page contentType="text/html; charset=UTF-8"%>
<%@ page trimDirectiveWhitespaces="true"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%
String cp = request.getContextPath();
request.setCharacterEncoding("utf-8");
%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>

스프링을 이용한 첫번째 MVC2 패턴 프로그래밍<br/>
${msg}

</body>
</html>
```

### MultiActionController

하나의 Controller에서 여러 개의 요청 처리 지원한다. 하나의 컨트롤러를 이용하여 여러 요청을 처리할수 있다. 예를 들어 게시판에서 글리스트, 글쓰기폼, 글저장, 글보기, 글수정폼, 글수정완료, 글삭제등의 여러 요청을 하나의 컨트롤러로 처리할수 있다.

작성 시 MultiActionController 상속하여 클라이언트의 요청을 처리할 다음의 메서드를 구현한다:

```java
public [ModelAndView|Map|void] 메서드이름(HttpServletRequest req, HttpServletResponse resp [, HttpSession|Command]) [throws Exception]
```

### MultiActionController 적용 예시

web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  <display-name>spring3</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>

    <!-- 스프링 환경 설정 시작 -->
    <!-- ContextLoaderListener: 서로 다른 DispatcherServlet이 공통으로 사용될 빈 설정  -->
    <!-- context-param 태그로 설정파일을 지정하지 않으면 applicationContext.xml이 설정 파일이 된다.  -->
    <!--
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>
                /WEB-INF/service.xml,
                /WEB-INF/common.xml
        </param-value>
    </context-param>
    -->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>

    <!-- 스프링 컨트롤러 -->
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>
                /WEB-INF/dispatcher-servlet.xml,
                /WEB-INF/front.xml
            </param-value>
        </init-param>
        -->
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>dispatcher</servlet-name>
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>

    <!-- 인코딩 필터 -->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
</web-app>
```

dispatcher-servlet.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:p="http://www.springframework.org/schema/p"
    xmlns:util="http://www.springframework.org/schema/util"
    xmlns:task="http://www.springframework.org/schema/task"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.1.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-3.1.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd
        http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-3.1.xsd
        http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task-3.1.xsd">

    <bean id="handlerMapping" class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
         <property name="alwaysUseFullPath" value="true" />
         <property name="order" value="1" />

         <property name="mappings"> //property: private String name, 즉 인스턴스변수
             <props> //props: properties
                 <prop key="/bbs/*.action">bbs.boardController</prop>
                 <!-- 여기서 bbs.boardController는 value -->
                 <!-- 아래에 bean으로 생성할 객체 bbs.boardController를 찾아간다. -->
             </props>
         </property>
    </bean>

    <bean id="bbs.boardController" class="com.bbs.BoardController">
        <property name="methodNameResolver" ref="propsResolver"/>
    </bean>

    <bean id="propsResolver"
    class="org.springframework.web.servlet.mvc.multiaction.PropertiesMethodNameResolver">
    <property name="mappings">
        <props> <!-- props: properties -->
           <prop key="/bbs/list.action">list</prop>
           <prop key="/bbs/created.action">createdForm</prop>
           <prop key="/bbs/created_ok.action">createdSubmit</prop>
        </props> <!-- propsResolver의 인스턴스변수 mappings에 각 key에 해당된 value 할당 -->
    </property>
    </bean>

    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/" />
        <property name="suffix" value=".jsp" />
    </bean>
</beans>
```

com.bbs.Board

```java
package com.bbs;

public class Board
{
    private int num;
    private String name, subject, content, created;
    public int getNum() {
        return num;
    }
    public void setNum(int num) {
        this.num = num;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getSubject() {
        return subject;
    }
    public void setSubject(String subject) {
        this.subject = subject;
    }
    public String getContent() {
        return content;
    }
    public void setContent(String content) {
        this.content = content;
    }
    public String getCreated() {
        return created;
    }
    public void setCreated(String created) {
        this.created = created;
    }
}
```

com.bbs.BoardController

```java
package com.bbs;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.multiaction.MultiActionController;

public class BoardController extends MultiActionController {
    public ModelAndView list(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        return new ModelAndView("bbs/list");
    }

    public ModelAndView createdForm(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        return new ModelAndView("bbs/write");
    }

    public ModelAndView createdSubmit(HttpServletRequest req, HttpServletResponse resp) throws Exception {
        return new ModelAndView("bbs/write_ok");
    }
}
```

list.jsp

```html
<!DOCTYPE html>
<html>
<head></head>
<body>
    게시판 리스트<br/>
    <a href="<%=cp%>/bbs/created.action">글쓰기</a>
</body>
</html>
```

write.jsp

```html
<!DOCTYPE html>
<html>
<head></head>
<body>
<form action="<%=cp%>/bbs/created_ok.action" method="post">
    이름: <input type="text" name="name"/><br/>
    제목: <input type="text" name="subject"/><br/>
    내용: <textarea rows="5" cols="60" name="content"></textarea><br/>
    <input type="submit" value="등록"/>
</form>
</body>
</html>
```

write_ok.jsp

```html
<!DOCTYPE html>
<html>
<head></head>
<body>
    입력한 내용 <br/>
    이름: ${dto.name} <br/>
    제목: ${dto.subject} <br/>
    내용: ${dto.content}<br/>
<a href="<%=cp%>/bbs/list.action">뒤로가기</a>
</body>
</html>
```
