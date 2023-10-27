---
layout: post
date: 2018-03-20 15:10:49 +0900
title: '[JavaScript] 제어문 control statement'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - control-statement
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling)
- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Loops_and_iteration)


## 조건문

조건문은 조건식(condition)과 코드 블록으로 구성된다. 코드 블록은 '조건부 실행 코드 블록' 혹은 줄여서 '실행 블록'이라고도 한다. (instruction이라 표현하는 문서도 있다)

### if-else

```
if (조건식) { 
  코드 블록
}
```

조건식이 `true`일 때(혹은 `true`로 변환되는 값일 때) 코드 블록을 실행한다.

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

```
switch (조건식) { 
  case (레이블):
    코드 블록
  default:
    코드 블록
}
```

조건식의 값과 `case` 절의 값이 일치하면 `case` 다음에 오는 코드 블록을 실행한다. 만약 일치하는 `case` 절이 없으면 `default` 레이블의 코드 블록을 실행한다. `default`는 생략할 수 있으며, `break`를 생략하지 않는 한 어디에 있어도 상관 없다.

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

`break`나 `return`으로 끝나지 않은 `case` 절의 코드 블록은 바로 다음에 이어지는 `case` 절의 실행을 유발한다:

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
};

// case 3과 case 4에 break가 없어 다음처럼 출력된다
// '삼'
// '사'
// '디폴트'
```

`case`와 `default` 절은 어휘 범위(Lexical scope)를 생성하지 않는다. 모두 같은 유효 범위 내에 있다는 뜻이다. 그래서 다음 코드는:

```js
var flag = true;
var result;
switch (flag) {
  case true:
    let foo = 'true'; // Uncaught SyntaxError: redeclaration of let foo
    console.log(foo);
    break;
  case false: 
    let foo = 'false'; // Uncaught SyntaxError: redeclaration of let foo
    console.log(foo);
    break;
}
````

같은 스코프에서 `foo`를 두 번 선언하는 꼴이 되어 `SyntaxError`를 발생시킨다. 만약 `case` 절마다 각각의 스코프를 생성하고 싶을 땐 중괄호`{}`를 사용한다:

```js
var flag = true;
var result;
switch (flag) {
  case true: {
    let foo = 'true';
    console.log(foo);
    break;
  }
  case false: {
    let foo = 'false';
    console.log(foo);
    break;
  }
} 
// "true"
```


## 반복문

반복문은 루프 헤더와 루프 바디로 구성되며, 루프 바디는 '코드 블록' 혹은 '반복 실행 블록'이라고도 한다.

### for

```
for (루프 헤더) {
  루프 바디
}

for (초기화; 조건식; 증감식) {
  코드 블록
}
```

루프가 시작되기 전 조건식을 먼저 평가한다. 조건식이 `true`면 코드 블록을 실행한다. 코드 블록의 끝을 만나면 증감식을 실행한 후 다시 조건식을 평가한다. 조건식이 `true`면 코드 블록을 다시 실행하고 `false`면 루프를 종료한다.

일반적인 사용은 다음과 같다:

```js
for (let i = 0 ; i < 10; i++) {
  console.log(i); // 0부터 9까지 출력
}
```

만약 초기식가 필요없다면 생략할 수 있다. 증감식도 마찬가지:

```js
var i = 0;
for (; i < 10;) { // 초기식와 증감식 생략
  console.log(i); // 0부터 9까지 출력
  i++;
}
```

루프 헤더는 생략할 수 있는데, 이렇게 하면 무한 루프가 된다:

```js
for (;;) {
  // 무한 루프
}
```

쉼표 연산자를 사용하면 여러 변수의 초기식이나 증감식을 적용할 수 있다:

```js
for (let i = 0, j = 10; i < 10; i++, j--) {
  console.log('i: ', i); // 0부터 9까지 출력
  console.log('j: ', j); // 10부터 1까지 출력
}
```

초기식에서 `let` 대신 `var` 키워드를 사용하면 끌어올림(hoisting)이 적용되는데, 이 때문에 버그를 유발할 수 있으니 되도록이면 `let`을 쓰자:

```js
var i = 'global';
(function() {
  console.log(i); // 끌어올림이 적용되어 'global'이 아니라 undefined 출력됨
  for (var i = 0 ; i < 10; i++) {
    console.log(i); // 0부터 9까지 출력
  }
})();
```

### while

```
while (조건식) { 
  코드 블록
}
```

조건식을 평가해 `true`일 때 코드 블록을 실행한다. 코드 블록의 끝을 만나면 조건식을 평가한다. 조건식이 `true`면 코드 블록을 다시 실행하고 `false`면 루프를 종료한다.

```js
var i = 0;

while (++i < 10) {
  console.log(i); // 1부터 9까지 출력
}
```

조건식을 `true`로 작성하면 무한 루프가 된다:

```js
while (true) {
  // (break를 만날 때까지 반복 실행되는) 코드 블록
}
```

### do-while

```
do { 
  실행 블록
} while (조건식);
```

코드 블록을 우선 실행한 후 조건식을 평가한다. 조건식을 나중에 평가한다는 점만 빼면 나머지는 `while`과 같다.

```js
var i = 0;

do {
  console.log(i); // 0부터 9까지 출력
} while (++i < 10);
```

### for-in

```
for (변수 in 객체) { 
  코드 블록
}
```

> The for...in statement iterates over all enumerable properties of an object that are keyed by strings (ignoring ones keyed by Symbols), including inherited enumerable properties.
>
> [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in)

