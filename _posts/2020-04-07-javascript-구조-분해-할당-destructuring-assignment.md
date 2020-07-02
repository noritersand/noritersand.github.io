---
layout: post
date: 2020-04-07 13:49:00 +0900
title: '[JavaScript] 구조 분해 할당 Destructuring assignment'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - es2015
  - destructuring-assignment
  - destructuring-expression
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [MDN: Destructuring assignment](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

#### 브라우저 호환

- IE에서 사용 불가
- Safari에서 _Rest in arrays_ 사용 불가

구조 분해 할당은 배열이나 객체의 속성을 해체하여 개별 변수에 할당하는 표현식을 말한다. 'Destructuring expression'이라고도 함.

## 배열 구조 분해

```js
var [min = 0, max = Infinity, ...rest] = [1, 2, 3, 4, 5];
min; // 1
max; // 2
rest; // [3, 4, 5]
```

위에서 `max = Infinity`는 우측변 배열의 두 번째 요소를 변수 `max`에 할당하라는 의미이며 두 번째 요소가 없거나 `undefined`면 `Infinity`를 할당하라는 뜻이다.

점 세 개로 배열의 나머지를 모두 할당하는 변수를 지정할 수 있다. `...rest`는 rest에 해체하지 않은 배열의 나머지 요소를 모두 할당하며 이를 _Rest in arrays_ 라고 한다.

## 객체 구조 분해

객체의 구조 분해 할당식은 배열과 조금 다르다. 요소의 순서에 의존하지 않고 이름이 중요한데:

```js
var { x = 24, b = 48, ...y } = { a: 1, b: 2, c: 3, d: 4 };
x; // 24
b; // 2
y; // { a: 1, c: 3, d: 4 }
```

여기서 우측변 객체에 'x'라는 이름의 프로퍼티가 없으므로 24를 할당한 변수 `x`를 만든다. `b`는 같은 이름의 프로퍼티가 있으므로 2를 할당한다.

`y`는 앞서 분해한 나머지를 모두 할당한다. 이건 _Rest in objects_ 라고 한다.

## 함수 매개변수의 기본값 설정

구조 분해 할당은 함수 매개변수의 기본값 설정에 사용할 수도 있는데 익혀두면 매우 유용한 트릭이다.

예를 들어 다음과 같은 함수가 있을 때:

```js
function doSomething({a = 1, b = 2, c = 3}) {
  // ...
}
```

아래처럼 호출하면:

```js
doSomething({
  b: 65536
});
```

매개변수로 사용된 객체에 실제로 존재하는 프로퍼티는 `b` 뿐이므로 `a`와 `c`는 기본값이 할당된다:

```js
function doSomething({a = 1, b = 2, c = 3}) {
  console.log(a); // 기본값 1 출력
  console.log(b); // 전달된 65536 출력
  console.log(c); // 기본값 3 출력
}
```

## 꼐속...
