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

이미지는 대충 적당히 움직이는 GIF 파일로 고르면 끗.