각 루프마다 객체의 프로퍼티를 하나씩 꺼내 지정한 변수에 할당하고 코드 블록을 실행한다. 이 과정은 객체의 열거할 수 있는 프로퍼티의 수 만큼 

객체가 소유한 프로퍼티의 길이만큼 루프를 반복하며, 각 루프마다 객체의 프로퍼티를 하나씩 꺼내 지정한 변수에 할당한다.

모든 프로퍼티의 수 만큼 반복하는 것은 아니고 열거 가능한 자체 프로퍼티(enumerable own properties)의 길이만큼만 반복한다. 열거 할 수 있는 프로퍼티란, [객체 고유의 프로퍼티 설명자](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)에서 `enumerable`이 `true`인 프로퍼티를 말한다. 가령 `Object.prototype.toString()`은 `for-in`에서 무시되는데 `enumerable`이 `false`라서 그렇다. (단, 자바스크립트 구현체마다 열거 가능한 프로퍼티가 동일하지 않을 수 있음)

```js
for (let prop in window) {
  console.log(window[prop]);
}
```

자바스크립트는 루프를 실행하기 전에 먼저 객체를 평가한다. 객체 타입이 아니라 원시 타입이면 래퍼(wrapper) 객체로 대체된다:

```js
var str = 'abc';
for (let prop in str) {
  console.log(str[prop]); // a b c 출력
}
```

배열은 객체로 취급되며 배열을 구성하는 각 요소가 배열의 프로퍼티다. 그리고 배열의 인덱스가 프로퍼티 이름이 된다:

```js
var arr = ['a', 'b', 'c'];
for (let prop in arr) {
  console.log(prop); // 0 1 2 출력
}
```

### for-of

```
for (변수 of 객체) {
  코드 블록
}
```

`for-of`는 `for-in`과 비슷하지만 프로퍼티의 이름이 아니라 프로퍼티의 값을 변수에 할당한다. **반복 가능한 객체(iterable object)만 `for-of`를 사용할 수 있다.**, 반복 가능한 객체의 데이터 타입은 `arguments object`와 `Array`, `String`, `TypedArray`, `Map`, `Set`, `NodeList` 등이 있다.  

[ES2015에서 새로 추가되었고](http://www.ecma-international.org/ecma-262/6.0/#sec-for-in-and-for-of-statements), IE에서는 사용할 수 없다.

```js
var arr = ['a', 'b', 'c', 'd'];
for (let ele of arr) {
  console.debug(ele); // 'a', 'b', 'c', 'd' 각각 출력
}

var obj = {
  a: 1, b: 2
}
for (let val of obj) {} // TypeError: obj is not iterable
```


## 점프문

### label

특정 루프 헤더나 `case` 레이블의 앞에 식별자와 콜론으로 label을 표시한다. label은 `continue`와 `break` 키워드와 조합하여 점프문으로 사용된다. 보통은 중첩된 루프나 `switch`에서 사용한다.

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

`break`를 감싸고 있는 가장 가까운 반복문이나 `switch`를 탈출한다. label이 명시하면 현재 위치와 상관 없이 해당 위치로 점프한다. `break`는 반복문 내에서 사용해야 하며, 이외 위치에선 `SyntaxError`가 발생한다.

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

반복문 내에서 사용하며 `break`와 달리 반복문을 탈출하지 않고 다음 루프로 넘어간다. '생략'이라고 보면 된다. 예를 들어 `for`에서는 `continue`를 만났을 때 코드 블록의 실행을 중단하고 증감식으로 점프한다. label을 명시하면 해당 위치부터 다음 반복을 진행한다. `break`와 마찬가지로 `continue`도 반복문 내에서만 사용할 수 있다.

```
continue [label];
```

```js
var arr = [1, 2, 3];
for (let idx in arr) {
  if (arr[idx] == 2) {
    continue;
  }
  console.debug(arr[idx]); // 2는 건너뛰므로 1 3 출력
}
```

### return

`return`은 함수에서 함수 호출부에 특정한 값을 반환할 때 사용하는 키워드다. 함수 내에서만 유효하며, 이외 지역에선 `SyntaxError`가 발생한다. 

```
return 표현식;
```

평가된 표현식의 결과를 현재 함수를 호출한 지역으로 반환한다. 표현식은 생략될 수 있으며 이 경우 반환되는 값은 `undefined`가 된다.

```js
function add(a, b) {
  return a + b;
}
add('a', 'b'); // 'ab' 반환됨
```

### throw

`Error`를 발생시킨다.

```js
function add(a, b) {
  if (isNaN(a) || isNaN(b)) {
    throw new Error('숫자가 아니네');
  }
  return a + b;
}
add('a', 'b'); // Error: 숫자가 아니네
```


## 예외 처리 exception handling

### try-catch-finally

try는 '예외가 발생할지도 모르는' 코드 블록을 정의한다. 이어지는 `catch`의 코드 블록은 `try`에서 예외가 발생했을 때 실행된다. `finally`는 예외 발생 여부와 관계없이 마지막에 반드시 한 번은 실행되는 코드 블록이다. `finally`는 생략 가능하다.

```
try { 
  코드 블록
} catch (error) { 
  코드 블록
} finally { 
  코드 블록
}
```

- `error`: `Error.prototype`의 인스턴스. 프로퍼티로 `name`, `message`, `lineNumber`, `fileName`이 있다.

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
    // do something
  } finally {
    return;
  }
}
fn(); // undefined
```

실제 반환되는 값은 `finally`의 `undefined`다.
