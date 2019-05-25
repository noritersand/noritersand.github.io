---
layout: post
date: 2019-05-25 22:53:00 +0900
title: 'JavaScript: jQuery 탈출 프로젝트 - 자바스크립트로 DOM 조작하기'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - jQuery 탈출 프로젝트
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [somewhere](somewhere)

## DOM 삭제

```js
var list = document.querySelectorAll('div._9AhH0')
for (let i in list) {
	let ele = list[i];
	console.log(ele);
	ele.parentNode.removeChild(ele)
}
```
