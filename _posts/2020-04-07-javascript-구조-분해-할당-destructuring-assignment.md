---
layout: post
date: 2020-04-07 13:49:00 +0900
title: '[JavaScript] 구조 분해 할당 Destructuring assignment'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
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

`max = Infinity`는 우변 배열의 두 번째 요소를 변수 `max`에 할당하라는 의미이며 두 번째 요소가 없거나 `undefined`면 `Infinity`를 할당하라는 뜻이다.

점 세 개로 배열의 나머지를 모두 할당하는 변수를 지정할 수 있다. `...rest`는 변수 `rest`에 해체하지 않은 배열의 나머지 요소를 모두 할당한다는 의미이며, 이를 _Rest in arrays_ 라고 한다. `...`는 전개 연산자인데, 해당 내용은 전개 구문을 살펴볼 것.

### 선언 이후의 할당

구조 분해 할당은 선언 이후에 해도 문제 음슴:

```js
var min, max;
[min, max] = [0, 999];
min; // 0
max; // 999
```

### 변수 값 교환 트릭

원래 임시 변수가 필요한 코드지만 구조 분해를 활용하면:

```js
var a = 1;
var b = 2;
[a, b] = [b, a];
a; // 2
b; // 1
```

이렇게 쓸 수 있다.

### 반환 값 무시

필요 없는 요소는 다음처럼 무시할 수도 있다:

```js
var [a, , b] = [1, 2, 3];
a; // 1
b; // 3
```

## 객체 구조 분해

객체의 구조 분해 할당식은 배열과 조금 다르다. 요소의 순서에 의존하지 않고 이름이 중요한데:

```js
var { x = 24, b = 48, ...y } = { a: 1, b: 2, c: 3, d: 4 };
x; // 24
b; // 2
y; // { a: 1, c: 3, d: 4 }
```

우변에서 'x'와 'b'라는 프로퍼티를 찾아 변수를 만든다. 좌변에서 `= 24`와 `= 48`은 이름이 같은 프로퍼티가 없을 때 할당할 기본값을 의미한다. 이 코드에선 우변에 'x' 프로퍼티가 없으므로 변수 `x`는 24로 초기화한다. 변수 `b`는 우변에 있으므로 2를 할당한다.

`...y`는 앞서 분해한 나머지를 모두 `y`에 할당하는 코드다. 이건 _Rest in objects_ 라고 한다. 나머지를 할당하는 변수의 이름은 중요하지 않고 `...`로 시작하면 된다.

### 선언 이후의 할당

배열의 경우와 마찬가지로 객체 구조 분해 할당도 선언 이후에 할 수 있다:

```js
var a, b;
({ a, b } = { a: 1, b: 2 });
a; // 1
b; // 2
```

