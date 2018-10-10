---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'Spring: RedirectView'
categories:
  - java
  - spring
tags:
  - spring
  - RedirectView
---

블라블라

참고: 리다이렉션은 상태코드가 307 혹은 308이며 최초 요청에 대한 응답은 바디가 없다.

## POST 파라미터를 같이 넘기는 방법
```java
@RequestMapping("/redirection")
public ModelAndView redirection() {
    ModelAndView modelAndView = new ModelAndView();
    RedirectView redirectView = new RedirectView("http://landing-url.com");
    redirectView.setStatusCode(HttpStatus.TEMPORARY_REDIRECT);
    redirectView.setHttp10Compatible(true);
    modelAndView.setView(redirectView);
    return modelAndView;
}
```
`setHttp10Compatible` api 문서에 따르면 true로 설정했을 때 303 대신 302로 응답한다고 한다. (HTTP 1.0 클라이언트는 303 응답코드를 이해하지 못함)
