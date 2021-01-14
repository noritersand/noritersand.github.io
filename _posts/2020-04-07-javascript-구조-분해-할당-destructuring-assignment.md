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

#### 버전 정보

- ES2015에서 최초 정의
- Chrome 60/Edge 79/Firefox 55/Opera 47/Safari 11.1 이상에서 사용 가능
- IE에서 사용 불가

구조 분해 할당은 배열이나 객체의 프로퍼티를 해체하여 개별 변수에 할당하는 표현식을 말한다. 'Destructuring expression'이라고도 함.

## 배열 구조 분해

```js
var [min = 0, max = Infinity, ...rest] = [1, 2, 3, 4, 5];
min; // 1
max; // 2
rest; // [3, 4, 5]
```

위에서 `max = Infinity`는 우측변 배열의 두 번째 요소를 변수 `max`에 할당하라는 의미이며 두 번째 요소가 없거나 `undefined`면 `Infinity`를 할당하라는 뜻이다.

점 세 개로 배열의 나머지를 모두 할당하는 변수를 지정할 수 있다. `...rest`는 rest에 해체하지 않은 배열의 나머지 요소를 모두 할당하며 이를 _Rest in arrays_ 라고 한다. `...`는 전개 연산자인데, 해당 내용은 전개 구문을 살펴볼 것.

## 객체 구조 분해

객체의 구조 분해 할당식은 배열과 조금 다르다. 요소의 순서에 의존하지 않고 이름이 중요한데:

```js
var { x = 24, b = 48, ...y } = { a: 1, b: 2, c: 3, d: 4 };
x; // 24
b; // 2
y; // { a: 1, c: 3, d: 4 }
```

여기서는 우측변 객체에 'x'라는 이름의 프로퍼티가 없으므로 24를 할당한 변수 `x`를 만든다. `b`는 같은 이름의 프로퍼티가 있으므로 2를 할당한다.

`y`는 앞서 분해한 나머지를 모두 할당한다. 이건 _Rest in objects_ 라고 한다. 여기까지 보면 알겠지만 나머지를 할당하는 변수는 이름과 상관없이 `...`로 시작하면 된다.

### 변수 정의 활용

객체 구조 분해의 특성을 이용하면 이런 코드도 성립한다:

```js
var obj = { a: 4 };
var { a } = obj;
a; // 4
```

`{ a: 4 }` 객체에서 `a`의 값을 가져오면서 동시에 `a`라는 변수를 정의하는 코드다.

응용하면 이런식으로도 된다:

```js
// const log = console.log; // 아래와 같음
const { log } = console;
log('hi');
```

### 함수 매개변수의 기본값 설정

객체 구조 분해는 함수 매개변수의 일종의 기본값처럼 활용할 수 있다. 익혀두면 매우 유용한 트릭이다.

예를 들어  함수 정의가 아래와 같을 때:

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

매개변수로 넘겨진 객체에 실제로 존재하는 프로퍼티는 `b` 뿐이므로 `a`와 `c`는 기본값이 할당된다:

```js
function doSomething({a = 1, b = 2, c = 3}) {
  console.log(a); // 기본값 1 출력
  console.log(b); // 전달된 65536 출력
  console.log(c); // 기본값 3 출력
}
```

## 꼐속...
