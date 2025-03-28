---
layout: post
date: 2019-05-23 11:48:00 +0900
title: '[HTML] HTML5 노트'
categories:
  - html
tags:
  - html
  - html-standard
  - s
  - del
  - ins
  - strike
  - note
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [HTML Standard \| WHATWG](https://html.spec.whatwg.org/)


## 개요 

HTML5 관련 다 모음.


## HTML5 템플릿

```html
<!DOCTYPE html>
<html>
<head>
  <title>Me is title</title>
  <meta charset="UTF-8">
</head>
<body>
    <div class="content">Hello world!</div>
</body>
</html>
```

좀 더 이것저것 추가하면:

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <title>Me is title</title>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="icon" type="image/png" href="/favicon.ico">
  <link rel="shortcut icon" href="/favicon.ico">
</head>
<body>
    <div class="content">Hello world!</div>
</body>
</html>
```

참고로 케릭터 셋 설정은 HTML5 이전엔 아래처럼 쓰였다:

```html
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
```


## data-* custom data attributes

- [data-\* \| MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/data-*)
- [HTMLElement.dataset \| MDN](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset)
- [w3schools: HTML data-* Attributes](http://www.w3schools.com/tags/att_global_data.asp)
- [http://www.sitepoint.com/managing-custom-data-html5-dataset-api/](http://www.sitepoint.com/managing-custom-data-html5-dataset-api/)

HTML5에서 정의된 global attribute 중 하나. '사용자 정의 속성' 혹은 '전용 데이터 속성'이라 부른다.

```html
<div id="soldier" data-recent-status="idle"></div>
```

```js
document.querySelector('#soldier').getAttribute('data-recent-status'); // 'idle'
document.querySelector('#soldier').dataset; // DOMStringMap {recentStatus: "idle"}
```

이 외 HTML5에서 추가된 [global attribute](http://webdir.tistory.com/89)들이 있으니 확인해볼 것.


## required attribute

- [http://www.w3schools.com/tags/att_input_required.asp](http://www.w3schools.com/tags/att_input_required.asp)
- [https://developer.mozilla.org/en-US/docs/Web/API/HTMLSelectElement/checkValidity](https://developer.mozilla.org/en-US/docs/Web/API/HTMLSelectElement/checkValidity)
- [https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/reportValidity](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/reportValidity)

required는 HTML5부터 추가된 유효성 검사 속성이다. 적용 가능한 HTML 태그는 다음과 같다:

- `<textarea>`
- `<input type="text">`
- `<input type="search">`
- `<input type="url">`
- `<input type="tel">`
- `<input type="email">`
- `<input type="password">`
- `<input type="date">`
- `<input type="number">`
- `<input type="checkbox">`
- `<input type="radio">`
- `<input type="file">`

submit 이벤트가 발생했을 때 브라우저(물론 HTML5를 지원하는 브라우저)는 required가 명시된 필드에 값이 입력되었는지 확인한다. 만약 값이 없으면 입력을 요구하는 메시지가 브라우저의 툴팁으로 표시되며 submission은 중단될 것이다. 이 과정은 onsubmit 핸들러보다 먼저 이뤄진다.

자바스크립트의 [`HTMLFormElement.submit()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/submit) 메서드는 submit 이벤트를 발생시키지 않는다. 따라서 required를 온전히 적용하려면 form 내부에 submit 버튼이 있어야 하고 이 버튼을 통해 진행되도록 해야 한다.

form 바깥에 있는 버튼과 스크립트만으로 required를 작동시킬 수 없을까 해서 아래처럼 해봤는데:

```html
<script>
  function test01(event) {
    alert('submitted');
    event.preventDefault();
  }

  function test02(event) {
    var form = document.testForm;
    if (form.checkValidity()) {
      form.onsubmit(event);
    }
    else {
      form.reportValidity();
    }
  }
</script>
<form name="testForm" onsubmit="test01(event)">
    <input type="text" required><br>
    <button type="submit">SUBMIT</button><br>
</form>
<button type="button" onclick="test02(event)">BUTTON</button>
```

