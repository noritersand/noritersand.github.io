---
layout: post
date: 2016-07-27 12:04:00 +0900
title: '[JavaScript] Number'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - number
  - standard-built-in-object
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)

Standard built-in Objects: Number


## 개요

표준 내장 객체 `Number`의 필드/메서드 설명 글.


## Number() 생성자 함수

`new Number()`는 주어진 값을 숫자로 변환한다. 주어진 값이 `underfined`이거나 수가 아닌 문자, 길이가 `2` 이상인 배열일 때 `NaN`을 반환한다.

이 생성자 함수의 반환 타입은 레퍼 객체인데, `new`를 생략하고 일반 함수 `Number()`로 호출하면 원시 타입의 값을 반환한다. 다만 이 둘의 변환 방식은 같다.


## 스태틱 프로퍼티

### Number.EPSILON

`2.220446049250313e-16` 값을 반환하는 상수. (소수점으로 표현하면 `2.220446049250313 / 10^16 = 0.0000000000000002220446049250313`)

자바스크립트는 ECMAScript 표준에 따라 자바스크립트의 모든 산술을 IEEE 754 배정밀도 부동소수점 연산(Double-precision floating-point arithmetic, 64비트)을 사용하여 수행하는데, `Number.EPSILON`은 이 연산에서 1과 1보다 큰 다음 표현 가능한 숫자 사이의 가장 작은 차이를, 쉽게 말하면 1과 그보다 약간 큰 수를 구분할 수 있는 최소한의 간격을 의미한다.

이 값은 아주 작은 값이라서, 보통 부동소수점 비교 연산에서 사실상 같다고 볼 수 있는 범위를 결정할 때 활용한다. 예를 들어 두 수 `a`와 `b`의 차이가 `Number.EPSILON` 보다 작다면, 이를 사실상 같은 값으로 간주하는 것이다.

```js
var result = 0.2 - 0.3 + 0.1;
result; // 2.7755575615628914e-17
result < Number.EPSILON; // true
```

위의 예시에서 `0.2 - 0.3 + 0.1`의 결과는 고정소수점 연산에선 `0`이지만 부동소수점 연산에선 `2.7755575615628914e-17`이 된다(소수점 표현으로는 `0.000000000000000027755575615628914`). 그리고 이 값은 `Number.EPSILON`보다도 작다.

ℹ️ 엡실론`Εε`은 그리스어 알파벳의 다섯째 글자다.

### Number.MAX_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수. 2의 53제곱에 1을 뺀 값으로 `9007199254740991`이다(약 구천조). 여기서 '안전'하다는 말은 이 값을 넘어가면 에러가 발생한다는 뜻이 아니라, 수학적으로 부정확한(mathematically incorrect) 연산 결과가 나온다는 뜻이다.

자바스크립트의 숫자 타입 `Number`는 이중 정밀도 부동 소수점 형식을 사용하여 64비트로 숫자를 표현한다. 이 형식은 53비트의 정밀도를 제공하기 때문에, 정확하게 표현할 수 있는 정수의 범위는 `-(2^53 - 1)`부터 `2^53 - 1`까지다. 이 범위를 넘어서는 정수는 정확하게 표현되지 않거나 정밀도 손실이 발생할 수 있다.

```js
typeof Number.MAX_SAFE_INTEGER; // "number" 
Number.MAX_SAFE_INTEGER; // 9007199254740991

var unsafeNumber1 = Number.MAX_SAFE_INTEGER + 1;

unsafeNumber1; // 9007199254740992
Number.MAX_SAFE_INTEGER === unsafeNumber1; // false

var unsafeNumber2 = unsafeNumber1 + 1;

unsafeNumber2; // 9007199254740992
unsafeNumber1 === unsafeNumber2; // true
unsafeNumber2 - unsafeNumber1; // 0
```

위 예시를 보면 `9007199254740991` 보다 큰 `number` 값의 비교와 연산을 제대로 처리하지 못한다.

관련 함수로 `Number.isSafeInteger()`가 있다. 그리고 만약 `Number.MAX_SAFE_INTEGER`를 초과하는 정수를 다뤄야 한다면 [`BigInt`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) 타입을 쓰자.

### Number.MAX_VALUE

