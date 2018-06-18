---
title: 'Spring: RedirectView'
categories:
  - java
  - spring
tags:
  - todo
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
핵심은 `setHttp10Compatible`. 다른건 크게 상관 없다.
