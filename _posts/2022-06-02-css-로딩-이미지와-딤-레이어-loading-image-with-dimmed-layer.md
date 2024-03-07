---
layout: post
date: 2022-06-02 22:01:33 +0900
title: '[CSS] 로딩 이미지와 딤 레이어'
categories:
  - css
tags:
  - css
  - code-snippet
  - dimmed-layer
  - loading
  - z-index
  - background-image
---

* Kramdown table of contents
{:toc .toc}


## 개요

로딩 이미지와 딤 레이어를 설정하는 방법을 작성한 글

참고로 해외에서는 로딩 이미지를 스피너(spinner: 뱅뱅 도는 무언가)라고 부른다. `spinner images`로 검색해 볼 것.


## 코드

```css
#loading {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: #ffffff36;
  z-index: 9999;
  background-image: url("/image/loading.gif");
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-position: center;
}
```

```html
<div id="loading"></div>
```

```js
function onLoad() {
  let $ele = document.querySelector('#loading');
  $ele.style = 'display: block';
}

function onLoaded() {
  let $ele = document.querySelector('#loading');
  $ele.style = 'display: none';
}
```

살짝 흐릿한 레이어가 전면을 덮어서 로딩이 완료될 때까지 클릭 등을 막는 용도로 쓴다. 

여기에 대충 적당히 움직이는 이미지를 얹으면 끗.
