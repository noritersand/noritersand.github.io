---
layout: post
date: 2017-02-03 13:42:00 +0900
title: 'JavaScript: querySelector()'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - queryselector
---

* Kramdown table of contents
{:toc .toc}

#### 브라우저 호환

- IE8 이상

## document.querySelector

```js
document.querySelector( selector )
```

태그의 아이디, 클래스, 이름 등으로 태그 객체를 반환하는 메서드.

`selector`는 [CSS 셀렉터](https://www.w3schools.com/cssref/css_selectors.asp)를 사용한다. 이때, 클래스이름이나 태그이름으로 불러와도 배열이 아닌, 맨 앞 객체 하나만 불러오게 된다.

```js
var container = document.querySelector('#container'); // id selector
var box = document.querySelector('.box'); // class selector
var body = document.querySelector('body'); // tag selector
var input = document.querySelector('[name=inp_box]'); // attribute selector
var dropZone = document.querySelector('div[data-name="drop-zone"]'); // attribute selector
```

## document.querySelectorAll

```js
document.querySelectorAll( selector )
```

태그의 아이디, 클래스, 이름 등으로 태그 객체를 반환하는 메서드.

`selector` [CSS 셀렉터](https://www.w3schools.com/cssref/css_selectors.asp)를 사용한다. `document.querySelector`와 다르게 여러 개의 태그 객체를 담는 배열을 반환한다. 아이디로 찾을 때도 마찬가지다.

```js
var containerArr = document.querySelectorAll('#container'); // id selector, 배열
var boxArr = document.querySelectorAll('.box'); // class selector, 배열
var divArr = document.querySelectorAll('div'); // tag selector, 배열
var radioArr = document.querySelectorAll('[name=radio_btn]'); // attribute selector, 배열
```