자바스크립트에서 표현할 수 있는 가장 큰 양의 부동소수점 수를 나타내는 상수.

```js
Number.MAX_VALUE; // 1.7976931348623157e+308

var foo = Number.MAX_VALUE + 1000;
foo === Number.MAX_VALUE; // true

Number.MAX_VALUE + Number.MAX_VALUE; // Infinity
Number.MAX_VALUE * 2; // Infinity
```

`1.7976931348623157e+308`은 `1.7976931348623157 x (10^308)` 이므로:

```
179769313486231570000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
```

...가 된다. 그런데 사실 이 값은 근사치고, 정확한 값은 `2^1024 - 2^971`이며, 고정 소수점 표기법으로:

```
179769313486231570814527423731704356798070567525844996598917476803157260780028538760589558632766878171540458953514382464234321326889464182768467546703537516986049910576551282076245490090389328944075868508455133942304583236903222948165808559332123348274797826204144723168738177180919299881250404026184124858368
```

이라 한다... 🙄

#### Number.MAX_SAFE_INTEGER와 비교

```js
Number.MAX_SAFE_INTEGER < Number.MAX_VALUE; // true
```

`Number.MAX_VALUE`는 매우 큰 부동소수점 수의 한계를 나타내며, `Number.MAX_SAFE_INTEGER`보다 훨씬 크다.

### Number.MIN_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수. 반환하는 값은 `-(2^53 - 1)` 혹은 `-9007199254740991`이다. 여기서 '안전'하다는 말은 `Number.MAX_SAFE_INTEGER`와 `Number.MIN_SAFE_INTEGER` 범위 내의 정수들이 `Number` 타입(IEEE 754 표준에 따른 64비트 부동소수점 숫자)으로 정확하게 표현되고, 연산 과정에서 정밀도 손실 없이 사용할 수 있다는 것을 의미한다.

```js
typeof Number.MIN_SAFE_INTEGER; // "number"
Number.MIN_SAFE_INTEGER - 1; // -9007199254740992

var unsafeNumber1 = Number.MIN_SAFE_INTEGER - 1;

unsafeNumber1; // -9007199254740992
Number.MIN_SAFE_INTEGER === unsafeNumber1; // false

var unsafeNumber2 = unsafeNumber1 - 1;

unsafeNumber2; // -9007199254740992
unsafeNumber1 === unsafeNumber2; // true
unsafeNumber1 - unsafeNumber2; // 0
```

### Number.MIN_VALUE

자바스크립트에서 표현할 수 있는 가장 작은 양의 부동소수점 수를 나타낸다.

```js
Number.MIN_VALUE; // 5e-324
```

`5e-324`는 `5 x (10^-324)`이다. 그러니까 소수점 뒤에 323개의 `0`이 이어지고 마지막에 `5`가 오는 값이다:

```
0.000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000005
```

그런데 사실 이 값도 `MAX_VALUE`처럼 근사치고, 정확한 값은 `2^-1074`이며, 고정 소수점 표기법으로:

```
0.000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004940656458412465441765687928682213723650598026143247644255856825006755072702087518652998363616359923797965646954457177309266567103559397963987747960107818781263007131903114045278458171678489821036887186360569987307230500063874091535649843873124733972731696151400317153853980741262385655911710266585566867681870395603106249319452715914924553293054565444011274801297099995419319894090804165633245247571478690147267801593552386115501348035264934720193790268107107491703332226844753335720832431936092382893458368060106011506169809753078342277318329247904982524730776375927247874656084778203734469699533647017972677717585125660551199131504891101451037862738167250955837389733598993664809941164205702637090279242767544565229087538682506419718265533447265625
```

이라 함... 🙄

#### Number.MIN_SAFE_INTEGER와 비교

```js
Number.MIN_SAFE_INTEGER < Number.MIN_VALUE; // true
```

`Number.MIN_VALUE`는 `0`에 매우 가까운 양의 값이고, `Number.MIN_SAFE_INTEGER`는 절댓값이 매우 큰 음의 정수다. 따라서 `Number.MIN_SAFE_INTEGER`가 훨씬 작다.

#### Number.EPSILON과 비교

```js
Number.MIN_VALUE < Number.EPSILON; // true
```

`MIN_VALUE`는 `5e-324`, `EPSILON`은 `2.220446049250313e-16`이다.

