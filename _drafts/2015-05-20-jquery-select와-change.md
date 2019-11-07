---
layout: post
date: 2015-05-20 18:35:00 +0900
title: '[jQuery] .select()와 .change()'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - select
  - change
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://api.jquery.com/select/](https://api.jquery.com/select/)
- [https://api.jquery.com/change/](https://api.jquery.com/change/)

#### .select()

```
.select(eventHandler)
```

쫑알쫑알.

#### .change()

```
.change(eventHandler)
```

`<select>` 태그의 옵션을 변경할 때, `.value()`나 `.prop()`으로 옵션을 변경할 땐 change 이벤트가 발생하지 않는다.

따라서 옵션 선택을 스크립트로 제어할 때에는 `.change()` 혹은 `.trigger()`를 사용하여 해당 이벤트를 강제로 발생시켜줘야 할 필요가 있다.
