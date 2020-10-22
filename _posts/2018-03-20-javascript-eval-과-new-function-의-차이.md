---
layout: post
date: 2018-03-20 16:58:02 +0900
title: '[JavaScript] eval()과 new Function()의 차이'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - eval
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://stackoverflow.com/questions/4599857/are-eval-and-new-function-the-same-thing](http://stackoverflow.com/questions/4599857/are-eval-and-new-function-the-same-thing)

```js
function test1() {
  var a = 111;
  eval('console.debug(a); a = 222;'); // 111
  console.debug(a); // 222
}
test1();

function test2() {
  var b = 333;
  new Function('console.debug(b);')(); // ReferenceError: b is not defined
}
test2();
```

내부동작과 유효범위(혹은 scope)의 차이가 있다.

- `eval()`: 자바스크립트 표현식으로 문자열을 자바스크립트 코드로 해석한 후 이를 평가한다. 유효범위는 `eval()`의 호출 시점이다. 따라서 실행 범위 내의 지역변수에 접근 할 수 있다.
- `new Function()`: 주어진 문자열을 자바스크립트 코드로 파싱하여 호출 가능한 객체에 저장한다. 별도의 함수 영역으로 분리되기 때문에 유효범위는 분리되며 지역변수에 접근 할 수 없다.
