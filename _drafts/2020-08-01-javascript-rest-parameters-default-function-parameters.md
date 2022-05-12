---
layout: post
date: 2020-08-01 21:00:00 +0900
title: '[JavaScript] Rest parameters, default function parameters'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - rest-parameters
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Rest parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)
- [\[MDN\] Default parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
- [\[MDN\] Spread syntax (...)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [\[MDN\] 전개 속성](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#spread_properties)

#### 버전 정보

- [ES2015에서 최초 정의](https://262.ecma-international.org/6.0/#sec-function-definitions)
- Chrome 49/Edge 79/Firefox 52/Opera 36/Safari 10 이상에서 사용 가능
- IE에서 사용 불가

## 개요

나머지 매개변수와 기본값 매개변수 간단 정리.

## 나머지 매개변수 Rest parameters

Rest parameters는 함수의 매개변수보다 전달인자의 수가 많을 때, 언급되지 않은 모든 전달인자를 하나의 매개변수에 모두 할당하는 구문이다.

```
function f(a, b, ...theArgs) {
  // ...
}
```

TODO

## 기본값 함수 매개변수 Default function parameters

어쩌구

```
function fnName(param1 = defaultValue1, ..., paramN = defaultValueN) { /* ... */ }
```

TODO
