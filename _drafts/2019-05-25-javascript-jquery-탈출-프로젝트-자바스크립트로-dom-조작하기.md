---
layout: post
date: 2019-05-25 22:53:00 +0900
title: 'JavaScript: jQuery 탈출 프로젝트 - 자바스크립트로 DOM 조작하기'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - jquery 탈출 프로젝트
  - dom
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

## `<script>` 태그 동적 삽입

```js
function loadScript(url, callback) {  
  var scriptEl = document.createElement('script');
  scriptEl.type = 'text/javascript';
  // IE에서는 onreadystatechange를 사용
  scriptEl.onload = function () {
    callback();
  };
  scriptEl.src = url;
  document.getElementsByTagName('head')[0].appendChild(scriptEl);
}

loadScript('example.js', function () {  
  // example.js가 로딩 완료한 시점에 실행
});
```

[코드 출처](https://d2.naver.com/helloworld/591319)

사실 위 코드는 [AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)의 필요성을 강조하기 위한 예시임.
