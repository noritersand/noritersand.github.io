---
layout: post
date: 2016-12-05 18:03:00 +0900
title: 'Spring: @InitBinder'
categories:
  - java
  - spring
tags:
  - java
  - spring
  - initbinder
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

-

WebDataBinder를 초기화하는 method를 지정 할 수 있는 설정을 제공한다.

일반적으로 WebDataBinder는 annotation handler 메서드의 command 와 form 객체 인자를 조작하는데 사용된다.

InitBinder 메서드가 필수적으로 반환값을 가질 필요는 없으며, 일반적으로 이런 경우에 void를 선언한다. 특별한 인자는 WebdataBinder와 WebRequest또는 Locale의 조합으로 이루어지며, 이러한 조건이 만족되면 context-specific editors를 등록하는것이 허용된다.

#### WebdataBinder

WebDataBinder는 web request parameter를 javaBean 객체에 바인딩하는 특정한 DataBinder이다. WebDataBinder는 웹 환경이 필요하지만, Servlet API에 의존적이지 않다. servlet API에 의존적인 ServletRequestDataBinder와 같이 특정한 DaraBinder를 위한 더많은 base class를 제공한다.

#### RequestMapping

RequestMapping annotation은 web request를 특정한 handler class와 handler method에 mapping하는 역활을 수행한다. 대응하는 handlerMapping(for type level annotation)과 HandlerAdapter(for method level annotation)가 dispatch에 존재한다면, `@RequestMapping`이 처리된다.

#### WebRequest

WebRequest는 웹 요청에 대한 Generic interface이다. 주로 일반 request metadata에 generic web request interceptors의 접근을 허용하여 metadata에 대한 처리를 하기 위한 것이지 request 자체를 처리하기 위한 것은 아니다.

#### Annotation 기반 Controller 에서 ServletContext 구하기

```java
@Controller
@RequestMapping("/common/download")
public class DownloadController {
    @Autowired
    private ServletContext sc;

    @RequestMapping
    public ModelAndView download(@RequestParam("filePath") String filePath) throws Exception {
        String path = sc.getRealPath(filePath);
        return new ModelAndView("common.download", "downloadFile", new File(path));
    }
}
```
