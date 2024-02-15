---
layout: post
date: 2019-01-24 15:04:00 +0900
title: '[Java] 타임리프 기본'
categories:
  - java
tags:
  - java
  - thymeleaf
  - template-engine
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://www.thymeleaf.org/](https://www.thymeleaf.org/)
- [https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html)
- [https://docs.spring.io/spring/docs/4.2.x/spring-framework-reference/html/expressions.html](https://docs.spring.io/spring/docs/4.2.x/spring-framework-reference/html/expressions.html)


## 개요

자바기반 템플릿 엔진인 타임리프 사용법 정리.


## HTML 템플릿

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title></title>
</head>
<body>
    <div class="content">
    </div>
</body>
</html>
```


## 코멘트

### 라인 코멘트

```html
<!--/* This code will be removed at Thymeleaf parsing time! */-->
```

주의: `<!--`, `-->` 이것들과 `/*`, `*/` 이것들 사이에 공백이 있으면 안됨.

### 블록 코멘트

```html
<!--/*-->
  <div>
     you can see me only before Thymeleaf processes me!
  </div>
<!--*/-->
```


## 기본 객체

### [Expression Basic Objects](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#appendix-a-expression-basic-objects)

표현식 기본 객체(?).

- `#ctx`: the context object.
- `#vars`: the context variables.
- `#locale`: the context locale.
- `#request`: (only in Web Contexts) the HttpServletRequest object. `#httpServletRequest`의 단축형.
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

```html
<div class="inner" th:if="*{#lists.size(unitList)} == 1">
```


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

선택 변수 표현식. 이것은 객체의 프로퍼티 표현식이다.

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

아래처럼 변수를 키로 사용 가능:

```html
<p th:utext="#{${welcomeMsgKey}(${session.user.name})}">
  Welcome to our grocery store, Sebastian Pepper!
</p>
```

### Link URL Expressions: `@{...}`

링크 표현식.

Because of their importance, URLs are first-class citizens in web application templates, and the Thymeleaf Standard Dialect has a special syntax for them, the @ syntax: @{...}

There are different types of URLs:

- Absolute URLs: http://www.thymeleaf.org
- Relative URLs, which can be:
  - Page-relative: user/login.html
  - Context-relative: /itemdetails?id=3 (context name in server will be added automatically)
  - Server-relative: ~/billing/processInvoice (allows calling URLs in another context (= application) in the same server.
  - Protocol-relative URLs: //code.jquery.com/jquery-2.0.3.min.js

The real processing of these expressions and their conversion to the URLs that will be output is done by implementations of the org.thymeleaf.linkbuilder.ILinkBuilder interface that are registered into the ITemplateEngine object being used.

By default, a single implementation of this interface is registered of the class org.thymeleaf.linkbuilder.StandardLinkBuilder, which is enough for both offline (non-web) and also web scenarios based on the Servlet API. Other scenarios (like integration with non-ServletAPI web frameworks) might need specific implementations of the link builder interface.

Let's use this new syntax. Meet the th:href attribute:

```html
<!-- Will produce 'http://localhost:8080/gtvg/order/details?orderId=3' (plus rewriting) -->
<a href="details.html"
   th:href="@{http://localhost:8080/gtvg/order/details(orderId=${o.id})}">view</a>

<!-- Will produce '/gtvg/order/details?orderId=3' (plus rewriting) -->
<a href="details.html" th:href="@{/order/details(orderId=${o.id})}">view</a>

<!-- Will produce '/gtvg/order/3/details' (plus rewriting) -->
<a href="details.html" th:href="@{/order/{orderId}/details(orderId=${o.id})}">view</a>
```

Some things to note here:

- th:href is a modifier attribute: once processed, it will compute the link URL to be used and set that value to the href attribute of the `<a>` tag.
- We are allowed to use expressions for URL parameters (as you can see in `orderId=${o.id})`. The required URL-parameter-encoding operations will also be automatically performed.
- If several parameters are needed, these will be separated by commas: `@{/order/process(execId=${execId},execType='FAST')}`
- Variable templates are also allowed in URL paths: `@{/order/{orderId}/details(orderId=${orderId})}`
- Relative URLs starting with / (eg: /order/details) will be automatically prefixed by the application context name.
- If cookies are not enabled or this is not yet known, a ";jsessionid=..." suffix might be added to relative URLs so that the session is preserved. This is called URL Rewriting and Thymeleaf allows you to plug in your own rewriting filters by using the `response.encodeURL(...)` mechanism from the Servlet API for every URL.
- The th:href attribute allows us to (optionally) have a working static href attribute in our template, so that our template links remained navigable by a browser when opened directly for prototyping purposes.

As was the case with the message syntax `#{...}`, URL bases can also be the result of evaluating another expression:

```html
<a th:href="@{${url}(orderId=${o.id})}">view</a>
<a th:href="@{'/details/'+${user.login}(orderId=${o.id})}">view</a>
```

#### Server root relative URLs

An additional syntax can be used to create server-root-relative (instead of context-root-relative) URLs in order to link to different contexts in the same server. These URLs will be specified like `@{~/path/to/something}`


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

### Preprocessing Expressions `__${expressions}__`

전처리 표현식

In addition to all these features for expression processing, Thymeleaf has the feature of preprocessing expressions.

Preprocessing is an execution of the expressions done before the normal one that allows for modification of the expression that will eventually be executed.

Preprocessed expressions are exactly like normal ones, but appear surrounded by a double underscore symbol (like `__${expression}__`).

Let's imagine we have an i18n Messages_fr.properties entry containing an OGNL expression calling a language-specific static method, like:

```java
article.text=@myapp.translator.Translator@translateToFrench({0})
```

…and a Messages_es.properties equivalent:

```java
article.text=@myapp.translator.Translator@translateToSpanish({0})
```
We can create a fragment of markup that evaluates one expression or the other depending on the locale. For this, we will first select the expression (by preprocessing) and then let Thymeleaf execute it:

```html
<p th:text="${__#{article.text('textVar')}__}">Some text here...</p>
```

Note that the preprocessing step for a French locale will be creating the following equivalent:

```html
<p th:text="${@myapp.translator.Translator@translateToFrench(textVar)}">Some text here...</p>
```

The preprocessing String `__` can be escaped in attributes using `\_\_`.


## 연산자

### Arithmetic operations:

- Binary operators: `+`, `-`, `*`, `/`, `%`
- Minus sign (unary operator): `-`

### Boolean operations:

- Binary operators: `and`, `or`
- Boolean negation (unary operator): `!`, `not`

### Comparisons and equality:

- Comparators: `<`, `>`, `<=`, `>=` (`lt`, `gt`, `le`, `ge`)
- Equality operators: `==`, `!=` (`eq`, `ne`)

### Conditional operators:

- If-then: (if) `?` (then)
- If-then-else: (if) `?` (then) `:` (else)
- Default: (value) `?:` (defaultvalue)

`?:` 연산자는 Elvis operator라고도 하는데, 일종의 단축 표현이다:

```js
x = f() ? f() : g()
```

위는 아래와 같다:

```js
x = f() ?: g()
```

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

### Safe navigation operator

SPEL의 고것임.

```html
<td th:text="${user?.address?.city}"></td>
```

- user가 null이면 address를 찾지 않는다.
- address가 null이면 city를 찾지 않는다.

이 연산자는 매우 유용한데, 예를 들어 아래 코드에서:

```html
<ul>
  <li th:each="image : ${pojo?.imageList}">
    <p>[[${image}]]</p>
  </li>
</ul>
```

`?.` 연산자 대신 접근 연산자 `.`를 사용하면 `imageList`가 null일 때 에러가 발생한다.


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

The nice part? Tokens don't need any quotes surrounding them. So we can do this:

```html
<div th:class="content">...</div>
```

instead of:

```html
<div th:class="'content'">...</div>
```

### Thymeleaf prototype-only comment blocks

Thymeleaf allows the definition of special comment blocks marked to be comments when the template is open statically (i.e. as a prototype), but considered normal markup by Thymeleaf when executing the template.

```html
<span>hello!</span>
<!--/*/
  <div th:text="${...}">
    ...
  </div>
/*/-->
<span>goodbye!</span>
```

Thymeleaf's parsing system will simply remove the <!--/*/ and /*/--> markers, but not its contents, which will be left therefore uncommented. So when executing the template, Thymeleaf will actually see this:

```html
<span>hello!</span>

  <div th:text="${...}">
    ...
  </div>

<span>goodbye!</span>
```

As with parser-level comment blocks, this feature is dialect-independent.

### Synthetic th:block tag

표현식 작성만을 위한 태그다. `th:block` 태그는 파싱할 때 코멘트로 처리된다. (열고 닫는 태그만 코멘트 처리되며 내부의 코드엔 영향이 없다.)

Thymeleaf's only element processor (not an attribute) included in the Standard Dialects is th:block.

th:block is a mere attribute container that allows template developers to specify whichever attributes they want. Thymeleaf will execute these attributes and then simply make the block, but not its contents, disappear.

So it could be useful, for example, when creating iterated tables that require more than one <tr> for each element:

```html
<table>
  <th:block th:each="user : ${users}">
    <tr>
        <td th:text="${user.login}">...</td>
        <td th:text="${user.name}">...</td>
    </tr>
    <tr>
        <td colspan="2" th:text="${user.address}">...</td>
    </tr>
  </th:block>
</table>
```

And especially useful when used in combination with prototype-only comment blocks:

```html
<table>
    <!--/*/ <th:block th:each="user : ${users}"> /*/-->
    <tr>
        <td th:text="${user.login}">...</td>
        <td th:text="${user.name}">...</td>
    </tr>
    <tr>
        <td colspan="2" th:text="${user.address}">...</td>
    </tr>
    <!--/*/ </th:block> /*/-->
</table>
```

Note how this solution allows templates to be valid HTML (no need to add forbidden <div> blocks inside <table>), and still works OK when open statically in browsers as prototypes!


## 제어문

### 분기

#### if

```html
<a href="comments.html"
   th:href="@{/product/comments(prodId=${prod.id})}"
   th:if="${not #lists.isEmpty(prod.comments)}">view</a>
```

Note that the th:if attribute will not only evaluate boolean conditions. Its capabilities go a little beyond that, and it will evaluate the specified expression as true following these rules:

- If value is not null:
  - If value is a boolean and is true.
  - If value is a number and is non-zero
  - If value is a character and is non-zero
  - If value is a String and is not “false”, “off” or “no”
  - If value is not a boolean, a number, a character or a String.
- (If value is null, th:if will evaluate to false).

#### unless

`th:if`의 역.

```html
<a href="comments.html"
   th:href="@{/comments(prodId=${prod.id})}"
   th:unless="${#lists.isEmpty(prod.comments)}">view</a>
```

#### switch

```html
<div th:switch="${user.role}">
  <p th:case="'admin'">User is an administrator</p>
  <p th:case="#{roles.manager}">User is a manager</p>
  <p th:case="*">User is some other thing</p>
</div>
```

`th:case="*"`: default를 의미한다.

### 반복

```html
<table>
  <tr>
    <th>NAME</th>
    <th>PRICE</th>
    <th>IN STOCK</th>
  </tr>
  <tr th:each="prod, iterStat : ${prods}" th:class="${iterStat.odd}? 'odd'">
    <td th:text="${prod.name}">Onions</td>
    <td th:text="${prod.price}">2.41</td>
    <td th:text="${prod.inStock}? #{true} : #{false}">yes</td>
  </tr>
</table>
```

위에서 `iterStat`이란 반복문의 상태를 알 수 있는 status 변수다. `th:each`에서 status 변수가 명시되지 않으면 타임리프는 요소가 할당되는 변수의 이름 뒤에 'Stat'을 붙여서 자동으로 정의한다. 위 예시의 경우 `iterStat`이 생략되면 status 변수는 `prodStat`이 된다.

#### status 변수의 프로퍼티:

- `index`: The current iteration index, starting with 0.
- `count`: The current iteration index, starting with 1.
- `size`: The total amount of elements in the iterated variable.
- `current`: The iter variable for each iteration.
- `even/odd`: Whether the current iteration is even or odd.
- `first`: Whether the current iteration is the first one.
- `last`: Whether the current iteration is the last one.

### 분기-반복 혼합 사용

```html
<!--/*imageList의 각 요소만큼 반복하되 요소의 sizeCode가 '280'일 때만 `<li>` 태그가 생성된다.*/-->
<li th:each="imageEle : *{imageList}" th:if="${imageEle.sizeCode} == '280'">
  <img th:src="|https://my-image-host.com/${imageEle.parent}/${imageEle.file}" alt="상품이미지">
</li>
```


## 변수 설정

`th:with`

**TODO** 이거 어떻게 쓰는겨?


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


## How to access data

### contexts

- `${x}`: will return a variable x stored into the Thymeleaf context or as a request attribute.
- `${param.x}` will return a request parameter called x (which might be multivalued).
- `${session.x}` will return a session attribute called x.
- `${application.x}` will return a servlet context attribute called x.

### Request parameters

```html
<!-- single -->
<p th:text="${param.q}">Test</p>

<!-- multivalue -->
<p th:text="${param.q[0] + ' ' + param.q[1]}" th:unless="${param.q == null}">Test</p>
```

```html
<p th:text="${#request.getParameter('q')}" th:unless="${#request.getParameter('q') == null}">Test</p>
```

### Request attributes

```html
<tr th:each="message : ${messages}">
  <td th:text="${message.id}">1</td>
  <td><a href="#" th:text="${message.title}">Title ...</a></td>
  <td th:text="${message.text}">Text ...</td>
</tr>
```

### Session attributes

```html
<p th:text="${session.mySessionAttribute}" th:unless="${session == null}">[...]</p>
```

```html
<p th:text="${#session.getAttribute('mySessionAttribute')}"></p>
```

### ServletContext attributes

```html
<table>
    <tr>
        <td>My context attribute</td>
        <!-- Retrieves the ServletContext attribute 'myContextAttribute' -->
        <td th:text="${#servletContext.getAttribute('myContextAttribute')}">42</td>
    </tr>
    <tr th:each="attr : ${#servletContext.getAttributeNames()}">
        <td th:text="${attr}">javax.servlet.context.tempdir</td>
        <td th:text="${#servletContext.getAttribute(attr)}">/tmp</td>
    </tr>
</table>
```

### Spring beans

Thymeleaf allows accessing beans registered at the Spring Application Context with the `@beanName` syntax, for example:

```html
<div th:text="${@urlService.getApplicationUrl()}">...</div>
```

In the above example, @urlService refers to a Spring Bean registered at your context, e.g.

```java
@Configuration
public class MyConfiguration {
    @Bean(name = "urlService")
    public UrlService urlService() {
        return () -> "domain.com/myapp";
    }
}

public interface UrlService {
    String getApplicationUrl();
}
```

This is fairly easy and useful in some scenarios.


## 그 밖에

### 금액 표시

```html
<!-- 123,456.00 -->
<td>$<span th:text="${#numbers.formatDecimal(123456, 0, 'COMMA', 2, 'POINT')}">10.00</span></td>
```

### 날짜 형식 표시

```html
<div th:text="${#dates.format(value, 'yyyy-MM-dd HH:mm:ss')}"></div>
```

### unescape 표현식에서 특정 컨텍스트 접근 불가

unescape 표현식(`th:utext`, `[(...)]`)은 특정 컨텍스트에 접근할 때 `TemplateProcessingException`이 발생한다.

```html
<span th:utext="${#request.getParameter('productNumber')}"></span>
```

```java
Caused by: org.thymeleaf.exceptions.TemplateProcessingException: Access to request parameters is forbidden in this context. Note some restrictions apply to variable access. For example, direct access to request parameters is forbidden in preprocessing and unescaped expressions, in TEXT template mode, in fragment insertion specifications and in some specific attribute processors.
    at org.thymeleaf.standard.expression.RestrictedRequestAccessUtils$RestrictedRequestWrapper.createRestrictedParameterAccessException(RestrictedRequestAccessUtils.java:87)
    at org.thymeleaf.standard.expression.RestrictedRequestAccessUtils$RestrictedRequestWrapper.getParameter(RestrictedRequestAccessUtils.java:68)
    at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
    at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
    at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
    at java.lang.reflect.Method.invoke(Method.java:498)
  ...
```

아마 파라미터 말고도 더 있을것 같은데, 타임리프의 보안정책으로 추정되며 escape 표현식(`th:text`, `[[...]]`)에선 아무 문제 없다:

```html
<span th:text="${#request.getParameter('productNumber')}"></span>
```

### th:block 태그에서 th:replace

```html
<th:block th:replace="/some-where/file-name :: grenade"></th:block>
```

### th:block 태그에서 th:fragment

```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
<th:block th:fragment="grenade">
  <!-- 중략 -->
</th:block>
```

요딴식으로 하면 fragment 쓴다고 불필요한 태그를 추가하지 않아도 된다.

### spring 환경일 때 타입(=클래스) 특정

SPEL을 쓸 수 있는 환경이라면 `T` 연산자로 타입을 특정할 수 있다.

```html
<div>[[${T(java.util.Date).class}]]</div>
<div>[[${T(Integer).MAX_VALUE eq 2147483647}]]</div>
```

위 코드의 결과:

```html
<div>class java.lang.Class</div>
<div>true</div>
```

### plain object의 null safety 처리

타입이 plain object일 때 프로퍼티의 null safety 처리는 `?.` 연산자를 쓰면 된다:

```html
<ul>
  <li th:each="image : ${pojo?.imageList}">
    <p>[[${image}]]</p>
  </li>
</ul>
```

### 컬렉션 프레임워크 타입의 empty check

#### map

map에서 특정 프로퍼티의 존재 유무는 `?.` 연산자로 확인할 수 없다. 대신 `maps` 유틸리티를 사용한다:

```html
<th:block th:if="${not #maps.isEmpty(motherShip)} and ${#maps.containsKey(motherShip, 'bomber')}">
  <p>[[${motherShip.bomber}]]</p>
</th:block>
```

- `maps.isEmpty(obj)`: obj가 null이거나 null이 아니어도 비어있으면 true
- `maps.containsKey(obj, name)`: obj에 name으로 지정된 요소가 있으면 true

#### collections

`th:if`와 `?.` 연산자는 list가 비었는지를 판단하지 못한다. 대신 `lists` 유틸리티를 사용한다:

```html
<div th:unless="${#lists.isEmpty(imageList)}">
  <img th:src="${imageList[0].location}">
</div>
```

- `lists.isEmpty(obj)`: obj가 null이거나 null이 아니어도 비어있으면 true

아래 같은 방법도 있지만 두 번 확인해야 해서 귀찮:

```html
<dt class="img" th:if="${imageList} and ${lists.size(imageList) > 0}">
  <img th:src="${imageList[0].location}">
</dt>
```
