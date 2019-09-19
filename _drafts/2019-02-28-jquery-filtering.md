---
layout: post
date: 2019-02-28 16:37:00 +0900
title: 'jQuery: filtering'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - traversing
  - filtering
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://api.jquery.com/category/traversing/filtering/](https://api.jquery.com/category/traversing/filtering/)

## .filter()

```
.filter( function( index, element ) )
```

- `function`: 요소만큼 반복할 함수
- `index`: 선택 요소 배열의 인덱스
- `element`: 선택 요소 배열의 각 요소

셀렉터로 선택한 요소만큼 반복하면서 함수에서 true가 반환되지 않으면 해당 요소를 제거하여 반환한다.

```js
var $previous = $('.navlink').filter(function(idx, ele) {
  return $(this).data("selected") == true
});
```

## 메서드

내용

끗.
