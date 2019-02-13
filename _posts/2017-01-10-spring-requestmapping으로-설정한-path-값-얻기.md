---
layout: post
date: 2017-01-10 10:04:00 +0900
title: 'Spring: RequestMapping으로 설정한 path 값 얻기'
categories:
  - spring
tags:
  - java
  - spring
  - path
  - requestmapping
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

```java
@RequestMapping({ "/first-path", "/second-path" })
public String myAction(Model model, HttpServletRequest request) {
    String path = (String) request.getAttribute(HandlerMapping.PATH_WITHIN_HANDLER_MAPPING_ATTRIBUTE);

    if ("first-path".equals(path)) {
        // do something
    } else if ("second-path".equals(path)) {
        // do something
    }

    return "viewName";
}
```

설정된 path 값을 가져오는 코드지만 요청에 따라 결과는 달라진다. 가령 요청 URL이 "http://localhost/first-path"면 얻는 값은 "first-path"다.
