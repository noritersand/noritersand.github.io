---
layout: post
date: 2017-01-12 15:28:00 +0900
title: '[Spring] RequestContextHolder'
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
Object someAttr = attrs.getAttribute("someAttrName", RequestAttributes.SCOPE_REQUEST);
```

```java
RequestAttributes attrs = RequestContextHolder.currentRequestAttributes();
Object someAttr = attrs.getAttribute("someAttrName", RequestAttributes.SCOPE_REQUEST);
```

그랬다고 한다.

대~충 응용하면:

```java
ServletRequestAttributes attrs = (ServletRequestAttributes) RequestContextHolder.currentRequestAttributes();
HttpServletRequest request = attrs.getRequest();
HttpSession session = request.getSession();

Enumeration<String> enumeration = session.getAttributeNames();
while (enumeration.hasMoreElements()) {
	String string = (String) enumeration.nextElement();
	logger.debug("AttributeNames: {}", string);
}
```

요딴식으로 session을 급히 가져다 쓸 수도 있다.

끗.