거의 똑같이 돌아가긴 하지만 [`HTMLFormElement.reportValidity()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/reportValidity)는 IE에서 지원하지 않는 문제가 있다.


## button

- [The Button element \| MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)
- [https://www.w3schools.com/tags/tag_button.asp](https://www.w3schools.com/tags/tag_button.asp)

`<button>`태그에 type 속성을 명시하지 않을 경우 type의 기본값은 submit으로 설정된다. 이 때문에 `<form>`안에 위치하게 되면 버튼 클릭 시 `HTMLFormElement.submit()` 메서드가 작동하게 된다.

따라서 아래처럼 작성해야 submit을 방지할 수 있다:

```html
<form>
    <button type="button"></button>
</form>
```


## form

- [The Form element \| MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form)
- [https://www.w3schools.com/tags/tag_form.asp](https://www.w3schools.com/tags/tag_form.asp)

**TODO**


## fieldset

```html
<fieldset>
    <legend>제목</legend>
    내용
</fieldset>
```

form field set 태그라고 하며 웹페이지의 내용을 그룹화 하는데 사용된다. form 태그와 같이 사용할 경우에, fieldset 태그에 disabled 속성을 주면 하위 폼 컨트롤들이 모두 비활성화 된다.

```html
<form method="post" action="#">
    <fieldset>
        <legend>주문 상세 페이지</legend>
        <table cellspacing="0" cellpadding="0">
            <caption>주문 상세 페이지</caption>
            <colgroup>
                <col width="15%"/>
                <col width="85%"/>
            </colgroup>
            <tbody>
                <tr>
                <!-- 이하 생략 -->
```


## label

```html
<form action="">
    <fieldset>
        <legend>하이</legend>
        <input type="checkbox" id="a1"/><label for="a1" title="풍선도움말">남자</label>
        <input type="checkbox" id="a2"/><label for="a2">여자</label>
    </fieldset>
</form>
```

체크 박스, 라디오 버튼 등과 함께 사용하는 태그로 폼 컨트롤을 직접 선택하지 않고 텍스트만으로도 폼 요소를 선택할 수 있게 해준다. title 속성에 값이 있을경우 마우스 오버 시 툴팁이 나타난다.


## input

**TODO**

### input 태그가 엔터로 submit 이벤트를 발동시키는 조건

1. `<form>` 태그가 직계 조상으로 존재해야 한다.
2. `<button type="submit">` 혹은 `<input type="submit">` 태그가 있어야 한다.
3. 만약 `<input>` 태그가 하나인 경우 2번 조건은 무시된다.
4. `<textarea>`와 `<select>` 태그는 `<input>` 태그로 간주되지 않는다. 따라서 `<input>` 하나와 `<textarea>` 혹은 `<select>` 가 있는 경우라면 `<input>` 태그 하나만 있는 것과 같다.
5. `<button type="button">` 태그도 `<input>`으로 세지 않는다.
6. `display:none` 상태는 관계 없음

그래서 조합하면 가능한 경우로:

```html
<form>
  <input>
<form>
```

```html
<form>
  <input>
  <textarea>
  <select>
<form>
```

```html
<form>
  <input>
  <button type="button">
<form>
```

```html
<form>
  <input>
  <input>
  ...
  <button type="submit">
<form>
```

```html
<form>
  <input>
  <input>
  ...
  <input type="submit">
<form>
```

요정도의 가지수가 나옴.

### tabindex

```html
<input type="text" tabindex="1" >
```

키보드 탭 컨트롤 시 포커스를 가져갈 순서를 지정하는 속성이다. 순서는 1이 가장 빠르며 정수로만 입력한다. `tabindex`가 0이면 가장 마지막에 도달한다. 만약 `-1`로 설정하면 해당 요소는 탭 컨트롤로 포커스를 가질 수 없다.


## 취소선(strike through line) 표현하기

- [HTML reference \| MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference)
- [w3schools: HTML Element Reference](https://www.w3schools.com/tags/default.asp)

HTML5 전에는 취소선을 표현하는 태그로 `<strike>`가 있었으나 HTML5부터 `<s>`, `<del>` 태그로 대체되었다.

`<s>` 태그는 더 이상 정답이 아니거나 정확하지 않거나 연관성이 없는 텍스트를 의미한다. (strikethrough의 약자인 모양)

`<del>` 태그는 교체되었거나 삭제된 텍스트를 의미한다. 교체되었을 경우 `<ins>` 태그로 새 텍스트를 표시한다.

여담으로 CSS로는 다음처럼 할 수 있다:

```css
p {
  text-decoration: line-through;
}
```


## 메타 태그로 페이지 가로폭 줄이기(모바일)

```html
<meta name="viewport" content="width=device-width, initial-scale=1,
        maximum-scale=1, minimum-scale=1.0, user-scalable=no">
```

![](/images/all-k.jpg)


## Chrome에서 file input이 초기화됨

마지막 확인 날짜: 2023-07-26

초로미에서는 이미 첨부된 파일이 있는 상태에서 파일 선택을 취소할 때 사용자 첨부 파일이 지워지는 현상이 있다. 확인하는 방법은 다음과 같다:

1. 우선 아무 파일이나 첨부한다.
2. 다시 '파일 선택' 혹은 '찾아보기...' 등의 버튼을 눌러 파일 탐색 창을 띄운다.
3. 파일 탐색 창에서 취소 버튼을 누른다.

이렇게 하면 앞서 첨부했던 파일이 사라진다. 마치 초기화한 것처럼. 

초로미 계열인 네이버 웨일, 마이크로소프트 엣지에서도 같은 현상이 발생한다. 파이어폭스는 파일 첨부를 취소해도 사용자 첨부 파일이 삭제되지 않는다.

검색해보니 이 현상은 [2008년부터 제기된 이슈](https://bugs.chromium.org/p/chromium/issues/detail?id=2508)이고 WonFix(Closed) 상태(2015년 7월에 변경됨)인 것을 보아 영영 고쳐질 일은 없을것 같다. 재미있게도 [파이어폭스 측의 어떤 개발자는 초로미의 방식이 마음에 들었는지 이를 구현한 패치를 제출했다](https://bugzilla.mozilla.org/show_bug.cgi?id=431098).

[스택오버플로우의 친구들은 초로미가 사용자 첨부 파일을 삭제하지 못하도록 복제본을 다시 끼워넣는 방법을 제안](https://stackoverflow.com/questions/17798993/input-type-file-clearing-file-after-clicking-cancel-in-chrome)한다. 초로미에서 파일 첨부를 취소하면 file input이 change 이벤트를 발생시키고, 이 시점의 `FileList`를 확인해 보면 길이가 0인데, 이걸 이용하면 된다:

```js
function handleFileChange(e) {
  console.log('files:' , e.target.files); // files: FileList {length: 0}
  // 파일 첨부 후 다시 띄운 파일 탐색창에서 취소했을 때 files.length는 0
  // 파이어폭스애선 취소했을 때 change 이벤트가 발생하지 않기 때문에 이 함수로 진입할 일이 없다.
}
```

하지만 이 방법은 복제본을 다루는 코드 작성이 번거롭기 때문에 더 쉬운 방법인:

```html
<input type="file" onchange="handleFileChange(event)">
<script>
function handleFileChange(e) {
  let file = e.target.files[0];
  if (!file) {
    return;
  }
  var reader = new FileReader();
  reader.addEventListener('loadend', e => {
    let actualResponse = e.target.result;
  });
  reader.readAsDataURL(file);
}
</script>
```

`FileReader`로 읽어들여 갖고 있다가 `FormData`로 전송하는 편이 좋다.


## 꼐속...

