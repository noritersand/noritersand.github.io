---
layout: post
date: 2018-03-20 15:10:49 +0900
title: '[JavaScript] 제어문 control statement'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - control-statement
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration

## 조건문

### if-else

표현식이 `true`일 때(혹은 `true`로 변환되는 값일 때) 구문을 실행한다.

```
if (표현식) { 구문 }
```

```js
var a = 1;

if (a) {
  if (a == 1) {
    console.log(a);
  }
}
else if (a === null) {
  console.log('a is null')
}
else {
  console.log('a is undefined');
}
```

### switch

표현식의 값과 `case` 레이블의 값이 일치하면 `case` 다음에 오는 구문을 실행한다. 만약 일치하는 `case` 레이블이 없으면 `default` 레이블의 구문을 실행한다.

```
switch (표현식) { 구문 }
```

```js
var a = 1;

switch (a) {
  case 1:
    console.debug('first');
    break;
  case 2:
    console.debug('second');
    break;
  default:
    console.debug('eliminated');
    break;
}
```

`break`나 `return`으로 끝나지 않은 `case` 레이블의 코드 블록은 바로 다음에 이어지는 `case` 레이블의 구문을 실행시킨다:

```js
var a = 3;

switch (a) {
  case 1:
    console.log('일');
  case 2:
    console.log('이');
  case 3:
    console.log('삼'); // 실행됨
  case 4:
    console.log('사'); // case 3에서 break로 끝나지 않았으므로 실행됨
  default:
    console.log('디폴트'); // case 4에서 break로 끝나지 않았으므로 실행됨
}
```

## 루프문

### while

표현식을 평가해 `true`일 때 구문을 실행한다. 구문 실행이 끝나면 다시 한번 표현식을 평가하며 표현식이 `false`로 평가될 때까지 이 과정을 반복한다.

```
while (표현식) { 구문 }
```

```js
var i = 0;

while (++i < 10) {
  console.log(i); // 1부터 9까지 출력
}
```

표현식이 `true`면 무한 루프가 된다:

```js
while (true) {
  // 무한 루프
}
```

### do-while

우선 `do`의 구문을 선실행한 후 표현식을 평가한다. 구문을 선실행하는 것만 빼면 `while`과 같다.

```
do { 구문 } while (표현식);
```

```js
var i = 0;

do {
  console.log(i); // 0부터 9까지 출력
} while (++i < 10);
```

### for

루프가 시작되기 전 초기화를 한 번 실행한다. 그 후 표현식을 평가하여 `true`일 때 구문을 실행한다. 코드 블록의 끝을 만나면 증감식을 실행한 후 다시 표현식을 평가한다. 이 과정은 표현식이 `false`가 될 때까지 반복된다.

```
for (초기화; 테스트 표현식; 증감식) { 구문 }
```

일반적인 사용은 다음과 같다:

```js
for (var i = 0 ; i < 10; i++) {
  console.log(i); // 0부터 9까지 출력
}
```

만약 초기화가 필요없다면 생략할 수 있다. 증감식도 마찬가지:

```js
var i = 0;
for (; i < 10;) { // 초기화와 증감식 생략
  console.log(i); // 0부터 9까지 출력
  i++;
}
```

테스트 표현식까지 생략할 수 있는데, 이렇게 되면 무한 루프다:

```js
for (;;) {
  // 무한 루프
}
```

쉼표 연산자를 사용하면 여러 변수의 초기화나 증감을 `for`에 적용할 수 있다:

```js
for (var i = 0, j = 10; i < 10; i++, j--) {
  console.log('i: ', i); // 0부터 9까지 출력
  console.log('j: ', j); // 10부터 1까지 출력
}
```

다음 코드를 보면 `for`에서 초기화되는 변수도 끌어올림<sup>hoisting</sup>이 적용되는것을 확인할 수 있다.

```js
var i = 'global';
(function() {
  console.log(i); // 끌어올림이 적용되어 'global'이 아니라 undefined 출력됨
  for (var i = 0 ; i < 10; i++) {
    console.log(i); // 0부터 9까지 출력
  }
})();
```

### for-in

> The for...in statement iterates over all enumerable properties of an object that are keyed by strings (ignoring ones keyed by Symbols), including inherited enumerable properties.
>
> [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)

객체가 소유한 프로퍼티의 길이만큼 구문을 반복하되 각 루프마다 객체가 갖고 있는 프로퍼티의 이름을 변수에 할당한다. 객체의 모든 프로퍼티만큼 반복하는 것은 아니고 열거 할 수 있는 프로퍼티<sup>enumerable properties</sup>의 길이만큼만 반복한다. 열거 할 수 있는 프로퍼티란, [객체 자신의 프로퍼티에 대한 '프로퍼티 설명자'](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)에서 `enumerable` 프로퍼티가 `true`인 프로퍼티, 예를 들어 `toString()`은 `enumerable`이 `false`라서 `for-in`으로 열거 할 수 없다. (자바스크립트 구현체마다 열거 가능한 프로퍼티가 다를 수 있음)

```
for (변수 in 객체) { 구문 }
```

```js
for (var prop in window) {
  console.log(window[prop]);
}
```

자바스크립트는 루프를 실행하기 전에 먼저 객체를 평가한다. 만약 객체 타입이 아닌 원시 타입이라면 래퍼 객체로 변환될 것이다:

```js
var str = 'abc';
for (var prop in str) {
  console.log(str[prop]); // a b c 출력
}
```

