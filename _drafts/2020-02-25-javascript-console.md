---
layout: post
date: 2020-02-25 16:15:00 +0900
title: '[JavaScript] console'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - console
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [MDN: console](https://developer.mozilla.org/en-US/docs/Web/API/Console)

## log, debug, info, warn, error

일반적인 출력.

## trace

코드 실행 위치의 호출 스택 출력.

## console 출력이 스타일 적용하기

`%c`부터 `%c`까지 스타일 적용. 두 번째 `%c`를 생략하면 끝까지 적용한다.

```js
console.log("This is %cMy stylish%c message", "color: yellow; font-style: italic; background-color: blue;padding: 2px");
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

## time, timeLog, timeEnd

## assert
