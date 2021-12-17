---
layout: post
date: 2014-02-05 18:17:00 +0900
title: '[jQuery] 파일 입력필드 초기화, reset input type="file"'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - input
  - file
  - form-field
  - reset
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://stackoverflow.com/questions/1043957/clearing-input-type-file-using-jquery/1043969#1043969](https://stackoverflow.com/questions/1043957/clearing-input-type-file-using-jquery/1043969#1043969)

```html
<script>
$(document).ready(function() {
  var domEleArray = [$('#attachFile').clone()]; // 원본 복사
  $('#btn_delAttach').click(function() {
    domEleArray[1] = domEleArray[0].clone(true); // 쌔거(0번) -> 복사(1번)
    $('#attachFile').replaceWith(domEleArray[1]);
  });
});
</script>

<body>
    <form id="submitForm">
        <input type="file" id="attachFile"/>
        <button type="button" id="btn_delAttach">삭제</button>
    <form>
</body>
```

1. 문서가 처음 로딩 될 때 `<input>`을 `clone()`으로 복사해 배열에 담는다.
1. 삭제버튼 클릭 시 value가 없는 상태인`<input>`의 복사본과 화면에 보이는 `<input>`을 바꿔치기 한다.
1. 삭제는 한 번 이상일 수 있다. 따라서 `replaceWith()` 사용 전에 최초에 생성된 클론의 복사본을 하나 더 만들어둬야 한다.


위 방법이 번거롭다면 다음처럼도 가능하다:

Firefox, Chrome:

```js
$('#attachFile').val('');
```

IE:

~~파일 첨부 기능을 구현하지 않는다.~~ IE는 form `reset()`이 안먹는다. `<input>`에 보이는 파일이름은 지워지지만 DOM 객체의 files 프로퍼티는 남아있는 상태.
