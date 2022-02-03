---
layout: post
date: 2017-02-03 13:46:00 +0900
title: '[JavaScript] History'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - html-standard
  - history
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/en-US/docs/Web/API/History_API#The_replaceState%28%29_method](https://developer.mozilla.org/en-US/docs/Web/API/History_API#The_replaceState%28%29_method)

## location.replace: 페이지 이동 이력 덮어쓰기

history는 윈도우 내에 존재하는 프레임들의 페이지 이동까지 기억한다. 이를 무시하고 싶다면 페이지 이동을 원하는 프레임의 src를 직접 바꾸거나 submit 하지 말고 다음 둘 중의 하나의 방법을 사용한다:

```js
window.frames['FRAME_NAME'].location.replace('TARGET_URL')
document.querySelector('TARGET_FRAME_SELECTOR').contentWindow.location.replace('TARGET_URL')
```

## 주요 메서드

### history.replaceState()

```
history.replaceState(data, title, url)
```

- `data`:
