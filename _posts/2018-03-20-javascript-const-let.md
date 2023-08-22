---
layout: post
date: 2018-03-20 16:40:10 +0900
title: '[JavaScript] const, let'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - const
  - let
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [\[MDN\] const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
- [\[MDN\] let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)

#### 버전 정보

- ES2015에서 최초 정의
- const
  - Chrome 21/Edge 12/Firefox 36/Opera 9/Safari 5.1 이상에서 사용 가능
  - IE는 10 이하에서 사용 불가
- let
  - Chrome 49/Edge 14/Firefox 44/Opera 17/Safari 10 이상에서 사용 가능
  - IE는 10 이하에서 사용 불가. [11에서 부분적 지원](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#browser_compatibility)


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

객체를 완전히 보호하고 싶다면 [`Object.freeze()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)를 사용할 것.


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

유효범위가 함수 단위인 `w`는 블록 밖에서도 참조 가능한 반면, `let`으로 선언한 변수 `b`는 해당 지역을 감싼 블록과 그 하위의 중첩 블록에서만 유효한 블록 단위의 유효범위를 갖기 때문에 블록을 벗어난 지역에서는 참조 에러가 발생한다.

```js
var arr = ['a', 'b', 'c'];

for (var ele1 in arr) {}
console.log(arr[ele1]); // 'c'

for (let ele2 in arr) {}
console.log(arr[ele2]); // ReferenceError: ele2 is not defined
```

`let`으로 선언된 `ele2`는 `for`문 밖에서 참조할 수 없다.


## let과 const는 끌어올림 대상에서 제외

`let`과 `const`는 공통적으로 끌어올림(hoisting, 호이스팅 혹은 선언 끌어올리기)의 적용을 받지 않는다:

```js
console.log(a); // undefined
var a;

console.log(b); // ReferenceError: can't access lexical declaration 'b' before initialization
let b;

console.log(c); // ReferenceError: can't access lexical declaration 'c' before initialization
const c = 3;
```

그래서 `var`와는 다르게, 선언 전에 참조할 경우 `ReferenceError`가 발생한다.


## 특이사항: for loop와 let

아래처럼 딱 한 바퀴만 도는 for loop가 있다고 하자:

```js
function getSomeWithVar() {
  var fn;
  for (var i = 0; i < 1; ++i) {
    fn = () => {
      console.log(i);
    }
  }
  return fn;
}
getSomeWithVar()(); // 1
```

반환된 `fn`에서 참조하는 `i`는 클로저 스코프에 존재한다. 그리고 `i`의 값은 마지막 루프의 증감식`++i`이 수행된 1이다.

그런데 만약 `i`가 `let`이라면?

```js
function getSomeWithLet() {
  var fn;
  for (let i = 0; i < 1; ++i) {
    fn = () => {
      console.log(i);
    }
  }
  return fn;
}
getSomeWithLet()(); // 0
```

1이 아니라 0이다.

![](/images/wuuuuut.png)

그래서 찾아봤는데: https://stackoverflow.com/questions/42556873/closure-let-keyword-javascript

> because you use let each anonymous function refers to a different instance of x. There is a different instance on each iteration of the loop. This happens because let has a block-level scope instead of the global function scope that var has.

(이 글이 사실이라는 전제 하에) 발번역하면 각 루프가 서로 다른 인스턴스이며, 익명함수가 그 서로 다른 인스턴스를 참조하기 때문이라고 한다. (혼절)

이 답변을 바탕으로 상상회로를 돌려보면 대충 이런 결론이 나온다:

0. 대전제: 유효범위 = 블록 = 스코프 = 인스턴스
1. 함수의 유효범위 깊이는 1이라 가정.
2. 자바스크립트의 반복문은 각 루프마다 별도의 인스턴스를 갖는다. (깊이 2)
3. 증감식은 각각의 반복이 끝날 때 수행된다.
4. 반복문의 선언식과 증감식은 깊이 2의 인스턴스에서 수행된다.
5. 각각의 반복은 한 단계 더 들어간 인스턴스에서 수행된다. 따라서 루프끼리는 서로 영향을 줄 수 없다. (깊이 3)
6. 선언식의 `let` 변수는 깊이 2의 원본을 복사해 깊이 3의 인스턴스로 전달한다. (`var`도 동일함)
7. 깊이 3의 인스턴스가 종료되면 익명함수가 참조하는 `i`는 클로저 스코프에만 존재한다.
8. 다음 루프가 시작되면서 깊이 2의 인스턴스의 `i`의 값이 증가하지만 깊이 3의 인스턴스에 있는 `i`는 증감식 수행 전의 값을 유지한다.

이게맞냐? 🤷‍♂️
