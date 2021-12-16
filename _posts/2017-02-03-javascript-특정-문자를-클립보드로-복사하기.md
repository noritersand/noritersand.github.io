---
layout: post
date: 2017-02-03 13:41:10 +0900
title: '[JavaScript] 특정 문자를 클립보드로 복사하기'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - clipboard
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Document.execCommand](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand)
- [Copying text to clipboard with JavaScript | Hacker Noon](https://hackernoon.com/copying-text-to-clipboard-with-javascript-df4d4988697f)

## execCommand

```
document.execCommand(aCommandName [, aShowDefaultUI, aValueArgument] )
```

`command`를 지원하지 않거나 사용할 수 없는 상태면 `false` 반환.

복사할 문자는 반드시 selection이 가능한 입력필드(`<input>`, `<textarea>`)의 값이어야 하며 숨겨진 상태가 아니어야 한다. 따라서 `<input type="hidden">`은 적용 불가.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>copy to clipboard</title>
<script>
function copyToClipboard() {
  var input = document.querySelector('input');
  try {
    input.select();
    // returnValue: A Boolean that is false if the command is not supported or enabled.
    var returnValue = document.execCommand('copy');
    console.debug(returnValue);
    if (!returnValue) {
      throw new Error('copied nothing');
    }
    alert('복사 되었습니다.');
  } catch (e) {
    prompt('Copy to clipboard: Ctrl+C, Enter', input.value);
  }
}
</script>
</head>
<body>
<input type="text" value="아무거나 쓰세요.">
<button type="button" onclick="copyToClipboard()">그리고 날 눌러</button>
</body>
</html>
```

만약 입력필드가 없는 상황이라면 다음처럼 `<textarea>`를 만들었다가 지우는 방식을 쓴다. 이 때 화면이 깜빡거리지 않도록 화면 밖에서 발생하도록 함:

```js
/**
 * 클립보드에 val을 복사. 복사에 실패하면 Error 던져짐.
 * @param {string} val 복사할 문자열
 */
function copyToClipboard(val) {
  const element = document.createElement('textarea');
  element.value = val;
  element.setAttribute('readonly', '');
  element.style.position = 'absolute';
  element.style.left = '-9999px';
  document.body.appendChild(element);
  element.select();
  var returnValue = document.execCommand('copy');
  document.body.removeChild(element);
  if (!returnValue) {
    throw new Error('copied nothing');
  }
}
```

`document.execCommand('copy')` 명령은 사용자 생성 이벤트(브라우저 사용자의 직접 행동에 의해 발동하는 이벤트)가 아니면 일부 브라우저에서 실행 불가. 실제로 파이어폭스 콘솔창에 직접 명령어를 입력하면: _경고: 짧게 실행되는 사용자 생성 이벤트 핸들러 안에서 호출되지 않았기 때문에 document.execCommand(‘cut’/‘copy’)가 거부되었습니다._ 라고 함.
