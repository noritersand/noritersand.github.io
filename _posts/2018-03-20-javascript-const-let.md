---
layout: post
date: 2018-03-20 16:40:10 +0900
title: '[JavaScript] const, let'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - const
  - let
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [MDN: const](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/const)
- [MDN: let](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let)

#### 버전 정보

- ES2015에서 최초 정의
- const
  - Chrome 21/Edge 12/Firefox 36/Opera 9/Safari 5.1 이상에서 사용 가능
  - IE는 10 이하에서 사용 불가
- let
  - Chrome 49/Edge 14/Firefox 44/Opera 17/Safari 10 이상에서 사용 가능
  - IE는 10 이하에서 사용 불가. [11에서 부분적 지원](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/let#Browser_compatibility)

## const

`const`는 자바스크립트 1.5 이후 버전에서 사용가능한 키워드로, 상수를 정의할 때 사용하며 `var` 키워드를 대체할 수 있다.

```
const name1 = value1 [, name2 = value2 [, ... [, nameN = valueN]]];
```

```js
const PI = 3.14;
```

`const`는 전형적인 '상수'답게 선언 시점에 반드시 초기화되어야 하며 선언 이후의 할당은 무시되거나 에러를 발생시킨다.

```js
const ABC; // SyntaxError: missing = in const declaration, 초기화 필요함

const PI = 3.14; // 정상적인 선언
const PI = 3.14159265; // SyntaxError: redeclaration of const PI, 중복 선언 불가
PI = 3.141592; // TypeError: invalid assignment to const `PI', 재할당 불가
var PI = 3; // SyntaxError: redeclaration of const PI, 같은 이름의 변수 선언 불가
```

`const`는 객체 선언에도 사용할 수 있으나 객체의 프로퍼티는 보호되지 않는다:

```js
const MY_OBJECT = {"key": "value"};
MY_OBJECT = {"OTHER_KEY": "value"}; // SyntaxError: redeclaration of const MY_OBJECT
MY_OBJECT.key = "otherValue";
console.log(MY_OBJECT); // Object { key: "otherValue" }
```

객체를 완전히 보호하고 싶다면 [`Object.freeze()`](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)를 사용할 것.

## let

`let`은 자바스크립트 1.7(ES2015)부터 신규 추가된 키워드다. `const`와 마찬가지로 `var`를 대체할 수 있다.

```
let var1 [= value1] [, var2 [= value2]] [, ..., varN [= valueN]];
```

`let`은 자바스크립트의 변수가 갖는 함수 유효범위를 블록 유효범위로 제한하기 위해 사용한다.
가령 다음의 경우:

```js
function fn() {
  let a = 1;
  {
    let b = 2;
    var w = 102;
  }
  console.log(a); // 1
  console.log(w); // 102
  console.log(b); // ReferenceError: b is not defined
}
fn();
```

유효범위가 함수 단위인 `var`로 선언한 변수 `w`는 블록 밖에서도 참조 가능한 반면, `let`으로 선언한 변수 `b`는 해당 지역을 감싼 블록과 그 하위의 중첩 블록에서만 유효한 블록 단위의 유효범위를 갖기 때문에 블록을 벗어난 지역에서는 참조 에러가 발생한다.

```js
var arr = ['a', 'b', 'c'];

for (var ele1 in arr) {}
console.log(arr[ele1]); // 'c'

for (let ele2 in arr) {}
console.log(arr[ele2]); // ReferenceError: ele2 is not defined
```

`let`으로 선언된 `ele2`는 `for`문 밖에서 참조할 수 없다.