배열은 객체로 취급되며 배열을 구성하는 각 요소가 배열의 프로퍼티다. 그리고 배열의 인덱스가 프로퍼티 이름이 된다:

```js
var arr = ['a', 'b', 'c'];
for (var prop in arr) {
  console.log(prop); // 0 1 2 출력
}
```

### for-of

`for-of`는 `for-in`과 비슷하지만 프로퍼티의 이름이 아니라 프로퍼티의 값을 변수에 할당한다. **단, iterable object만 `for-of`로 반복할 수 있는데**, iterable object에 해당되는 타입은 `arguments object`와 `Array`, `String`, `TypedArray`, `Map`, `NodeList` 등이 있다.  
[ES2015에서 새로 추가되었고](http://www.ecma-international.org/ecma-262/6.0/#sec-for-in-and-for-of-statements), IE에서는 사용할 수 없다.

```js
var arr = ['a', 'b', 'c', 'd'];
for (var ele of arr) {
  console.debug(ele); // 'a', 'b', 'c', 'd' 각각 출력
}

var obj = {
  a: 1, b: 2
}
for (var ele of obj) {} // TypeError: obj is not iterable
```

## 점프문

### 레이블

특정 구문에 식별자 이름과 콜론으로 레이블을 붙인다. 레이블이 있는 구문은 `continue`와 `break` 키워드와 조합하여 점프문으로 사용된다. 보통은 중첩된 루프나 `switch`에서 현재 구문이 가장 안쪽에 있지 않을 때 사용한다.

```
식별자:
구문
```

```js
var i, j;

loop1:
for (i = 0; i < 3; i++) {      //The first for statement is labeled "loop1"
   loop2:
   for (j = 0; j < 3; j++) {   //The second for statement is labeled "loop2"
      if (i === 1 && j === 1) {
         break loop1;
      }
      console.log("i = " + i + ", j = " + j);
   }
}
```

### break

`break`를 감싸고 있는 가장 가까운 반복문이나 `switch`를 탈출한다. 레이블이 명시되면 현재 어느 위치에 있는지와 상관없이 해당 제어문을 바로 탈출한다.

```
break [label];
```

```js
var i = 0;
while (true) {
  i++;
  if (i > 10) {
    break;
  }
}
```

### continue

반복문 내에서 사용하며 `break`와 달리 반복문을 탈출하지 않고 다음 반복을 진행한다. '이번 반복은 생략'과 같은 의미다. `for`를 예로 들면 `continue`를 만났을 때 증감식을 실행하는 부분부터 다시 시작한다. `continue`는 반드시 루프문 내에서 사용해야 한다. 그렇지 않으면 `SyntaxError`가 발생할 것이다. 레이블이 명시되면 해당 레이블부터 다음 반복을 진행한다.

```
continue [label];
```

```js
var arr = [1, 2, 3];
for (var idx in arr) {
  if (arr[idx] == 2) {
    continue;
  }
  console.debug(arr[idx]); // 2는 건너뛰므로 1 3 출력
}
return
```

함수 내에서만 사용할 수 있는 키워드다. 평가된 표현식의 결과를 함수의 값으로 반환한다. 표현식은 생략될 수 있으며 이 경우 반환되는 값은 `undefined`. 반환된 값은 함수호출 표현식의 평가 결과가 된다.

```
return 표현식;
```

```js
function add(a, b) {
  return a + b;
}
```

### throw

명시적인 예외를 발생시킨다. 발생지점을 감싼 try-catch가 없다면 에러로 취급되며 에러 메시지를 사용자에게 보고한다.

```js
function add(a, b) {
  if (isNaN(a) || isNaN(b)) {
    throw new Error('숫자가 아니네');
  }
  return a + b;
}
add('a', 'b'); // Error: 숫자가 아니네
```

### try-catch-finally

try는 '예외가 발생할지도 모르는' 코드 블록을 정의한다. 이어지는 `catch`의 구문은 `try` 블록에서 예외가 발생했을 때 실행된다. `finally`는 `try` 블록의 코드가 일부라도 실행되고 나면 예외 발생 여부와 관계없이 반드시 한 번은 실행되는 코드 블록이다. `finally`는 생략 가능하다.

```
try { 구문 } catch (error) { 구문 } finally { 구문 }
```

- `error`: `Error.prototype`의 인스턴스. `name`(에러 타입), `message`(에러 메시지), `lineNumber`(예외 발생 지점의 라인 번호), `fileName`(파일명)을 프로퍼티로 갖는다.

```js
var obj = {};
try {
  obj.a();
} catch (e) {
  console.log(e.name + ': ' + e.message); // TypeError: obj.a is not a function
} finally {
  console.log('어찌되던 실행');
}
```

finally의 반드시 한 번은 실행된다는 점은 `try` 코드 블록에서 `return`, `continue`, `break`를 만났을 때도 유효하다.

```js
var i = 0;
while(++i < 4) {
  try {
    continue;
  } catch (e) {
  } finally {
    console.log(i); // 1 2 3 출력
  }
}
```

주의해야 할 점은 `try`의 점프문에 의해 제어가 `finally`로 넘어갔을 땐 `finally` 코드 블록의 점프문이 `try`보다 우선권을 가진다는 것이다. 다음을 보면 `try`에서 'abc'를 반환하고 `finally`에서 `undefined`를 반환하도록 되어 있는데:

```js
function fn() {
  try {
    return 'abc';
  } catch (e) {
    // ...
  } finally {
    return;
  }
}
fn(); // undefined
```

실제 반환되는 값은 `finally`의 `undefined`다.
