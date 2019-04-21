---
layout: post
date: 2015-04-06 15:33:00 +0900
title: 'HTML5: data-* Attributes'
categories:
  - html
tags:
  - html
  - html5
  - data-*
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://developer.mozilla.org/ko/docs/Web/API/HTMLElement/dataset](https://developer.mozilla.org/ko/docs/Web/API/HTMLElement/dataset)
- [http://www.w3schools.com/tags/att_global_data.asp](http://www.w3schools.com/tags/att_global_data.asp)
- [http://www.sitepoint.com/managing-custom-data-html5-dataset-api/](http://www.sitepoint.com/managing-custom-data-html5-dataset-api/)

HTML5부터 제공되는 global attribute 중 하나. 사용자 정의 속성이라고 보면 된다.

```html
<div id="soldier" data-recent-status="idle"></div>
```

```js
document.querySelector('#soldier').getAttribute('data-recent-status'); // 'idle'
document.querySelector('#soldier').dataset; // DOMStringMap {recentStatus: "idle"}
```

이 외 HTML5에서 추가된 global attribute: [http://webdir.tistory.com/89](http://webdir.tistory.com/89)
