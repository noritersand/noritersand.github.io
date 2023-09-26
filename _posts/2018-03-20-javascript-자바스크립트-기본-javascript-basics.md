---
layout: post
date: 2018-03-20 14:40:10 +0900
title: '[JavaScript] 자바스크립트 기본'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - language
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [TC39 – Specifying JavaScript.](https://tc39.es/)
- [ECMAScript 20xx Language Specification](https://tc39.es/ecma262/)
- [ECMAScript Living Standard](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/)
- [\[MDN\] Grammar and Types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_Types)
- [\[MDN\] Data structures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)
- [\[MDN\] Object initializer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)
- [http://www.insightbook.co.kr/book/programming-insight/자바스크립트-완벽-가이드](http://www.insightbook.co.kr/book/programming-insight/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C)


## 버전 넘버링

JavaScript는 ECMAScript 표준을 따른다. ECMAScript의 버전은 ES5, ES6 같은 단순 넘버링이었다가 표준이 만들어진 연도를 이름으로 사용하도록 변경되었다. ES2015(ES6), ES2016(ES7), ... 대충 2009를 더하면 된다. (6 + 2009 = 2015)

이 글을 수정한 시점 기준으로 가장 최근 버전은 ES2019인데, 이 경우 ES2019 라고만 적어도 되지만 ES2019(ES10) 이렇게 이전 방식의 버전을 병기하기도 한다.


## Plain JavaScript

라이브러리나 프레임웤을 사용하지 않는 순수 자바스크립트를 의미한다. Pure JavaScript 혹은 Vanilla JavaScript 라고도 하는데, pure.js라는 템플릿 엔진과, Vanilla JS라는 프레임웤이 있어서(이놈들은 왜 이름을 이따구로 지어서 지롤 😒) plain이 가장 적절함.

[https://plainjs.com/](https://plainjs.com/)에 jQuery를 대체하는 순수 자바스크립트 API가 정리되어 있음.


## 개념 정리

### 호출 스택 call stack

현재 실행 중인 함수와, 해당 함수 내에서 호출되는 함수들의 호출 순서를 기억하기 위한 메커니즘. 호출 스택의 맨 위에 현재 실행중인 함수가 위치한다. 실행이 완료된 함수는 스택에서 제거된다.

### 스택 프레임 stack frame

호출 스택에 추가되는 각각의 레코드로, 현재 실행 중인 함수의 정보를 담고 있다. 이 정보에는 함수 파라미터, 지역 변수, 반환 주소 등이 포함된다.

### 실행 컨텍스트 execution context

자바스크립트 엔진이 코드를 실행하는 환경을 말한다. 세부적으로는 전역 실행 컨텍스트, 함수 실행 컨텍스트, eval 실행 컨텍스트로 나뉜다. 각각의 실행 컨텍스트는 변수 객체, 스코프 체인, this 바인딩 등 실행할 코드에 필요한 환경 정보를 갖는다.

전역 실행 컨텍스트는 스크립트가 처음 실행될 때 생성되며 전역 범위를 나타낸다. 함수 실행 컨텍스트는 함수가 호출될 때마다 생성되며 함수의 로컬 범위를 나타낸다. (eval 실행 컨텍스트는 생략)

실행 컨텍스트는 코드가 실행될 때 생성되며, 호출 스택에 적재된다. 현재 실행 중인 실행 컨텍스트는 호출 스택의 맨 위에 위치하며, 해당 실행 컨텍스트의 코드가 모두 실행되면 호출 스택에서 제거된다.

### 콜백 큐 callback queue

비동기 작업의 콜백 함수가 대기하는 곳. 여기에 있던 함수들은 나중에 호출 스택에 넣어진다.

### 이벤트 루프 event loop

자바스크립트 엔진이 작업을 처리하는 방식을 설명하는 개념이다. 이벤트 루프는 콜백 큐에서 대기 중인 작업을 가져와 호출 스택이 비어 있을 때 실행한다. 자바스크립트의 비동기 작업은 이런식으로 이벤트 루프와 콜백 큐에 의해 처리된다.

### 메인 쓰레드 main thread

브라우저가 사용자 이벤트와 paints(이게뭐시여)를 처리하는 곳. main execution thread라고도 함.


## 코멘트 처리

### SingleLine Comment

```js
// 코멘트 처리될 문자열
```

### MultiLine Comment

```js
/*
  코멘트
  처리될
  문자열
*/
```


## 명령줄 구분

2개 이상의 스크립트 명령을 사용할 경우 세미콜론`;`으로 구분한다.

```
표현식1; 표현식2;
표현식3;
```

```js
var a = 1; var b = 2;
var c = a + b;
```

단, 아래처럼 예외적으로 줄 바꿈을 세미콜론으로 해석하는 경우가 있다:

```js
var a
a
=
3
console.log(a)

// var a; a=3; console.log(a);
```

`return`, `break`, `continue` 문 후에 줄 바꿈 할 경우:

```js
return
true;

// return; true;
```

`++`, `--` 후에 줄 바꿈 할 경우:

```js
x
++
y

// x; ++y;
```

대부분 현재 구문의 다음에 오는 공백이 아닌 문자를 해석할 수 없을 때 세미콜론으로 해석하는데, 이를 _자동 세미콜론 삽입_ 이라고 한다. 가독성을 위한 줄 바꿈을 사용할 때 자동 세미콜론 삽입을 피하기 위해서 원래대로면 없어도 되는 괄호를 추가하기도 한다.

```js
var a = (
  1
  +
  2
)
```


## HTML 문서에 자바스크립트 포함시키기

### inline

HTML 태그 속성에 지정하여 사용. `<body>` 내에서 사용한다.

```html
<a href="javascript:location.href='http://daum.net'">다음</a>
```

### embedded(internal)

스크립트 블록`<script></script>` 안에 일괄 지정. `<head>`와 `<body>` 어느곳에든 사용할 수 있다.

```html
<html>
  <head>
    <script>
      document.write("경고창 전에 출력될 문자"); alert("redalert?");
      document.write("경고창 후 출력될 문자");
    </script>
  </head>
  <body></body>
</html>
```

### linked(external)

외부 파일을 링크하여 여러 개의 파일에 일괄 지정. `<body>`에 사용해도 가능하지만 보통은 `<head>`에 위치한다.

```html
<html>
  <head>
    <script src='test.js'></script>
  </head>
  <body>
  </body>
</html>
```


## 특수 문자의 출력

특수문자는 이스케이프 시퀀스로 표현한다:

- `\u`: 유니코드 이스케이프 시퀀스
- `\x`: Latin-1 이스케이프 시퀀스
- `\n`: 줄 바꿈 `\u000A`
- `\b`: 백스페이스 `\u0008`
- `\t`: 수평 탭 `\u000B`
- `\v`: 수직 탭 `\u000B`
- `\r`: CR, 캐리지리턴 `\u000D`
- `\0`: `null` `\u0000`
- `\f`: 폼 피드 `\u000C`
- `\"`: 큰따옴표
 `\u0022`
- `\'`: 작은따옴표
 `\u0027`
- `\r`: 줄 바꿈 문자 `\u000A`
- `\\`: 백슬래시 `\u005C`


## 따옴표 처리

```
console.log("'테스트'");   // '테스트'
console.log("\"테스트\"");   // "테스트"
console.log('"테스트"');   // "테스트"
```


## 자바스크립트의 변수

### 식별자 명명 규칙

기본규칙:

- 영문자, 숫자, `_`, `$` 기호등을 조합하여 지정한다.
- 변수의 첫글자는 반드시 영문자, `$`, `_` 기호중의 하나를 이용하여야 한다.
- 예약어는 사용할수 없다.
- 대소문자를 구분한다.

아래 키워드들은 식별자로 사용할 수 없다:

```
break  delete  function  return  typeof  case
do  if  switch  var  catch  else
in  this  void  continue  false  instanceof
throw  while  debugger  finally  new  true
with  default  for  null  try
```

아래 이름들은 전역 변수 혹은 전역 함수이므로 식별자로 사용하면 안된다:

```
arguments  encodeURI  Infinity  Number  RegExp
Array  encodeURIComponent  isFinite  Object  String
Boolean  Error  isNaN  parseFloat  SyntaxError
Date  eval  JSON  parseInt  TypeError
decodeURI  EvalError  Math  RangeError  undefined
decodeURIComponent  Function  NaN  ReferenceError  URIError
```

아래 키워드들은 ECMAScript 5 에서 사용할 수 없다:

```
class  const  enum  export
extends  import  super
```

strict mode(엄격 모드)에서는 아래 예약어들을 사용할 수 없다:

```
implements  let  private  public
yield  interface  package  protected
static  arguments  eval
```

아래 키워드들은 ECMAScript 3 에서 사용할 수 없다:

```
abstract  double  goto  native  static
boolean  enum  implements  package  super
byte  export  import  private  synchronized
char  extends  int  protected  throws
class  final  interface  public  transient
const  float  long  short  volatile
```

### 변수의 데이터 타입

- `boolean`: `true`, `false`값을 가지는 논리 데이터 타입. `Boolean()` 함수를 이용하여 검증을 수행할수 있다.
- `number`: 숫자를 표현하거나 산술 연산을 하는데 사용되는 데이터 타입이다. 기본적으로 `+`, `-`, `*`, `/` 등의 산술연산이 가능하며 `Math` 라는 내장객체를 이용하여 수학함수를 이용한 결과를 얻을 수도 있다. 자바스크립트의 `Number`는 '64비트 형식 IEEE 754 값' 으로 정의 된다. 이 때문에 간혹 의도하지 않은 결과가 나오기도 한다.
- `string`: 문자열을 표현하는데 사용되는 데이터 타입이다. 자바스크립트의 문자열은 16비트 유니코드 문자들의 연결 구조이기도 하다. 즉 문자열이라 함은 문자 하나하나가 연결되어 하나의 표현을 이루는 데이터를 말하는 것이다.
- `null`, `undefined`: `null`은 아무 값도 갖지 않거나(no value) 객체가 아닌 것을 의미하는 특수한 객체 값이다. 반면 undefined는 값 자체가 없음을 나타낸다. 초기화되어 있지 않거나 존재하지 않는 객체 프로퍼티 혹은 배열의 원소값에 접근하려고 할 때 얻는 값이다. `null`과 `undifined`는 둘 다 값이 없음을 가리키고 `false`로 판정된다. 동등 비교`==`에서 둘은 같다고 간주하지만 엄격한 동등 비교`===`에선 다른것으로 나온다.
- `symbol`: ES2015에서 처음 정의된 데이터 타입. 고유함(unique)이 보장되며 객체에 고유한 키를 속성으로 추가할 때 많이 쓰인다.
- `object`: 일반적인 값이 아닌 다른 객체의 참조값을 갖는 데이터 타입. `Array`, `Date`, `RegExp`, `Error` 등이 object 타입이다.
- `function`: 함수 타입. 자바스크립트에서 함수는 `Function` 프로토타입의 인스턴스를 의미한다.

#### string vs String

`string`은 문자열의 원시 타입을 의미한다. `String`은 `String()` 생성자 함수를 의미할 수도 있고, `String` 프로토타입을 의미할 수도 있다. 

하지만 모든 소문자로 작성한 타입이 원시 타입을 의미하는 것은 아니다. `object`와 `function` 타입은 원시 타입이 아니다. 이 소문자로 나타내는 타입의 범위는 `typeof` 연산자의 결과로 보이는 것까지인 것으로 보인다. 그러니까 자바스크립트에서 '`array` 타입'이란 말은 틀린 표현인 것이다(`typeof`는 배열을 `object` 타입이라 하니까...).

혼란을 피하고 싶다면 원시 타입을 제외한 나머지는 프로토타입을 쓴다. e.g., `number`, `string`, `boolean`, `symbol`, `Object`, `Function`, `Array`, `Node`, `Window`, ...


### 변수 선언과 초기화

```
var 변수명; 변수명 = 3;
혹은
var 변수명 = 3;
```

```js
var a;
a = 1;
var b = 2;
var c = a + b;
```

### 변수의 유효범위 Scope

자바스크립트의 변수는 블록 단위가 아닌 함수 단위로 유효범위를 갖는다.

```js
var a = 0;
{
  var b = 1;
}
console.debug(b); // 1
```

위 코드에서 `b`는 `a`와 블록으로 나뉘어 있지만 같은 함수에 있으므로 동일한 유효범위를 갖는다.

```js
var a = "에이";

function test() {
  b = "비";
  var c = "씨";
}

function test2() {
  document.write("a : " + a);  // 에이
  document.write("b : " + b);  // 비
  document.write("c : " + c);  // undefined
}

test();
test2();
```

위 코드에서 `c`는 함수 내에서 선언된 변수, 즉 지역 변수이기 때문에 아무것도 출력되지 않는다.

변수 b처럼 함수 내에서 선언되었어도 `var` 키워드를 생략하면 선언되지 않은 변수(undeclared variable)가 되며, 해당 변수는 마치 미리 선언된 전역 변수처럼 작동한다. 동시에 해당 변수는 전역 객체의 프로퍼티가 된다. (단, strict mode에선 허용되지 않음)


## 리터럴 Literal

리터럴이란 정의되어 있는 그대로 해석되어야 하는 값을 말한다. 숫자 혹은 문자로 표현된다. [http://www.terms.co.kr/literal.htm](http://www.terms.co.kr/literal.htm)

### 정수 리터럴

'0x', '0X' 로 시작하면 16진수로 인식한다.

```js
0xff   // 십진수 255  (15*16 + 15)
```

'0' 뒤에 0-7 사이의 숫자 시퀀스가 오면 8진수로 인식한다.

```js
0377   // 십진수 255 (3*64 + 7*8 + 7)
```

단, 8진수 표기는 ES 표준이 아니기 때문에 사용하지 않는것을 권장하고 있다. (ES5의 Strict mode에서는 명시적으로 0으로 시작하는 정수 표기법을 금지함.)

다음은 ES 표준인지 아닌지 확실치 않은 리터럴이다:

```js
0b110 // 십진수 6의 2진수 리터럴
0o17 // 십진수 15의 8진수 리터럴
```

### 부동소수점 리터럴

부동소수점 리터럴은 전통적인 지수표기법과 같다.

```
[digits][.digits][(E|e)[(+|-)]digits]
```

```js
6.02e23   // 6.02 * 1023
1.4738E-32   // 1.4738 * 10.32
```

### 문자열 리터럴

문자열은 연속적으로 나열된 16비트 값으로 표현한다. 각 문자는 수정할 수 없는 유니코드(UTF-16) 문자다.

```js
"Hello World!"
```

자바스크립트의 문자열 리터럴은 여타 언어와 마찬가지로 따옴표로 표현한다.
Java와 비교해 다른점은 큰따옴표, 작은따옴표 어느것을 사용해도 무방하다는 것이다:

```js
" ' testing' "   // ' testing ', 자바와 같다.
' " testing " '   // " testing ", 자바스크립트만 가능한 표현
```

이 외에 ES3에선 문자열 리터럴을 반드시 한 줄로 작성해야 하지만, ES5에선 줄 끝에 백슬래시`\`를 놓는 방식으로 여러 줄의 문자열 리터럴을 마치 한 줄인것 처럼 작성할 수 있다. 이때 문자 사이의 공백은 탭으로 처리된다.

```js
"one\
long\
line"
 // one    long    line
```

ES5에서 문자열은 읽기 전용 배열처럼 취급할 수 있다. (문자열, 숫자, 불리언은 기본적으로 읽기 전용 값이다.)

```js
s = "hello, world";
s[0];            // "h"
s[s.length-1];   // "d"
```

### 함수 리터럴

함수 리터럴은 다른 말로 '함수 정의 표현식'이라고 한다.

```js
var add = function(a, b) {
  return a + b;
};
```

함수 리터럴은 (함수 선언도 마찬가지지만) 내부적으로 `Function()` 생성자 함수를 사용하도록 변환된다.

### 배열 리터럴

```js
var arr = []; // new Array()와 같음
arr[0] = 'abc';
arr; // Array [ "abc" ]
var arry = [ '1', '2', [ '3-1', '3-2' ] ];
arry[1]; // "2"
arry[2][1]; // "3-1"
```

### 객체 리터럴

엄밀히 따져 배열 또한 객체지만 초기화 리터럴이 다르므로 둘을 구분 짓는다.

객체 리터럴은 '리터럴 표기에 의한 객체 생성(creating objects with literal notation)'이라고도 한다.

```js
var ob = {}; // new Object()와 같음
ob.a = 1;
ob; // Object { a: 1 }
var obj = {b: 2, c: {d: 3}};
obj.b; // 2
obj['b']; // 2
obj.c.d; // 3
```

객체 리터럴은 내부적으로 `Object()` 생성자 함수를 사용하도록 변환된다.

#### 프로퍼티 접근자 Property accessors

[https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors)

객체의 프로퍼티에 접근하는 방법은 두 가지로 점 표기법(dot notation, `obj.prop`)과 대괄호 표기법(bracket notation, `obj['prop']`)이 있다.

자바스크립트의 객체는 map, dictionary, hash 타입 같은 연관 배열(associative array)로 생각할 수 있으며 객체의 프로퍼티가 배열의 키가 된다고 한다. 이를 이용하는 것이 대괄호 표기법이다.

```js
var obj = {
  prop: 'hello'
};
obj['prop']; // 프로퍼티 접근자 중 대괄호 표기법으로 obj.prop과 같다
```

#### 메서드 리터럴

```js
var mankind = {
  walk: function() {}
};
mankind.walk();
```

전통적인 방법은 위와 같고, 다음은 ES2015의 단축 표기법이다:

```js
var mankind = {
  walk() {}
};
```

#### 객체 내부의 배열

```js
var complex = {
  numeric: 123,
  alphabet: 'abc',
  hangul: ['가', '나', '다']
};
complex.hangul[0]; // "가"
```
