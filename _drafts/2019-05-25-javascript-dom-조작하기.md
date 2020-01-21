---
layout: post
date: 2019-05-25 22:53:00 +0900
title: '[JavaScript] DOM 조작하기'
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

#### 관련 문서

- [somewhere](somewhere)

## 요소 삭제

```js
var list = document.querySelectorAll('div._9AhH0')
for (let i in list) {
  let ele = list[i];
  console.log(ele);
  ele.parentNode.removeChild(ele)
}
```

## 요소 생성

```js
var newElement = document.createElement('p');
newElement.id = 'newbie';
newElement.appendChild(document.createTextNode('텍스트 노드'));
// p.innerHTML = '텍스트<br/>노드';
// p.innerText = '텍스트 노드';  // FF에서 사용불가
document.getElementById('myContents').appendChild(newElement);
alert(document.getElementById('newbie') == newElement);  // true, 두번째 추가부터 false
```

### `<script>` 태그 동적 삽입

[코드 출처](https://d2.naver.com/helloworld/591319)

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

사실 위 코드는 [AMD](https://github.com/amdjs/amdjs-api/wiki/AMD)의 필요성을 강조하기 위한 예시임.

## 요소 선택하기

### 옛날에나 쓰던거

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

### Document.querySelector()

CSS selector 사용 가능.

```js
document.querySelectorAll('div'); // 모든 div 태그 선택, 배열로 반환.
const editor = document.querySelector('#TistoryEditor'); // id가 'TistoryEditor'인 태그 반환
editor.tagName; // 태그명
editor.textContent; // 바디가 있는 태그일 경우 태그 내의 텍스트 반환
editor.attributes; // 태그의 모든 속성을 배열로 반환
editor.attributes['style']; // 태그에서 이름이 'style'인 속성 반환
```

### 폼 셀렉터

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
