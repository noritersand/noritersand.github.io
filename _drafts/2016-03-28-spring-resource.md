---
layout: post
date: 2016-03-28 11:53:00 +0900
title: 'Spring: @Resource'
categories:
  - spring
tags:
  - java
  - spring
  - resource
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

-

## @Resource

```java
@Resource( name = 이름 )
```

- `name`: beans에 등록한 이름을 지정한다. 생략 가능.

설명

IO.Resource - File 타입 지원

```xml
<util:map id="fileResourceMap" key-type="java.lang.String" value-type="org.springframework.core.io.Resource">
    <entry key="goodsImageFolder" value="file:C:\dev\workspace\bo\WebContent\upload\goodsDetail\"/>
</util:map>
```

```java
@javax.annotation.Resource(name = "fileResourceMap")
private Map<String, Resource> fileResourceMap;

public void save() {
    File uploadFolder = fileResourceMap.get("goodsImageFolder").getFile();
    // C:\dev\workspace\bo\WebContent\upload\goodsDetail\
}
```
