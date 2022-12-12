---
layout: post
date: 2022-01-02 00:40:56 +0900
title: '[JavaScript] DOM과 자바스크립트: Manipulation'
categories:
  - javascript
tags:
  - javascript
  - web-api
  - dom-standard
  - manipulation
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Node](https://developer.mozilla.org/en-US/docs/Web/API/Node)
- [\[MDN\] Web APIs](https://developer.mozilla.org/en-US/docs/Web/API)


## 개요

TODO DOM(Document Object Model) 어쩌구 저쩌구

내 맴대로 분류한 DOM 조작 관련 API 정리.


## append

### Node.appendChild()

```html
<button onclick="appendIframe()">눌러</button>
<div id="target"></div>

<script>
function appendIframe() {
  var iframe = document.createElement('iframe');
  iframe.src = 'http://noritersand.tistory.com';
  document.querySelector('#target').appendChild(iframe);
}
</script>
```


## prepend


## before


## after


## class

```html
<div class="active testme" id="here"></div>

<script>
let $here = document.querySelectorAll('#here');
$here.className; // 'active testme'
$here.classList; // DOMTokenList(2) ['active', 'testme', value: 'active testme']
</script>
```

태그의 클래스는 `Node.className` 혹은 `Node.classList` 프로퍼티로 확인할 수 있다.

`Node.classList`는 단순 배열이 아닌 `DOMTokenList`를 반환한다.

### DOMTokenList

`DOMTokenList`는 `Set` 타입과 유사하며, 마크업에서 스페이스바로 구분한 값을 요소로 갖는다. `Node.classList` 외에 `HTMLLinkElement.relList`도 `DOMTokenList`를 반환한다.

### 클래스 추가/삭제

`Node.classList`로 `DOMTokenList`를 받아온 뒤 `.add()`로 추가, `.remove()`로 삭제한다:

```html
<div class="active testme" id="here"></div>

<script>
let $here = document.querySelectorAll('#here');

$here.classList.add('foo');
$here.className; // 'active testme foo'

$here.classList.remove('active');
$here.className; // 'testme foo'

$here.classList.remove('foo');
$here.classList; // DOMTokenList ['testme', value: 'testme']
</script>
```


