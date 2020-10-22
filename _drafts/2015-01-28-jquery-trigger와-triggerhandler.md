---
layout: post
date: 2015-01-28 14:52:00 +0900
title: '[jQuery] .trigger()와 .triggerHandler()'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - trigger
  - triggerhandler
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://api.jquery.com/trigger/](https://api.jquery.com/trigger/)
- [https://api.jquery.com/triggerHandler/](https://api.jquery.com/triggerHandler/)


## .trigger()

```
.triggerHandler( eventType [, extraParameters ] )
```

- `eventType`: asdasd asd
- `extraParameters`: sadasd asd

`.trigger()`는 이벤트 자체를 발생. `.triggerHandler()`는 특정 이벤트에 할당된 모든 핸들러를 실행한다. 단, 핸들러만 실행되고 이벤트는 발생하지는 않는다. 가령 `.triggerHandler('submit')`의 경우 'submit' 이벤트에 반응하도록 설정된 핸들러가 작동하게는 하지만 실제로 submit이 이뤄지는 것은 아니다.
