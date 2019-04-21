---
layout: post
date: 2017-02-03 13:41:10 +0900
title: 'JavaScript: 특정 문자를 클립보드로 복사하기'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - clipboard
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand](https://developer.mozilla.org/en-US/docs/Web/API/Document/execCommand)

## execCommand

```
document.execCommand(aCommandName [, aShowDefaultUI, aValueArgument] )
```

command가 지원되지 않거나 사용할 수 없는 상태면 false 반환.

주의: 복사할 문자는 반드시 selection이 가능한 입력필드(input, textarea 태그 중 hide 상태가 아닌것)의 값이어야 한다.

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>copy to clipboard</title>
<script>
    function fn() {
        var input = document.querySelector('input');
        try {
            input.select();
            // returnValue: A Boolean that is false if the command is not supported or enabled.
            var returnValue = document.execCommand('copy');
            console.debug(returnValue);
            if (!returnValue) {
                throw new Error();
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
<button type="button" onclick="fn()">그리고 날 눌러</button>
</body>
</html>
```
