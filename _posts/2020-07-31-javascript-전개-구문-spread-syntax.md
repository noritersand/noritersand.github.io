---
layout: post
date: 2020-07-31 20:48:00 +0900
title: '[JavaScript] 전개 구문 Spread syntax (...)'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - spread-syntax
  - rest-syntax
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Spread syntax (...)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
- [\[MDN\] Spread properties](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#spread_properties)

#### 버전 정보

- ES2015에서 최초 정의
- ES2018에서 객체 초기화 관련 정의
- Chrome 60/Edge 79/Firefox 55/Opera 47/Safari 11.1 이상에서 사용 가능
- IE에서 사용 불가

## 개요

전개 구문은 객체나 배열의 요소를 말그대로 전개하거나 분해할 때 사용한다. 단순 전개부터 `apply()`를 대체하거나 인스턴스 생성, 배열 복제/연결, 객체 복제 등에 사용할 수 있다.

```js
var obj = { a: 1, b: 2 };
var obj2 = { ...obj };

var n = [1, 2, 3];
console.log(n); // Array(3) [ 1, 2, 3 ]
console.log(...n); // 1 2 3
```

전개 구문에서 점 세개`...`는 **전개 연산자(spread operator)**라고 부른다.

## 함수 호출에서: 인수 목록을 위한 전개 Spread for argument lists

### 객체는 안되영

인수 목록에서 plain object는 전개할 수 없다:

```js
var obj = { a: 1, b: 2 };
log(...obj); // Uncaught TypeError: obj is not iterable
```

인수 목록에서 전개하려면 `Array`나 `Map`같은 *Iterable object*여야 함.

### apply() 대체

원래 배열의 요소를 함수의 인자로 활용하려면 `Function.prototype.apply()`를 써야 했다:

```js
function fn(a, b, c) {
  return '' + a + b + c;
}
```

```js
fn(3, 2, 1); // 321
fn.apply(null, [3, 2, 1]); // 321

var arr = [10, 30, 5];
Math.min.apply(Math, arr); // 5
```

이제는 전개 구문으로:

```js
fn(...[3, 2, 1]); // 321

var arr = [10, 30, 5];
Math.min(...arr); // 5
```

이렇게 간단하게 작성한다.

그리고 전개 구문은 인수 목록에서 여러번 허용된다:

```js
var arr = [7, 8];
fn(...[6], ...arr) // 678
```

요렇게도 쓸 수 있다는 말.

### new에 적용

배열의 요소를 전개한다는 점을 이용해서 생성자에게 전달할 인수를 작성할 때 사용함:

```js
function Fn(a, b, c) {
  this.a = '' + a + b + c;
}

var o = new Fn(1, 2, 3);
log(o); // Fn { a: "123" }

var o2 = new Fn(...[1, 2, 3]);
log(o2); // Fn { a: "123" }
```

## 배열 리터럴에서의 전개

### 배열 연결

둘 이상의 배열을 연결할 땐 `Array.prototype.concat()`을 사용하곤 했는데:

```js
var arr1 = [1, 2, 3];
var arr2 = [4, 5, 6];

var arr3 = arr1.concat(arr2); // [1, 2, 3, 4, 5, 6]
```

이제는 그냥 이렇게 하면 된다:

```js
var arr4 = [...arr1, ...arr2];
```

응용하면 좀 더 유연한 방식의 리터럴 작성이 가능함:

```js
var def = ['d', 'e', 'f'];
var alphabet = ['a', 'b', 'c', ...def, 'g'];
alphabet; // ["a", "b", "c", "d", "e", "f", "g"]
```

### 배열 복제

원본을 복제한 새 배열을 만드는 방법이다:

```js
var arr = [1, 2, 3];
var arr2 = [...arr];
arr === arr2; // false
arr2; // [1, 2, 3]
```

배열을 한 번 전개해도 내용물이 여전히 배열인 경우(e.g. 2단 이상의 배열) 기존 배열을 참조하는게 되니 주의할 것:

```js
var arr = [[1, 2], [3]];
var arr2 = [...arr];

arr[0]; // [1, 2]
arr2[0].pop(); // 참조하는 배열을 수정해서 arr도 영향을 받음.
arr[0]; // [1]
```

## 객체 리터럴에서의 전개

### 객체 복제

객체의 전개는 `Object.assign()`을 대체하여 객체 복제에 사용할 수 있다:

```js
var obj = { foo: 'bar', x: 42 };

var clone1 = { ...obj };
clone1; // Object { foo: "bar", x: 42 }
clone1 === obj; // false

var clone2 = { ...obj, x: '사십이', c: 1 }; // obj에서 x는 재할당하고 c를 추가한 새 배열 객체 반환
clone2; // Object { foo: "bar", x: "사십이", c: 1 }
```

배열과 마찬가지로 얕은 복제(shallow cloning)인 것에 주의:

```js
var obj = { foo: { a: 1, b: 2 } };

var clone4 = { ...obj };

obj.foo.a; // 1
clone4.foo.a = 3;
obj.foo.a; // 3
```

### 객체 연결

객체의 프로퍼티를 이어 붙여서 새 객체를 만드는 방법. 만약 이름이 같은 프로퍼티가 있다면, 나중에 오는 객체의 프로퍼티가 덮어쓴다:

```js
var obj1 = { foo: 'bar', x: 42 };
var obj2 = { foo: 'baz', y: 13 };

var mergedObj = { ...obj1, ...obj2 }; // obj2.foo가 obj1.foo를 덮어쓰게 됨
mergedObj; // Object { foo: "baz", x: 42, y: 13 }
```

## 나머지 구문

구조 분해 할당 표현식에서 사용할 땐 전개 구문 대신 **나머지 구문**이라 부르며 모양만 같지 완전히 다른 구문이다:

```js
var [min = 0, max = Infinity, ...rest] = [1, 2, 3, 4, 5];
min; // 1
max; // 2
rest; // [3, 4, 5]

var { x = 24, b = 48, ...y } = { a: 1, b: 2, c: 3, d: 4 };
x; // 24
b; // 2
y; // { a: 1, c: 3, d: 4 }
```

위 코드에서 `...rest`, `...y`가 나머지 구문임. 해당 내용은 구조 분해 할당을 살펴볼 것.

끗.
