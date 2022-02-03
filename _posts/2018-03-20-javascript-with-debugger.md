---
layout: post
date: 2018-03-20 16:43:07 +0900
title: '[JavaScript] with, debugger'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - with
  - debugger
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/with](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/with)

## with

주로 깊이 중첩된 구조를 갖는 객체의 프로퍼티 접근 표현식을 생략하기 위해 사용된다. 다만 사용이 권장되지 않는다. David Flanagan은 최적화가 힘들고 코드 실행 속도가 느리기 때문이라 한다. MDN은 "혼란스러운 버그를 유발하고 호환성에 문제가 있어서 비권장"한다. 게다가 엄격 모드(strict mode)에선 사용조차 불가하니 실제 쓸 일은 없어 보인다.

```js
var obj = {};
obj.inner = {};
obj.inner.number = 1234;

with (obj.inner) {
  console.debug(number); // 1234
}
```

## debugger

디버거의 브레이크 포인트 역할을 하는 키워드. 실제로 사용해보면 debugger 키워드가 있는 지점에 브레이크가 걸린다. 단 실제 작동은 자바스크립트 구현체에 따라 달라질 수 있다. 인터넷 브라우저에선 크롬51, 파폭47, IE11, 엣지에서 정상 작동하는걸 확인함.

브라우저 개발자 도구를 열어놓고 아래 코드를 실행해보자:

```js
var str = 'debugger test';
function fn() {
  debugger;
  return str;
}
fn();
```
