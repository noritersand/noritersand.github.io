---
layout: post
date: 2018-03-20 16:46:26 +0900
title: '[JavaScript] 클로저 closures'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - closures
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[MDN\] Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)
- [\[MDN\] JavaScript 재입문하기: Closures](https://developer.mozilla.org/ko/docs/Web/JavaScript/A_re-introduction_to_JavaScript#%ED%81%B4%EB%A1%9C%EC%A0%B8_closures)
- [https://opentutorials.org/course/743/6544](https://opentutorials.org/course/743/6544)
- [http://www.insightbook.co.kr/book/programming-insight/자바스크립트-완벽-가이드](http://www.insightbook.co.kr/book/programming-insight/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EC%99%84%EB%B2%BD-%EA%B0%80%EC%9D%B4%EB%93%9C)

## 클로저란?

> 대다수의 현대 프로그래밍 언어와 마찬가지로 자바스크립트 또한 어휘적 유효범위(Lexical scoping)을 사용한다. 이는 함수를 호출하는 시점에서의 변수 유효범위가 아니라, 함수가 정의되었을 때의 변수 유효범위를 사용하여 함수가 실행된다는 뜻이다. 이러한 어휘적 유효범위를 구현하기 위해, 자바스크립트 함수객체는 내부 상태에 함수 자체의 코드뿐만 아니라 현재 유효범위 체인에 대한 참조도 포함하고 있다. 함수 객체와 함수의 변수가 해석되는 범위(변수 바인딩의 집합)의 조합은 컴퓨터 과학 문헌에서 클로저(closure)<sup>1</sup>라고 일컫는다<sup>2<sup>.

- 1: 이는 함수의 변수가 유효범위 체인에 바인딩되어 있고, 따라서 그 함수는 함수의 변수에 '따라 닫힌다'는 뜻에서 유래한 용어다.
- 2: (역자주) 함수 내에서 선언된 변수는 보통 함수의 실행이 끝나면 같이 소멸해야 한다. 하지만, 함수의 실행이 끝나더라도 유효범위 체인 상에서 함수 내의 변수가 계속 살아있어야 하는 상황이라면, 내부 변수가 살아있으므로 그 함수는 '닫힐' 수 없다. 해당 함수가 완전히 '닫힐' 수 있는 경우는 함수 내에서 정의한 변수들을 참조하는 곳이 없어야 하는 상황이므로, 함수의 닫힘 가능 여부는 해당 함수의 실행 종료 여부가 아니라 변수 유효 여부에 달린 것이다.
- 출처: 자바스크립트 완벽가이드 (JavaScript the definitive guide 6/E), David Flanagan

간단히 말해서 함수 객체나 함수 내의 변수가 유효범위를 벗어나도 참조가 유지되는 한 소멸하지 않는 환경 혹은 그러한 함수가 클로저다.

```js
function outer() {
  var a = 'still alive';
  function inner() {
    console.log(a);
  }
  return inner;
}
var inner = outer();
inner(); // 'still alive'
```

`inner()` 함수의 참조가 종료될 때까지 `a`도 소멸하지 않는다.

## 클로저인가 클로져인가?

```
closure [klóuʒər]
```

국립국어원에선:

> 마찰음`Ʒ`과 파찰음 `dʒ, ts, dz, t∫`이 모음 앞에 올 때에는 '지, 치'가 아니라 'ㅈ, ㅊ'으로 적으므로 항상 '자, 저, 조, 주', '차, 처, 초, 추'로 표기

라고 한다. 따라서 클로저가 올바른 표기다.
