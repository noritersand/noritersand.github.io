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
  - javascript object literal notation
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Object_literal_notation_vs_JSON](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Object_literal_notation_vs_JSON)
- [http://stackoverflow.com/questions/2904131/what-is-the-difference-between-json-and-object-literal-notation](http://stackoverflow.com/questions/2904131/what-is-the-difference-between-json-and-object-literal-notation)


세 줄 요약:

- JSON은 프로퍼티의 이름을 반드시 쌍따옴표("")로 감싸야한다.
- JSON의 값은 문자열, 숫자, 배열, 불리언, null, 또다른 JSON만이 가능하다. 반면 객체 리터럴은 함수, 객체 등도 사용할 수 있다.
- 자바스크립트 객체 리터럴 표기법은 JSON의 빡빡한 규칙을 대부분 허용한다.
