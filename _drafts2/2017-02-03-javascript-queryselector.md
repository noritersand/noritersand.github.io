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

document.querySelector

태그의 아이디, 클래스, 이름 등으로 태그 객체를 반환하는 메서드로 jQuery의 $("셀렉터") 와 비슷하다.

셀렉터는 CSS에서 사용하는 태그 셀렉터 ("#아이디" , ".클래스이름", "태그이름", "태그이름 하위태그이름" 등 ) 을 사용한다.

이때, 클래스이름이나 태그이름으로 불러와도 배열이 아닌, 맨 앞 객체 하나만 불러오게 된다.

해당 메서드는 Internet Explorer 8, FireFox 3.5 이상의 브라우저에서 지원된다.

사용법

var container = document.querySelector('#container'); // id selector

var box = document.querySelector('.box'); // class selector

var body = document.querySelector('body'); // tag selector

var input = document.querySelector('[name=inp_box]'); // attribute selector



document.querySelectorAll

태그의 아이디, 클래스, 이름 등으로 태그 객체를 반환하는 메서드로 jQuery의 $("셀렉터") 와 비슷하다.

셀렉터는 CSS에서 사용하는 태그 셀렉터 ("#아이디" , ".클래스이름", "태그이름", "태그이름 하위태그이름" 등 ) 을 사용한다.

document.querySelector와 다르게 여러 개의 태그 객체를 담는 배열을 반환한다. (아이디로 불러왔을 경우에도 배열 반환)

해당 메서드는 Internet Explorer 8, FireFox 3.5 이상의 브라우저에서 지원된다.

사용법

var containerArr = document.querySelectorAll('#container'); // id selector, 배열

var boxArr = document.querySelectorAll('.box'); // class selector, 배열

var divArr = document.querySelectorAll('div'); // tag selector, 배열

var radioArr = document.querySelectorAll('[name=radio_btn]'); // attribute selector, 배열
