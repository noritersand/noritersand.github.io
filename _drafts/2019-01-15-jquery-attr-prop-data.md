---
layout: post
date: 2019-01-15 15:24:00 +0900
title: 'jQuery: attr(), prop(), data()'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - attr
  - prop
  - data
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://api.jquery.com/attr/](https://api.jquery.com/attr/)
- [https://api.jquery.com/prop/](https://api.jquery.com/prop/)
- [https://api.jquery.com/data/](https://api.jquery.com/data/)

## .attr(name, value),<br>.attr(object)

태그 속성을 변경한다.

## .prop(name, value)

TODO

## .data(name, value),<br>.data(object)



태그 데이터를 변경한다.

```js
$(element).data('a', 123);
$(element).data({ 'b': 456 });
```

```js
var obj = { label: 'some object' };
$("#do3").click(function() {
    $("<img>").attr("src","test.gif").data('obj', obj).append("body");
});
```

단순 문자열이 아니라 자바스크립트 객체를 담을 수도 있다.

태그 속성을 아래처럼 작성하면 이건 각 속성을 `.attr()`로 할당한것과 같다.

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

## 셋의 차이점

- `.attr()`은 문자열만 할당할 수 있다.
- `.attr()`로 변경한 데이터는 prop에 영향이 있을 수도 있고 없을 수도 있다. (테스트 필요)
- `.prop()`은 attr에 영향이 있다.
- `.prop()`으로 변경한 데이터는 data에 영향 음슴.
- `.data()`로 데이터를 변경하면 attr은 변화 없음.
- `.attr()`로 변경한 데이터는 data에도 영향이 있다. 단, 애초에 없던 속성을 처음 등록할 때만 영향이 있고 이 후로는 따로 논다.
- `.attr()`의 파생형 `.removeAttr()`이 있는데, 이 함수의 성격상 `.data()`에 영향이 없다고 봐야한다. (이미 등록된 속성을 지우는건데 해당 속성은 data로써도 존재할 테니까)

세 번째가 매우 중요한데, 만약 태그가 아래와 같을 때:

```html
<div id="no-one" data-name="drop-zone"></div>
```

태그 랜더링 후 스크립트로 속성값을 수정할 때 `.data()`를 사용하면 attr이 바뀌는게 아니기 때문에 jQuery attribute selector로 찾을 수 없다:

```js
$('#no-one').data('name', 'forget-me-not');
console.log($('[data-name="forget-me-not"]').length); // 0
```

따라서 이런 경우에는 `.attr()`을 사용한다:

```js
$('#no-one').attr('data-name', 'forget-me-not');
console.log($('[data-name="forget-me-not"]').length); // 1
```

`.data()`로 데이터를 변경했으면 attribute selector 대신 filtering을 해야 한다.

```js
$('#no-one').data('name', 'forget-me-not');
$('div').filter(function() {
  if ($(this).data('name') == 'forget-me-not') {
    return true;
  }
});
```

## 결론

태그에 어쩔 수 없을 때만 `.attr()`을 사용하되, 문자열만 가능하다는 점을 주의할 것. `.data()`가 특정 태그의 모든 data를 object로 가져오는 기능이 있어서 `.attr()`보다 좋음.

그리고 두 함수를 섞어 쓰지 않는게 좋다.
