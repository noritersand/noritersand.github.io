---
layout: post
date: 2020-02-25 16:15:00 +0900
title: '[JavaScript] console'
categories:
  - javascript
tags:
  - javascript
  - console-standard
  - console
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] console](https://developer.mozilla.org/en-US/docs/Web/API/Console)


## 개요

console의 메서드는 콘솔 창에 어떠한 값이나 문자를 출력할 때 사용한다. 실제 출력되는 모양은 브라우저마다 차이가 있다.


## log(), debug(), info(), warn(), error()

일반적인 출력.


## trace()

코드 실행 위치의 호출 스택 출력.


## dir()

객체나 배열을 좀 더 상세히 출력할 때 사용한다. 추어진 object를 계층적(hierarchical)이며 상호작용적(interactive)인 프로퍼티 목록으로 표시한다:

```js
var obj = [{a: 1}, {b: 2}];
console.dir(obj);
// Array [ {…}, {…} ]
// > 0: Object { a: 1 }
// > 1: Object { b: 2 }
```


## console 출력에 스타일 적용하기

`%c` 이후의 텍스트부터 스타일 적용.

```js
console.log("This is %cmy message", "color: black; font-weight: bold; background-color: #eee; padding: 2px;");
```


## table

```js
console.table({a: 1, b: 2, c: 3});
// 혹은
console.table(["apples", "oranges", "bananas"]);
```


## group, groupCollapsed, groupEnd

```js
var c = {
  header: 'color: black; background-color: #ddd; font-size: 15px; font-weight: bold; padding: 5px',
  label: 'color: black; background-color: #ddd; padding: 2px',
  warning: 'color: red; background-color: #ddd; padding: 2px'
};
console.log('%cCONSOLE OUTPUT TEST', c.header);
console.log('this is the level 0');
console.group('%clevel 1', c.label);
console.log('this is level 1');
console.group('%clevel 2', c.label);
console.log('this is level 2');
console.log('more of level 2');
console.groupCollapsed('%ccollapsed level 3: %cDO NOT TOUCH ME', c.label, c.warning);
console.warn('Don\'t say i didn\'t warn you.');
console.log('this is level 3');
console.groupEnd();
console.log('back to level 2');
console.groupEnd();
console.log('back to level 1');
console.groupEnd();
console.log('back to the level 0');
```


## count()

TODO


## time, timeLog, timeEnd

TODO


## assert

TODO
