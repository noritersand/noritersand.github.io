---
layout: post
date: 2014-01-17 15:58:26 +0900
title: '[jQuery] jQuery Object 식별 방법'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - object
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://learn.jquery.com/using-jquery-core/jquery-object/](https://learn.jquery.com/using-jquery-core/jquery-object/)

## selector가 문자열인지 객체인지 판단

```
jQuery.type( obj )
```

```js
var selector1 = 'body';
var selector2 = document.querySelector('body');
var selector3 = $('body');

$.type(selector1); // 'string'
$.type(selector2); // 'object'
$.type(selector3); // 'object'
```

`typeof`와 차이점은 [여기서](https://stackoverflow.com/questions/19921838/difference-between-jquery-type-and-typeof-which-is-faster).

## jQuery 객체인지 판단

```js
var selector1 = document.querySelector('body');
var selector2 = $('body');

selector1 instanceof $; // false
selector2 instanceof $; // true
selector2 instanceof jQuery; // true
```

`$()` 혹은 `jQuery()`는 셀렉터이면서 jQuery 객체의 생성자이기 때문에 가능한 방법.

## 동등 비교

```html
<!DOCTYPE html>
<html>
<body>
<div id="target">안녕!</div>
</body>
</html>
```

페이지가 위와 같을때 자바스크립트에선:

```js
document.getElementById('target') == document.getElementById('target');  // true
document.getElementById('target') === document.getElementById('target');  // true
```

이처럼 DOM selector를 이용해 선택한 요소를 동등비교에 사용할 수 있으나 jQuery에선:

```js
$('#target') == $('#target');  // false
$('#target') === $('#target');  // false
```

위처럼은 불가능하다. 이는 jQuery 선택자가 반환하는 객체가 기본적으로 배열의 형태를 띄기 때문이다. 따라서 jQuery로 선택한 요소를 비교하려면 자바스크립트 객체로 변환하거나 배열에서 꺼내야 한다:

```js
// return javascript DOM element
var a = $('#target')[0];
var b = $('#target').get(0);
var c = $('#target').get()[0];
a === b // true;
b === c // true;
c === a // true;
```

## 자신이 아닌것 찾기

<ul>
    <li class="numeric">1</li>
    <li id="target2" class="numeric">2</li>
    <li class="numeric">3</li>
    <li class="numeric">4</li>
</ul>

위와 같을 때 target2가 아닌 `<li>`를 선택하는 방법이다.

```js
$('li:not(#target2)').length  // 3
$('li').not('#target2').length  // 3
```

이렇게 `.not()`[^1] 혹은 `:not()`으로 특정 요소를 제거하거나,

```js
$('#target2 ~ li').length  // 2
$('#target2').siblings().length  // 3
$('#target2').siblings('li').length  // 3
```

혹은 `.siblings()`[^2]처럼 자신을 제외한 요소를 선택하는 셀렉터를 사용하는 방법이 있다.

위 예시 중 Next Siblings Selector`~`는 마지막으로 선택한 요소 이후부터 탐색한다.

## 이벤트 발생 시 특정 요소가 맞는지 확인하기

```
.is( selector )
```

[`is()`](https://api.jquery.com/is/)는 지정된 요소와 jQuery 객체가 동등한지 비교하는 필터링 메서드다. selector에는 CSS selector expression, 혹은 jQuery 객체나 배열이 올 수 있는데 선택한 요소가 selector와 일치하거나 그 일부일 경우 true를 반환한다.

```html
<script>
  $(document).ready(function() {
    $('body').click(function(event) {
      var flag = $(event.target).is('#yooo');
      $('#result1').val(flag);
    });

    $('body').click(function(event) {
      var flag = $(event.target).is($('body *').not('#yooo'));
      $('#result2').val(flag);
    });
  });
</script>

<body>
    <ul>
        <li>나말고</li>
        <li id="yooo">클릭</li>
        <li>나말고</li>
        <li>나말고</li>
    </ul>
    is #yooo: <input type="text" id="result1"/><br/>
    is not #yooo: <input type="text" id="result2"/>
</body>
```

[^1]: 객체 배열에서 지정된 요소를 삭제한다.
[^2]: 자신을 제외한 같은 부모를 갖는 모든 형제요소를 반환한다.
