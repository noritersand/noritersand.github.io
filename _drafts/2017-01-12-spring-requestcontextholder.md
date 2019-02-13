---
layout: post
date: 2017-01-12 15:28:00 +0900
title: 'Spring: RequestContextHolder'
categories:
  - spring
tags:
  - java
  - spring
  - requestcontextholder
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

-

태초에 이런게 있었다.

```java
ServletRequestAttributes attrs
        = (ServletRequestAttributes) RequestContextHolder.currentRequestAttributes();

HttpServletRequest request = attrs.getRequest();
Object someAttr = attrs.getAttribute(RequestAttributesName.MOBILE.toString(),
		RequestAttributes.SCOPE_REQUEST);
```

```java
RequestAttributes attrs = RequestContextHolder.currentRequestAttributes();
```

그랬다고 한다.

끗
