---
layout: post
date: 2016-01-12 19:24:00 +0900
title: '[Spring] org.springframework.validation.BindException'
categories:
  - spring
tags:
  - java
  - spring
  - BindException
---

* Kramdown table of contents
{:toc .toc}

## 원인: 파라미터 바인딩 중 캐스팅 에러

메서드의 파라미터를 지정하여 화면에서 전달된 값을 자동으로 할당하는 방식을 사용할 때 간혹 컨트롤러에 진입조차 못하고 org.springframework.validation.BindException 에러가 발생할 때가 있다. 브라우저의 웹 디버거로 확인해보면 HTTP 응답의 상태코드가 400 bad request일 것이다. 아마도

```java
@RequestMapping("/setModelTest.do")
public void setModelTest(ExampleBean exampleBean) {
    System.out.println(exampleBean);
}
```

위에서 ExampleBean은:

```java
public class ExampleBean {
    private double doubleValue;
    private long longValue;
    private int intValue;
    private String stringValue;

    // getter/setter
}
```

이런식으로 객체타입과 원시타입의 변수가 혼합되어 있는 모델이다.

스프링은 화면에서 넘어간 값이 empty string(빈 문자열, `""`)이거나 null일 때 변수에도 이 값을 그대로 할당하려고 사도하는데, 원시타입의 변수들은 empty string이나 null을 할당할 수 없기 때문에 타입 캐스팅 중에 에러가 발생하는 것이다.

다음의 에러메시지는 empty string이 넘어온 경우고:

```java
org.springframework.validation.BindException: org.springframework.validation.BeanPropertyBindingResult: 1 errors
Field error in object 'exampleBean' on field 'intValue': rejected value []; codes [typeMismatch.exampleBean.intValue,typeMismatch.intValue,typeMismatch.int,typeMismatch]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [exampleBean.intValue,intValue]; arguments []; default message [intValue]]; default message [Failed to convert property value of type 'java.lang.String' to required type 'int' for property 'intValue'; nested exception is java.lang.NumberFormatException: For input string: ""]
    at org.springframework.web.method.annotation.ModelAttributeMethodProcessor.resolveArgument(ModelAttributeMethodProcessor.java:118)
    at org.springframework.web.method.support.HandlerMethodArgumentResolverComposite.resolveArgument(HandlerMethodArgumentResolverComposite.java:121)
    at org.springframework.web.method.support.InvocableHandlerMethod.getMethodArgumentValues(InvocableHandlerMethod.java:160)
    at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:129)
    at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:116)
    at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:827)
    at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:738)
    at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:85)
    at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:963)
    at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:897)
    at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:970)
    ...
```

다음은 null이 넘어온 경우다:

```java
org.springframework.validation.BindException: org.springframework.validation.BeanPropertyBindingResult: 1 errors
Field error in object 'exampleBean' on field 'intValue': rejected value [null]; codes [typeMismatch.exampleBean.intValue,typeMismatch.intValue,typeMismatch.int,typeMismatch]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [exampleBean.intValue,intValue]; arguments []; default message [intValue]]; default message [Failed to convert property value of type 'java.lang.String' to required type 'int' for property 'intValue'; nested exception is java.lang.NumberFormatException: For input string: "null"]
    at org.springframework.web.method.annotation.ModelAttributeMethodProcessor.resolveArgument(ModelAttributeMethodProcessor.java:118)
    at org.springframework.web.method.support.HandlerMethodArgumentResolverComposite.resolveArgument(HandlerMethodArgumentResolverComposite.java:121)
    at org.springframework.web.method.support.InvocableHandlerMethod.getMethodArgumentValues(InvocableHandlerMethod.java:160)
    at org.springframework.web.method.support.InvocableHandlerMethod.invokeForRequest(InvocableHandlerMethod.java:129)
    at org.springframework.web.servlet.mvc.method.annotation.ServletInvocableHandlerMethod.invokeAndHandle(ServletInvocableHandlerMethod.java:116)
    at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.invokeHandlerMethod(RequestMappingHandlerAdapter.java:827)
    at org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter.handleInternal(RequestMappingHandlerAdapter.java:738)
    at org.springframework.web.servlet.mvc.method.AbstractHandlerMethodAdapter.handle(AbstractHandlerMethodAdapter.java:85)
    at org.springframework.web.servlet.DispatcherServlet.doDispatch(DispatcherServlet.java:963)
    at org.springframework.web.servlet.DispatcherServlet.doService(DispatcherServlet.java:897)
    at org.springframework.web.servlet.FrameworkServlet.processRequest(FrameworkServlet.java:970)
    ...
```

간단한 해소방법으로 아무 값이나 명시하여 넘기는 것과 값이 없는 항목은 쿼리스트링이나 폼 데이터에서 아예 이름까지 제외시키는 방법이 있다:

```java
"/setModelTest.do?doubleValue=99&longValue=99&intValue=99" // correct
"/setModelTest.do?stringLiteral=imnotempty" // correct
```

추천하는 방법은 파라미터를 바인딩하는 용도의 빈에는 원시타입을 사용하지 않는것이다. 이렇게하면 empty string일 때 null로 초기화된다:

```java
public class ExampleBean {
    private Double doubleValue;
    private Long longValue;
    private Integer intValue;
    private BigDecimal bigDecValue;
    private String stringValue;

    // getter/setter
}
```
