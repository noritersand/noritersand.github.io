---
layout: post
date: 2021-04-02 01:15:03 +0900
title: '[jQuery] 지연된 객체 Deferred Object'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - deferred
  - when
  - then
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [jQuery API: Deferred Object](https://api.jquery.com/category/deferred-object/)

Deferred는 사실 자바스크립트의 [`Promise`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)와 거의 동일하다. 다만, 이쪽은 IE에서도 쓸 수 있다는 장점이 있다.

간단한 예시로 맛을 좀 보자:

```js
function fn() {
  $('#tgt').css('color', 'red');
  alert('색 바뀌기 전에 뜨는 경고창');
}
```

이 코드를 실행해보면 `#tgt`에 스타일이 적용되기 전에 경고창이 발생한다. 그래서 1ms 후에 경고창이 뜨되, `jQuery.Deferred()`를 활용하는 방식으로 작성하면:

```js
function changeStyle() {
  $('#tgt').css('color', 'red');

  var dfd = $.Deferred();
  setTimeout(function() {
    dfd.resolve();
  }, 100);
  return dfd;
}

function fn() {
  $.when(
    changeStyle()
  ).then(function() {
    alert('색 바뀐 후에 뜨는 경고창');
  });
}
```

이렇게 된다.
