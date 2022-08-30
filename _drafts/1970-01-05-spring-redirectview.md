---
layout: post
date: 1970-01-05 00:00:00 +0900
title: '[Spring] RedirectView'
categories:
  - spring
tags:
  - java
  - spring
  - redirectview
---

블라블라

참고: 리디렉션은 상태코드가 307 혹은 308이며 최초 요청에 대한 응답은 바디가 없다.

## POST 파라미터를 같이 넘기는 방법

```java
import org.springframework.http.HttpStatus;
import org.springframework.web.servlet.view.RedirectView;

...

  @RequestMapping("/testRV")
  public RedirectView testRV() {
      RedirectView rv = new RedirectView("/example/draw-example.do");
      rv.setStatusCode(HttpStatus.TEMPORARY_REDIRECT); // 307 Temporary Redirect
      rv.setHttp10Compatible(true); // HTTP 1.0 호환 설정
      rv.setExposeModelAttributes(false); // ModelAttribute가 URL에 노출되는 것을 방지함
      return rv;
  }
```

- `setHttp10Compatible`: API 문서에 따르면 true로 설정했을 때 HTTP 1.0에서 이해하는 응답코드로 대신 설정한다고 한다. (가령 303은 302로 치환된다)