지수 부분만 비교해봐도, `MIN_VALUE`는 `10^-324`, `EPSILON`은 `10^-16`이므로 `EPSILON`이 훨씬 크다.

### Number.NaN

'수가 아님'을 나타내는 값인 `NaN`을 의미하는 상수. 전역 객체의 `NaN`과 동일한 값이다. 수학 연산의 결과가 수가 될 수 없음을 나타낼 때 쓰인다.

`NaN`은 자신을 포함한 어떤 값과도 같지 않다. 일치 비교나 동등 비교로 `NaN`인지 알 수 없다는 말이다. 따라서 `NaN`인지 판단할 때는 `Number.isNaN()` 또는 `isNaN()` 함수를 사용해야 한다.

```js
Number.NaN; // NaN

Number.NaN === NaN; // false
NaN === NaN; // false

isNaN(Number.NaN); // true
isFinite(Number.NaN); // false
```

`typeof NaN`은 `"number"`으로 평가된다. '수가 아님'을 의미하더라도 표현 방식 자체는 64비트 부동소수점이며, `NaN`도 내부적으로는 숫자 타입의 표현 방식(부호, 지수, 가수)을 사용하기 때문이라고 한다.

```js
typeof NaN; // "number"
```

### Number.NEGATIVE_INFINITY

음의 무한대를 나타내는 상수. 수학적으로 표현하면 `-∞`

```js
Number.NEGATIVE_INFINITY; // -Infinity 
Number.NEGATIVE_INFINITY === -Infinity; // true

Number.NEGATIVE_INFINITY * 2; // -Infinity
Number.NEGATIVE_INFINITY * 2 === Number.NEGATIVE_INFINITY; // true
```

표현 가능한 수의 범위를 음의 방향으로 초과하는 연산은 `-Infinity`를 반환한다:

```js
-1e308 * 2; // -Infinity 
-Number.MAX_VALUE * 2; // -Infinity
```

특징으로, 음의 무한대와 어떤 유한한 수를 더하거나 빼더라도 결과는 여전히 음의 무한대라는 것, 모든 유한한 수보다 작다는 것, 그리고 음의 무한대에서 음의 무한대를 빼거나 양의 무한대를 더한 결과는 `NaN`이라는 것이 있다:

```js
Number.NEGATIVE_INFINITY + 100; // -Infinity
Number.NEGATIVE_INFINITY - 100; // -Infinity

Number.NEGATIVE_INFINITY < -100; // true
Number.NEGATIVE_INFINITY < Number.MIN_SAFE_INTEGER; // true
Number.NEGATIVE_INFINITY < Number.MIN_VALUE; // true
Number.NEGATIVE_INFINITY < Number.EPSILON; // true 

Number.NEGATIVE_INFINITY - Number.NEGATIVE_INFINITY; // NaN
Number.NEGATIVE_INFINITY + Number.POSITIVE_INFINITY; // NaN
```

### Number.POSITIVE_INFINITY

양의 무한대를 나타내는 상수. 전역 변수 `Infinity`와 동일한 값이다. 수학적 표현은 `∞`

```js
Number.POSITIVE_INFINITY; // Infinity
Number.POSITIVE_INFINITY === Infinity; // true

Number.POSITIVE_INFINITY * 2; // Infinity
Number.POSITIVE_INFINITY * 2 === Number.POSITIVE_INFINITY; // true
```

`1`을 `0`으로 나누거나, 표현 가능한 수의 범위를 초과하는 연산은 `Infinity`를 반환한다:

```js
1 / 0; // Infinity
1e308 * 2; // Infinity
Number.MAX_VALUE * 2; // Infinity
```

특징으로, 양의 무한대에 어떤 유한한 수를 더하거나 빼더라도 결과는 여전희 양의 무한대라는 것, 모든 유한한 수보다 크다는 것, 양의 무한대에서 양의 무한대를 빼거나 음의 무한대를 더한 결과는 `NaN`이라는 것이 있다:

```js
Number.POSITIVE_INFINITY + 100; // Infinity
Number.POSITIVE_INFINITY - 100; // Infinity 

100 < Number.POSITIVE_INFINITY; // true 
Number.MAX_SAFE_INTEGER < Number.POSITIVE_INFINITY; // true
Number.MAX_VALUE < Number.POSITIVE_INFINITY; // true

Number.POSITIVE_INFINITY - Number.POSITIVE_INFINITY; // NaN
Number.POSITIVE_INFINITY + Number.NEGATIVE_INFINITY; // NaN
```


