---
layout: post
date: 2013-11-21 11:49:26 +0900
title: 'jQuery: node 생성하기'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - node
---

* Kramdown table of contents
{:toc .toc}

자바스크립트에선 다음처럼 `Document.createElement()` 함수로 노드를 생성한다:

```js
function makeone() {
  var p = document.createElement('p');
  p.id = 'who';
  p.appendChild(document.createTextNode('텍스트 노드'));
//  p.innerHTML = '텍스트<br/>노드';
//  p.innerText = '텍스트 노드';  // FF에서 사용불가

  document.getElementById('result').appendChild(p);
  alert(document.getElementById('who') == p);  // true, 두번째 추가부터 false
}
```

jQuery라면 다음의 방법 중 하나를 사용한다.

#### String

```js
$("#do3").click(function() {
  $("#result3").append("<div id='field'>안녕? 나는 div</div>");
});
```
#### jQuery Object

```js
var obj = { label: 'some object' };
$("#do2").click(function() {
  $("#result2").append($('<div>', {
    id: 'field',
    text: '새로 생성된 div',
    data-obj: obj;
  }));
});
```

**주의**: 태그 속성을 위 처럼 작성하면 자바스크립트 객체(위 예시에서는 `obj`)는 객체 자체가 아니라 객체의 `toString()` 실행 결과가 할당된다.

#### jQuery Object-Function 사용

```js
$("#do2").click(function() {
  $("#result2").append(function() {
    return $('<div>', {id: 'field', text: '새로 생성된 div'});
  });
});

// 위를 응용하면...
function createEle(i, value) {
    return $('<div>', { id: 'field' + i, text: value});
}

jQuery(document).ready(function() {
    $("#do").click(function() {
        $('#result').append(createEle(1, 'hello, world!'));
    });
});
```

#### method call chaining

```js
var obj = { label: 'some object' };
$("#do3").click(function() {
    $("<img>").attr("src","test.gif").data('obj', obj).append("body");
});
```
