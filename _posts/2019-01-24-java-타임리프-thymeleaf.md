---
layout: post
date: 2019-01-24 15:04:00 +0900
title: 'Java: 타임리프 Thymeleaf'
categories:
  - java
tags:
  - java
  - thymeleaf
  - template engine
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://www.thymeleaf.org/](https://www.thymeleaf.org/)
- [https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html)

템플릿 엔진 타임리프 사용법 정리.

## 표현식

### 변수 표현식 `${...}`

Variable expressions. 객체 표현식으로 봐도 된다.

### 선택 변수 표현식 `*{...}`

Selection variable expressions. 이건 객체의 프로퍼티 표현식이다.

### 메시지 표현식 `#{...}`

Message expressions.

### 링크 표현식 `@{...}`

Link URL expressions.

### 인라인 표현식 `[[...]]` `[(...)]`

```html
<p>Hello, [[${session.user.name}]]!</p>
```

위 표현식은 아래와 같다:

```html
<p>Hello, <span th:text="${session.user.name}">Sebastian</span>!</p>
```

인라인 표현식은 괄호 두 개를 쌍으로 감싸는 형태(`[[...]]` 혹은 `[(...)]`)로 사용한다. `[[...]]` 표현식은 `th:text`와 같고(HTML-escaping), `[(...)]` 표현식은 `th:utext`와 같다(HTML-unescaping).

## 연산자

### 문자열 연산 `+` `|...|`

```html
<!-- 덧셈 연산자 사용 -->
<div th:text="'Hello!' + ${waldo.name}"><div>

<!-- 파이프로 감싸기 -->
<div th:text="|'Hello!', ${waldo.name}|"><div>
```

## HTML5 data-* 표기법

`th:href`와 같은 표기가 HTML 문법 에러로 검출되는게 싫다면 `data-th-href`로 표기할 수 있다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Good Thymes Virtual Grocery</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
    <link rel="stylesheet" type="text/css" media="all"
          href="../../css/gtvg.css" data-th-href="@{/css/gtvg.css}" />
  </head>
  <body>
    <p data-th-text="#{home.welcome}">Welcome to our grocery store!</p>
  </body>
</html>
```

### Elvis operator `?:`

삼항 연산자의 단축형.

```js
x = f() ? f() : g()
```

위는 아래와 같다:

```js
x = f() ?: g()
```
