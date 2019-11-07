---
layout: post
date: 2017-02-03 13:39:10 +0900
title: '[JavaScript] node selector'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - selector
---

* Kramdown table of contents
{:toc .toc}

## querySelector

querySelector는 CSS selector의 문법과 호환됨.

```js
document.querySelectorAll('div'); // 모든 div 태그 선택, 배열로 반환.
const editor = document.querySelector('#TistoryEditor'); // id가 'TistoryEditor'인 태그 반환
editor.tagName; // 태그명
editor.textContent; // 바디가 있는 태그일 경우 태그 내의 텍스트 반환
editor.attributes; // 태그의 모든 속성을 배열로 반환
editor.attributes['style'] // 태그에서 이름이 'style'인 속성 반환
```

## 폼 셀렉터

```js
document.forms.loginForm
```



## 프레임 셀렉터

```js
window.loginFrame

window.frames.loginFrame

window.frames.length // 현재 창의 프레임 수

// window.frames는 window와 동일한 객체다.

window.frames === window  // true
window.frames[0] === window // false
```

```html
<body>
    <form name="myform">
        <input type="text" name="test1"/>
        <input type="password" name="test2"/>
        <input type="hidden" name="test3"/>
    </form>

    <div id="result"></div>
</body>
```

id를 통한 접근:

```js
document.getElementById('result')
```

name을 통한 접근:

```js
document.myform.test2
document.getElementsByName('test1')[0]
document.all['test2'] // 비표준
document.forms[0].elements['test3'] // 비표준
```