## 스태틱 메서드

### Number.isFinite()

주어진 값이 유한한 수인지 확인하는 메서드.

```
Number.isFinite(value)
```

`value`가 숫자이며 `Infinity`, `NaN`, `undefined`가 아니면 `true`를 반환한다:

```js
Number.isFinite(123); // true
Number.isFinite(-123.45); // true
Number.isFinite(0); // true
Number.isFinite(0x12); // true

Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
Number.isFinite(NaN); // false
Number.isFinite(undefined); // false
```

전역 내장 함수인 `isFinite()`와 다르게 암묵적 타입 변환은 발생하지 않는다:

```js
Number.isFinite(null); // false
Number.isFinite(''); // false
Number.isFinite('123'); // false
Number.isFinite(' '); // false
Number.isFinite(true); // false
Number.isFinite(false); // false
Number.isFinite([]); // false

isFinite(null); // true
isFinite(''); // true
isFinite('123'); // true
isFinite(' '); // true
isFinite(true); // true
isFinite(false); // true
isFinite([]); // true
```

### Number.isInteger()

주어진 값이 정수인지 판단하는 메서드. 값이 유한한 수이고 소수점이 없거나 소수점 아래가 `0`이면 `true`를 반환한다.

```js
Number.isInteger(5); // true
Number.isInteger(5.0); // true
Number.isInteger(5.5); // false
Number.isInteger(5.5); // false
Number.isInteger(NaN); // false
Number.isInteger(Infinity); // false
```

MDN의 설명에 따르면 이 메서드는 특이사항이 몇 가지 있다.

우선 소수점 아래 값이 너무 작으면 오차가 발생한다. 가령 `5.0000000000000001`은 실제 정수가 아니지만 `true`를 반환하는데, 소수점 아래인 `0.0000000000000001`(= `1e-16`)은 정수가 아니지만 `Number.EPSILON`(= `2.220446049250313e-16`)보다 작아서 정밀도 한계로 인해 정수로 간주되는 경우다:

우선 소수점 아래 값이 너무 작으면 오차가 발생한다. 예를 들어, `5.0000000000000001`은 실제로는 정수가 아니지만 `true`를 반환한다. 소수점 아래 값 `0.0000000000000001`(= `1e-16`)이 `Number.EPSILON`(약 `2.220446049250313e-16`)보다 작아 정밀도 한계로 인해 정수로 간주되는 경우다:

```js
Number.isInteger(5.0000000000000001); // true
```

그리고 `Number.MAX_SAFE_INTEGER` 값 `9.007199254740991e+15`와 자리수 기준으로 근접한 수에 도달하면 정밀도 손상이 발생하여 정수 판단을 제대로 하지 못한다. 이것은 소수점 아래를 표현할 비트가 부족하여 발생하는 현상이다:

```js
Number.isInteger(4500000000000000.5); // false
Number.isInteger(4500000000000000.1); // true
```

### Number.isNaN()

주어진 값이 `NaN`인지 판단하는 메서드.

```js
Number.isNaN(NaN); // true
Number.isNaN(5); // false
Number.isNaN('5'); // false
```

전역 함수 `isNaN()`보다 엄격한(원문: robust) 버전으로, `isNaN()`과 다르게 정확히 `NaN`일 때만 `true`를 반환한다:

```js
Number.isNaN('not a number'); // false
Number.isNaN('false'); // false
Number.isNaN(undefined); // false
Number.isNaN({}); // false

isNaN('not a number'); // true (숫자로 변환 불가)
isNaN('false'); // true (문자열 "false"는 숫자로 변환 불가)
isNaN(undefined); // true
isNaN({}); // true
```

ℹ️ `NaN`은 비교 연산에서 `false`로 평가되기 때문에, 동등 연산자(`==`)나 일치 연산자(`===`)로 판단할 수 없다.

### Number.isSafeInteger()

주어진 값이 정밀도 오차 없이 표현 가능한 정수인지 여부를 판단하는 메서드. `Number.MIN_SAFE_INTEGER`와 `Number.MAX_SAFE_INTEGER`의 사이에 있는 정수면 `true`를 반환한다.

