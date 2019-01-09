---
layout: post
date: 2016-01-25 16:42:00 +0900
title: 'Spring: @RequestMapping'
categories:
  - java
  - spring
tags:
  - java
  - spring
  - requestmapping
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- http://docs.spring.io/spring/docs/current/spring-framework-reference/html/mvc.html#mvc-ann-requestmapping
https://docs.spring.io/spring/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestMapping.html


URL을 컨트롤러의 메서드와 매핑할 때 사용하는 스프링 프레임워크의 어노테이션이다.

클래스나 메서드 선언부에 @RequestMapping과 함께 URL을 명시하여 사용한다. URL외에도 HTTP 요청 메서드나 헤더값에 따라 매핑되도록 -0=옵션을 제공한다. 메서드 레벨에서 정의한 @RequestMapping은 타입 레벨에서 정의된 @RequestMapping의 옵션을 상속받는다.

참고로, 메서드 내에서 viewName을 별도로 설정하지 않으면 @RequestMapping의 path로 설정한 URL이 그대로 viewName으로 설정된다.

## options

### path(혹은 value)

요청된 URL에 따라 매핑한다.

```java
path = "some-url.action"
path = { "some-url1, some-url2" }
```

매핑할 URL을 하나 이상 지정한다.

```java
@RequestMapping(path = { "/addMovie.do", "/updateMovie.do" })
public String myMethod() {
    // "/addMovie.do", "/updateMovie.do" 두 URL 모두 처리한다.
}
```

디폴트 속성이기 때문에 속성명을 생략하고 `@RequestMapping("/addMovie.do")`와 같은 방식으로 사용할 수 있다.

### method

GET, POST, PUT, DELETE같은 HTTP Request method에 따라 매핑을 결정한다. 값은 enum 타입인 RequestMethod다.

```java
@RequestMapping(method = RequestMethod.POST)
public String myMethod() {
    // ...
}
```

### params

요청된 파라미터에 따라 매핑한다.

```java
params = { "someParam1=someValue", "someParam2" }
params = "!someExcludeParam"
```

아래처럼 설정하면 요청 파라미터에 param1과 param2 파라미터가 존재해야하고 param1의 값은 'a'이어야하며, myParam이라는 파라미터는 존재하지 않아야한다.

```java
@RequestMapping(params = {"param1=a", "param2", "!myParam"})
public String myMethod() {
    // ...
}
```

### headers

특정 헤더의 값에 따라 매핑한다.

```java
headers = "someHader=someValue"'
headers = "someHader"', 'headers="!someHader"
```

와일드 카드 표현*도 지원한다.

아래처럼 설정하면 HTTP Request에 Content-Type 헤더 값이 "text/html", "text/plain" 모두 매칭이 된다.

```java
@RequestMapping(path = "/movie.do", headers = "content-type=text/*")
public String myMethod() {
    // ...
}
```

### produces

```java
@RequestMapping(path = "...", produces = "application/json")
@RequestBody
public HashMap<String, String> testMethod(Model model) {
    HashMap<String, String> map = new HashMap<String, String>();
    map.put("code", "0");
    return map;
}
```

TODO

반대격으로 `consumes`가 있는걸로 **추정된다.**

## example

```java
@Controller
@RequestMapping("/test/*")
public class SampleController1 {
    @RequestMapping(method = RequestMethod.GET, path = "go")
    public returntype getMethodName() {
        // ...
    }

    @RequestMapping(method = RequestMethod.POST, path = "go2")
    public returntype getMethodName2() {
        // ...
    }
}

@Controller
@RequestMapping("/main.action")
public class SampleController2 {
    @RequestMapping(method=RequestMethod.GET)
    public String method() {
        return ".mainLayout";
    }
}

@Controller
public class SampleController3 {
    @RequestMapping("/main.action")
    public returntype m01() {
        // ...
    }
}
```