단, 그냥 쓰면 `SyntaxError`가 발생하므로, 괄호`()`로 감싸야 한다. [MDN의 설명](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment#선언_없는_할당)에 따르면 좌변의 `{ a, b }`가 객체 리터럴이 아닌 블록으로 간주되지 때문이라 한다.

### 새로운 변수 이름으로 할당

콜론`:`으로 원래의 프로퍼티명이 아닌 다른 이름으로 변수를 정의할 수 있다:

```js
let { p: foo, q: bar } = { p: 42, q: true };
foo; // 42
bar; // true
typeof p; // "undefined"
typeof q; // "undefined"
```

보면 알겠지만 변수 `p`와 `q`는 만들어지지 않았음.

### 기본값과 새 변수 할당 동시 설정

```js
var {a: aa = 10, b: bb = 5} = {a: 3}; // 순서에 주의
aa; // 3
bb; // 5
```

이쯤되면 이게 자바스크립트 맞나 싶다. 🙄

### 변수 정의 활용

```js
var obj = { a: 4 };
var { a } = obj;
a; // 4
```

위 코드는 객체 `{ a: 4 }`에서 `a`의 값을 가져오며 동시에 `a`라는 변수를 정의하는 코드다.

응용하면 다음과 같은 코드도 성립한다:

```js
// const log = console.log; // 아래와 같음
const { log } = console;
log('hi');
```

### 함수 매개변수의 기본값 설정

객체 구조 분해는 함수 매개변수의 일종의 기본값처럼 활용할 수 있다. 예를 들어 함수 정의가 아래와 같을 때:

```js
function fn({ a = 1, b = 2, c = 3 }) {
  // ...
}
```

아래처럼 호출하면:

```js
fn({ b: 65536 });
```

매개변수로 넘겨진 객체에 실제로 존재하는 프로퍼티는 `b` 뿐이므로 `a`와 `c`는 기본값이 할당된다:

```js
function fn({a = 1, b = 2, c = 3}) {
  console.log(a); // 기본값 1 출력
  console.log(b); // 전달된 65536 출력
  console.log(c); // 기본값 3 출력
}
```

### 유효하지 않은 식별자명인 경우

```js
var obj = { 'r2-d2': 1 };
var { 'r2-d2' } = obj; // Uncaught SyntaxError: missing : after property id
```

식별자명이 유효하지 않으면 `SyntaxError`가 발생하므로 반드시 대체할 식별자를 지정해줘야 한다:

```js
var { 'r2-d2': R2D2 } = obj;
R2D2; // 1
```

### 계산된 프로퍼티명과 같이 쓰기

좌변이 객체 리터럴이므로 계산된 프로퍼티명을 적용할 수 있음:

```js
let { [ 'a' + 'b' + 'c' ]: z } = { abc: 123 }; // 'abc'를 찾아 새 변수 z에 할당
z; // 123
```

이 때도 그냥 쓰면 `SyntaxError`가 발생하므로 대체할 식별자를 지정할 것.

## 나머지 구문의 위치 제한

나머지 구문은 구조 분해 할당식의 좌변 마지막에만 올 수 있다:

```js
var [min, ...rest, max] = [1, 2, 3, 4, 5]; // SyntaxError: Rest element must be last element
var { x, ...y, b } = { a: 1, b: 2, c: 3, d: 4 }; // SyntaxError: Rest element must be last element
```

## 중첩된 객체/배열의 구조 분해

여기까진 깊이가 1인 객체의 예시만 들었는데, 만약 2레벨 이상의 객체를 분해하려면 어떻게 해야 할까?

답은:

```js
const data = {
  character: {
    name: 'waldo',
    script: {
      hello: 'Hello there! Mighty fine morning',
      introduceSelf: 'If you ask me! I\'m Waldo'
    }
  }
};

var {
  character: {
    name: realName // rename: 'name' => 'realName'
  },
  character: {
    script: {
      hello: hi // rename: 'hello' => 'hi'
    }
  }
} = data;

realName; // "waldo"
hi; // "Hello there! Mighty fine morning"
```

만약 배열이 섞인다면:

```js
const data = {
  character: {
    name: 'waldo',
    scripts: [
      'Hello there! Mighty fine morning',
      'If you ask me! I\'m Waldo'
    ]
  }
};

var { character: { scripts: [ firstScript ] } } = data;
firstScript; // "Hello there! Mighty fine morning"
```

배열 리터럴을 적절히 섞어주면 됨.

## for-of에서 구조 분해

놀랍게도 `for-of`에서 구조 분해를 사용할 수 있다. 예시는 MDN의 고것을 그대로 가져왔다:

```js
var people = [
  {
    name: "Mike Smith",
    family: {
      mother: "Jane Smith",
      father: "Harry Smith",
      sister: "Samantha Smith"
    },
    age: 35
  },
  {
    name: "Tom Jones",
    family: {
      mother: "Norah Jones",
      father: "Richard Jones",
      brother: "Howard Jones"
    },
    age: 25
  }
];

for (var {name: n, family: { father: f } } of people) {
  console.log("Name: " + n + ", Father: " + f);
}

// "Name: Mike Smith, Father: Harry Smith"
// "Name: Tom Jones, Father: Richard Jones"
```

끟.
