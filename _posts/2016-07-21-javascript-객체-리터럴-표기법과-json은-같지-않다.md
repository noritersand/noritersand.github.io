---
layout: post
date: 2016-07-21 18:17:10 +0900
title: '[JavaScript] 객체 리터럴 표기법과 JSON은 같지 않다'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - json
  - literal
  - javascript-object-literal-notation
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Object_literal_notation_vs_JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Object_literal_notation_vs_JSON)
- [http://stackoverflow.com/questions/2904131/what-is-the-difference-between-json-and-object-literal-notation](http://stackoverflow.com/questions/2904131/what-is-the-difference-between-json-and-object-literal-notation)

JSON<sup>JavaScript Object Notation</sup>과 JavaScript Object **Literal** Notation은 같지 않다. (줄여서 JSOLN인가?)

세 줄로 요약하면:

- JSON은 프로퍼티의 이름을 반드시 쌍따옴표("")로 감싸야한다.
- JSON의 값으로는 문자열, 숫자, 불리언, null, 배열, object(또 다른 JSON)만 가능하다.
- 반면 자바스크립트 객체 리터럴 표기법은 JSON의 빡빡한 규칙을 대부분 허용하며, 값으로 함수와 인스턴스도 할당할 수 있다.
