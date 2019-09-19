---
layout: post
date: 2015-01-25 22:54:00 +0900
title: 'Spring: @ControllerAdvice'
categories:
  - spring
tags:
  - java
  - spring
  - controlleradvice
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc](https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc)

spring framework의 @ControllerAdvice 관련 내용 정리

## @ExceptionHandler

`@ControllerAdvice` 와 `@ExceptionHandler`의 조합으로 컨트롤러에서 던져진 익셉션을 처리할 전역 클래스를 지정할 수 있다.

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    /**
     * 타입이 명시되지 않은 모든 예외 처리
     *
     * @param ex 컨트롤러에서 발생한 Exception
     * @param req 클라이언트 요청정보
     * @return
     * @throws Exception
     */
    @ExceptionHandler(Exception.class)
    public ModelAndView handleException(Exception ex, WebRequest req,
            HttpServletResponse res) throws Exception {
        // 핸들러 할당
        if (ex instanceof UncategorizedSQLException) {
            UncategorizedSQLException sqlEx = (UncategorizedSQLException) ex;
            return handleUncategorizedSqlException(sqlEx, req, res);
        }
        // 핸들러가 지정되지 않은 예외일 때
        else {
            if ("global exception test".equals(ex.getMessage())) {
                ModelAndView mv = new ModelAndView("sample/extest");
                return mv;
            }
            throw ex;
        }
    }
}
```

만약 전역으로 작동하지 않는다면 dispatcher-servlet.xml 등의 context 설정파일에 아래처럼 되어 있는지 확인한다:

```xml
<mvc:annotation-driven/>
<context:component-scan base-package="com.test" use-default-filters="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    <context:include-filter type="annotation" expression="org.springframework.web.bind.annotation.ControllerAdvice"/>
</context:component-scan>
```

참고:

- `<mvc:annotation-driven>`은 스프링 MVC를 활성화하는 요소로 사용된다.
- `<context:component-scan>`은 스프링 컨테이너가 빈으로 등록할 컴포넌트의 스캐닝 범위를 지정한다.
