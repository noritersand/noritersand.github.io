---
layout: post
date: 2016-04-08 11:48:00 +0900
title: '[HTML5] required Attribute'
categories:
  - html
tags:
  - html
  - html5
  - required
  - attribute
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://www.w3schools.com/tags/att_input_required.asp](http://www.w3schools.com/tags/att_input_required.asp)
- [https://developer.mozilla.org/en-US/docs/Web/API/HTMLSelectElement/checkValidity](https://developer.mozilla.org/en-US/docs/Web/API/HTMLSelectElement/checkValidity)
- [https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/reportValidity](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/reportValidity)

#### 테스트 환경

- Edge25
- Chrome49
- 파폭45

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

submit 이벤트가 발생했을 때 브라우저(물론 HTML5를 지원하는 브라우저)는 required가 명시된 필드에 값이 입력되었는지 확인한다. 만약 값이 없으면 입력을 요구하는 메시지가 브라우저의 툴팁으로 표시되며 submit은 중단될 것이다. 이 과정은 onsubmit 핸들러보다 먼저 이뤄진다.

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

거의 똑같이 돌아가긴 하지만 [`HTMLFormElement.reportValidity()`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLFormElement/reportValidity)는 익스플로러에서 지원하지 않는 문제가 있다. 익스 없는 세상은 언제 오나.
