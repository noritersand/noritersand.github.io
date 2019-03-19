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

## 기본 객체

### [Expression Basic Objects](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-a-expression-basic-objects)

표현식 기본 객체(?).

- `#ctx`: the context object.
- `#vars`: the context variables.
- `#locale`: the context locale.
- `#request`: (only in Web Contexts) the HttpServletRequest object.
- `#response`: (only in Web Contexts) the HttpServletResponse object.
- `#session`: (only in Web Contexts) the HttpSession object.
- `#servletContext`: (only in Web Contexts) the ServletContext object.

아래처럼 사용한다:

```html
Established locale country: <span th:text="${#locale.country}">US</span>.
```

### [Expression Utility Objects](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-b-expression-utility-objects)

표현식 유틸리티 객체(?).

Besides these basic objects, Thymeleaf will offer us a set of utility objects that will help us perform common tasks in our expressions.

- `#execInfo`: information about the template being processed.
- `#messages`: methods for obtaining externalized messages inside variables expressions, in the same way as they would be obtained using #{…} syntax.
- `#uris`: methods for escaping parts of URLs/URIs
- `#conversions`: methods for executing the configured conversion service (if any).
- `#dates`: methods for java.util.Date objects: formatting, component extraction, etc.
- `#calendars`: analogous to #dates, but for java.util.Calendar objects.
- `#numbers`: methods for formatting numeric objects.
- `#strings`: methods for String objects: contains, startsWith, prepending/appending, etc.
- `#objects`: methods for objects in general.
- `#bools`: methods for boolean evaluation.
- `#arrays`: methods for arrays.
- `#lists`: methods for lists.
- `#sets`: methods for sets.
- `#maps`: methods for maps.
- `#aggregates`: methods for creating aggregates on arrays or collections.
- `#ids`: methods for dealing with id attributes that might be repeated (for example, as a result of an iteration).

## 표현식

### Variable Expressions: `${...}`

변수 표현식. 객체 표현식으로 봐도 된다.

```html
<p>Today is: <span th:text="${today}">13 february 2011</span>.</p>
```

위 표현식은 아래와 같다:

```java
ctx.getVariable("today");
```

그리고 이 표현식은:

```html
<p>Hello <span th:utext="${session.user.name}"></span>.</p>
```

아래와 같다:

```java
((User) ctx.getVariable("session").get("user")).getName();
```

### Selection Variable Expressions: `*{...}`

선택 변수 표현식. 이건 객체의 프로퍼티 표현식이다.

```html
<div th:object="${session.user}">
  <p>Name: <span th:text="*{firstName}">Sebastian</span>.</p>
  <p>Surname: <span th:text="*{lastName}">Pepper</span>.</p>
  <p>Nationality: <span th:text="*{nationality}">Saturn</span>.</p>
</div>
```

위는 아래와 같다:

```html
<div>
  <p>Name: <span th:text="${session.user.firstName}">Sebastian</span>.</p>
  <p>Surname: <span th:text="${session.user.lastName}">Pepper</span>.</p>
  <p>Nationality: <span th:text="${session.user.nationality}">Saturn</span>.</p>
</div>
```

### Message Expressions: `#{...}`

메시지 표현식.

```html
<p th:utext="#{home.welcome(${user.name})}">
  Welcome to our grocery store, Sebastian Pepper!
</p>
```

### Link URL Expressions: `@{...}`

링크 표현식.

```html
<!-- Will produce 'http://localhost:8080/gtvg/order/details?orderId=3' (plus rewriting) -->
<a href="details.html"
   th:href="@{http://localhost:8080/gtvg/order/details(orderId=${o.id})}">view</a>

<!-- Will produce '/gtvg/order/details?orderId=3' (plus rewriting) -->
<a href="details.html" th:href="@{/order/details(orderId=${o.id})}">view</a>

<!-- Will produce '/gtvg/order/3/details' (plus rewriting) -->
<a href="details.html" th:href="@{/order/{orderId}/details(orderId=${o.id})}">view</a>
```

### Fragment Expressions: `~{...}`

프래그먼트 표현식.

```html
<div th:insert="~{commons :: main}">...</div>
```

```html
<div th:with="frag=~{footer :: #main/text()}">
  <p th:insert="${frag}">
</div>
```

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

### Arithmetic operations:

- Binary operators: `+`, `-`, `*`, `/`, `%`
- Minus sign (unary operator): `-`

### Boolean operations:

- Binary operators: `and`, `or`
- Boolean negation (unary operator): `!`, `not`

### Comparisons and equality:

- Comparators: `>`, `<`, `>=`, `<=` (`gt`, `lt`, `ge`, `le`)
- Equality operators: `==`, `!=` (`eq`, `ne`)

### Conditional operators:

- If-then: (if) ? (then)
- If-then-else: (if) ? (then) : (else)
- Default: (value) ?: (defaultvalue)

### Special tokens:

- No-Operation: `_`

### Literal substitutions `+` `|...|`

문자열 연산.

```html
<!-- 덧셈 연산자 사용 -->
<span th:text="'Welcome to our application, ' + ${user.name} + '!'">

<!-- 파이프로 감싸기 -->
<span th:text="|Welcome to our application, ${user.name}!|">

<!-- 혼합 사용 -->
<span th:text="${onevar} + ' ' + |${twovar}, ${threevar}|">
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

## 리터럴

### Text literals

Text literals are just character strings specified between single quotes. They can include any character, but you should escape any single quotes inside them using `'`.

```html
<p>
  Now you are looking at a <span th:text="'working web application'">template file</span>.
</p>
```

### Number literals

Numeric literals are just that: numbers.

```html
<p>The year is <span th:text="2013">1492</span>.</p>
<p>In two years, it will be <span th:text="2013 + 2">1494</span>.</p>
```

### Boolean literals

The boolean literals are true and false. For example:

```html
<div th:if="${user.isAdmin()} == false"> ...
```

In this example, the `==` false is written outside the braces, and so it is Thymeleaf that takes care of it. If it were written inside the braces, it would be the responsibility of the OGNL/SpringEL engines:

```html
<div th:if="${user.isAdmin() == false}"> ...
```

### The null literal

The null literal can be also used:

```html
<div th:if="${variable.something} == null"> ...
```

### Literal tokens

Numeric, boolean and null literals are in fact a particular case of literal tokens.

These tokens allow a little bit of simplification in Standard Expressions. They work exactly the same as text literals `'...'`, but they only allow letters (A-Z and a-z), numbers (0-9), brackets `[` and `]`, dots `.`, hyphens `-` and underscores `_`. So no whitespaces, no commas, etc.

The nice part? Tokens don’t need any quotes surrounding them. So we can do this:

```html
<div th:class="content">...</div>
```

instead of:

```html
<div th:class="'content'">...</div>
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
