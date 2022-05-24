---
layout: post
date: 2022-01-02 00:40:56 +0900
title: '[JavaScript] DOM과 자바스크립트: Selection'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - dom-standard
  - selector
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [somewhere](somewhere)

## 개요

TODO DOM(Document Object Model) 어쩌구 저쩌구

내 맴대로 분류한 DOM 선택 관련 API 정리.

이미 Element를 선택한 상태에서 탐색하는 기능은 Traversing으로 분류함.

## 옛날에나 쓰던 요소 선택하기

```html
<body>
    <form name="myform">
        <input type="text" name="test1"/>
        <input type="password" name="test2"/>
        <input type="hidden" name="test3"/>
    </form>

    <div id="result"></div>
</body>

<script>
document.getElementById('result')

document.myform.test2;
document.getElementsByName('test1')[0];
document.all['test2']; // 비표준
document.forms[0].elements['test3']; // 비표준
</script>
```

## Document.querySelector()

CSS selector 사용 가능.

```js
document.querySelectorAll('div'); // 모든 div 태그 선택, 배열로 반환.
const editor = document.querySelector('#TistoryEditor'); // id가 'TistoryEditor'인 태그 반환
editor.tagName; // 태그명
editor.textContent; // 바디가 있는 태그일 경우 태그 내의 텍스트 반환
editor.attributes; // 태그의 모든 속성을 배열로 반환
editor.attributes['style']; // 태그에서 이름이 'style'인 속성 반환
```

## 폼 셀렉터

```js
document.forms.loginForm
```

### 프레임 셀렉터

```js
window.loginFrame
window.frames.loginFrame
window.frames.length // 현재 창의 프레임 수
// window.frames는 window와 동일한 객체다.
window.frames === window  // true
window.frames[0] === window // false
```

## TODO

- toggleAttribute

## 꼐속...
