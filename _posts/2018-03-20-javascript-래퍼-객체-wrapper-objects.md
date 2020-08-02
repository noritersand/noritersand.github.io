---
layout: post
date: 2018-03-20 16:08:35 +0900
title: '[JavaScript] 래퍼 객체 wrapper objects'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - wrapper
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)
- [http://www.insightbook.co.kr/book/programming-insight/자바스크립트-완벽-가이드](http://www.insightbook.co.kr/book/programming-insight/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C)

## 래퍼 객체란?

이름처럼 래퍼<sup>wrapper</sup>는 원시<sup>primitive</sup> 타입의 값을 감싸는 형태의 객체다. `number`, `string`, `boolean`, `symbol` 데이터 타입에 각각 대응하는 `Number`, `String`, `Boolean`, `Symbol`이 제공된다. 래퍼 객체와 원시 타입간의 변환은 자바스크립트가 알아서 해주기 때문에 우리는 이를 신경쓰지 않아도 되며 명시적인 처리가 필요한 경우도 거의 없다.

다만 래퍼 객체를 아예 모르는 상태에선 종종 보이는 '이상한' 현상에 당황할 수도... 아래를 보자:

```js
var s = "abc";
typeof s; // "string"
s.substring(1, 2); // 'b'
```

`s`는 분명 문자열 원시 타입이다. 그런데 어떻게 메서드를 갖고 있을까?

```js
public class LogicTest {
  public static void main(String... args) {
    String s = "abc"; // new String("abc");
    s.substring(1, 2);
  }
}
```

위는 자바에서 문자열을 다루는 코드다. 자바의 원시 타입에는 문자열이 존재하지 않는다. 자바의 문자열은 원칙적으로 `new` 키워드를 사용해야 하는 객체 타입이다. 다만 우리는 이 과정을 컴파일러에게 맡기고 `new`를 생략할 수 있을 뿐이다. 다시 말해 자바의 문자열은 생성될 때부터 객체 타입이며 메서드를 소유할 수 있다.

반면, 자바스크립트의 문자열은 원시 타입으로 존재한다. 우리가 문자열의 프로퍼티에 접근하려고 할 때(가령 length 같은) 자바스크립트는 `new String`을 호출한 것처럼 문자열 값을 객체로 변환한다. 이 객체를 래퍼 객체라고 한다. 래퍼 객체는 프로퍼티를 참조할 때 생성되며 프로퍼티 참조가 끝나면 사라진다.

아래 현상은 위와 같은 특성으로 발생한다:

```js
var s = "test-test";
s.len = 4; // new String(s).len = 4
console.debug(s.len); // 'undefined'
```

두 번째 라인에서 `s.len`의 값으로 4를 할당했다. 하지만 다시 꺼내었을 때 출력되는건 'undefined'다. 두 번째 라인의 s는 `new String(s)`로 바뀌었고 3라인은 2라인과는 또 다른 객체를 참조하고 있기 때문이다. 애초에 할당된 적이 없는 `s.len`을 참조하고 있는 셈이 된다.

```js
var n = 123;
typeof n; // "number"
n.toExponential();
n.prop = 1;
console.debug(n.prop); // 'undefined'

var b = true;
typeof b; // "boolean"
b.toString()
b.name = 'abc';
console.debug(b.name); // 'undefined'
```

숫자와 불리언 타입도 문자열과 같은 방식으로 작동한다. 변수의 프로퍼티에 접근할 때 래퍼 객체가 '임시로' 생성된다. 프로퍼티의 값을 할당하는 것은 '임시로' 생성된 래퍼 객체에서 수행되며 지속되지 않는다. 때문에 원시 타입의 프로퍼티(실제로는 래퍼 객체의 프로퍼티)가 마치 읽기 전용 값처럼 존재하는 것이다.

```js
var n  = 1;
var N = new Number(n);

console.debug(n == N); // true
console.debug(n === N); // false
```
원시 타입과 래퍼 객체는 거의 동등한 값처럼 다뤄진다. 동등 연산자`==`로는 이 둘을 구분할 수 없지만 엄격한 동등 연산자`===`로 구분할 수 있다.
