---
layout: post
date: 2020-08-01 21:00:00 +0900
title: '[JavaScript] 나머지 매개변수 Rest parameters'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - es2015
  - rest-parameters
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer#전개_속성](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Object_initializer#전개_속성)

#### 버전 정보

- ES2015에서 최초 정의
- Chrome 49/Edge 79/Firefox 52/Opera 36/Safari 10 이상에서 사용 가능
- IE에서 사용 불가

나머지 매개변수는 함수의 파라미터보다 인수의 수가 많을 때, 언급되지 않은 모든 인수를 하나의 변수에 모두 할당하는 구문이다.

```
function f(a, b, ...theArgs) {
  // ...
}
```
