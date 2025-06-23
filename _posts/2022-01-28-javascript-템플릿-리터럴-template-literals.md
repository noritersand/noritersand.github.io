---
layout: post
date: 2022-01-28 17:50:06 +0900
title: '[JavaScript] 템플릿 리터럴 Template literals'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - language
  - template-literals
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Template literals (Template strings) \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

#### 브라우저 호환 정보

- IE 제외 모든 브라우저에서 지원함


## 개요

ES2105의 새 문법 템플릿 리터럴의 간단 정리 글. 원래 명칭은 'template strings' 였으나 변경됨.


## 템플릿 리터럴

```
`string text ${expression} string text`
```

따옴표 대신 백틱(backticks 혹은 grave accent)``` ` ```으로 표현하는 문자열 리터럴. 플레이스 홀더(placeholders, `${}`)와 같이 쓰이며, 플레이스 홀더의 표현식을 먼저 평가하고 문자열에 삽입한 결과를 반환한다.

이렇게 쓴다:

```js
var abc = 1234;
`${abc} is dumdum`
// '1234 is dumdum' 출력
```

백틱을 표현하려면 백슬래시`\`로 이스케이핑 하면 됨:

```js
`\`` === '`' // true
```

템플릿 리터럴 내부의 줄 바꿈은 이스케이프 문자 `\n`으로 자동 치환된다:

```js
var str = `1
2
3`;

str; // "1\n2\n3"
```