```js
Number.isSafeInteger(100); // true
Number.isSafeInteger(3.14); // false
Number.isSafeInteger(Number.MAX_SAFE_INTEGER); // true
Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1); // false
Number.isSafeInteger(Number.MIN_SAFE_INTEGER); // true
Number.isSafeInteger(Number.MIN_SAFE_INTEGER - 1); // false
```

### Number.parseFloat()

주어진 값을 부동 소수점 형식의 숫자로 변환한다. 전역 함수 `parseFloat()`와 동일한 함수다.

이 메서드는 파라미터를 `string`으로 간주하며, 맨 앞의 공백을 무시한다. 공백이 아닌 첫 번째 문자를 `number`로 변환할 수 없으면 `NaN`을 반환한다:

```js
Number.parseFloat('0'); // 0
Number.parseFloat('1234'); // 1234
Number.parseFloat('3.14159265'); // 3.14159265
Number.parseFloat(Number.MIN_VALUE); // 5e-324

const f = Number.parseFloat('1.234');
Object.getPrototypeOf(f); // Number { 0 }
```

숫자가 이어지는 도중 문자를 만나면, 그때까지의 유효한 숫자만 잘라낸다:

```js
Number.parseFloat('4a'); // 4
Number.parseFloat('48a'); // 48
Number.parseFloat('4.a'); // 4
Number.parseFloat('4.32a'); // 4.32
Number.parseFloat('4.3219876a'); // 4.3219876
Number.parseFloat(null); // NaN
Number.parseFloat(undefined); // NaN
```

### Number.parseInt()

문자열을 정수(Integer)로 변환한다. 주어진 문자열의 맨 앞부터 이어지는 공백은 무시하며, 소수점 이하는 버린다. 전역 함수 `paseInt()`와 동일한 함수다.

```
Number.parseInt(string)
Number.parseInt(string, radix)
```

- `string`: 정수로 변환할 값. 공백이 아닌 첫 번째 문자를 `number`로 변환할 수 없으면 `NaN`을 반환한다.
- `radix`: 진법의 기수. 생략하면 `string`이 `0x`로 시작할 때만 16진수로, 나머지는 10진수로 해석한다.

```js
Number.parseInt('123'); // 123
Number.parseInt("42.7"); // 42
Number.parseInt([1]); // 1
Number.parseInt([1, 2]); // 1
Number.parseInt('a'); // NaN
Number.parseInt('   123'); // 123
Number.parseInt('123abc'); // 123
Number.parseInt('abc123'); // NaN
Number.parseInt('11', 2); // 3
Number.parseInt('ff', 16); // 255
Number.parseInt(null); // NaN
Number.parseInt(undefined); // NaN
```

#### Number()와 다른점

`Number()`는 소수점 처리가 가능하고, `NaN`으로 판단하는 게 약간 다르다:

```js
Number([1, 2]); // NaN
Number('123abc'); // NaN
Number(null); // 0
Number(undefined); // NaN
```


## 인스턴스 메서드 

### Number.prototype.toExponential()

**TODO**

### Number.prototype.toFixed()

부동 소수점으로 다뤄지고 있는 소수점 아래의 수를 고정 소수점으로 변환하여 표시하는 메서드.

```
number.toFixed()
number.toFixed(digits)
```

- `digits`: 소수점 아래 자릿수. `0`부터 `100`까지 지정할 수 있고 생략하면 `0`이다.

`Number` 인스턴스의 값을 지정한 소수점 아래 자릿수만큼의 길이를 갖는 문자열로 반환한다:

```js
(0.55).toFixed(20); // "0.55000000000000004441" 
```

만약 소수점 아래의 수가 지정한 길이보다 길면 반올림하여 반환한다:

```js
(0.1).toFixed(30); // "0.100000000000000005551115123126" 
(0.1).toFixed(18); // "0.100000000000000006" 
```

MDN에선 이를 고정 소수점 표기법(fixed-point notation)이라 설명한다.

### Number.prototype.toLocaleString()

**TODO**

### Number.prototype.toPrecision()

**TODO**

### Number.prototype.toString()

**TODO**

### Number.prototype.valueOf()

**TODO**
