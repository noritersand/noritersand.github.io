---
layout: post
date: 2022-01-25 14:45:00 +0900
title: '[CSS] CSS 기본'
categories:
  - css
tags:
  - css
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://www.w3schools.com/cssref/css_selectors.asp](https://www.w3schools.com/cssref/css_selectors.asp)
- [\[MDN\] CSS selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors)

## 개요

CSS(Cascading Sytle Sheet) 기본 사용법 정리.

## [At-rules](https://developer.mozilla.org/en-US/docs/Web/CSS/At-rule)

At-rules은 CSS가 어떻게 작동해야하는지를 정의하는 지시어에 가깝다.

```
@IDENTIFIER (RULE);
```

```css
/* Example: tells browser to use UTF-8 character set */
@charset "utf-8";
```

## 스타일 적용하기

### inline

HTML Tag 속에 style 속성을 사용하여 직접 지정한다.

```html
<div style="color:red;">여기는 적색으로 나타난다.</div>
```

### embedded(internal)

스타일 시트의 기본적인 사용 방법으로 html의 `<head></head>` 사이에 삽입하여 `<style type="text/css"><style>` 사이에 정의한다. 또한 같은 스타일을 중복해서 지정 했을 때는 나중에 지정 한 것이 적용된다.

```
<style type="css/text" [media="값"]>....</style>
```

- `MEDIA`: Style Sheet가 적용되어야하는 매체를 지정한다. 가능한 값은 다음과 같다.
- `print`: 프린터 출력인 경우
- `screen`: 화면 출력인 경우
- `projection`: 프로젝터 출력인 경우
- `braille`: 점자출력 장치로 출력하는 경우
- `aural`: 음성 출력인 경우
- `all`: 모든 매체를 통한 출력인 경우(내정치)

```html
<style type="text/css">
  p { background: yellow; color: black; }
</style>
```

### Linked(external)

`<head> </head>` 사이에 Link Element 를 사용하여 CSS file(확장자가 .css 인 파일)을 연결 시켜서 사용하는 방식이다.

```html
<link rel="stylesheet" href="http://tistory.com/address.css" type="text/css"/>
```

### Imported

이 방식은 결과적으로 Linked style sheet와 같고 위치는 Embedded 방식과 마찬 가지로 Style block 속에 들어간다.

```
@import url("파일명");
또는
@import "파일명";
```

### 사용 방식에 따른 Style 적용의 우선 순위(Cascading order)

1. Inline style sheet
1. Embedded style sheet
1. Linked style sheet
1. Imported style sheet

## CSS selector

```css
태그명 { 스타일코드 }
#아이디 { 스타일코드 }
.클래스명 { 스타일코드 }
```

### Basic selectors

#### Universal selector

```
*
```

#### Type selector

```
TAG_NAME
```

#### Class selector

```
.CLASS_NAME
```

#### ID selector

```
#ID
```

#### Attribute selector

```
[attr] [attr=value] [attr~=value] [attr|=value] [attr^=value] [attr$=value] [attr*=value]
```

### Grouping selectors

#### Selector list

```
A, B
```

### Combinators

#### Descendant combinator

```
A B
```

#### Child combinator

```
A > B
```

#### General sibling combinator

```
A ~ B
```

#### Adjacent sibling combinator

```
A + B
```

#### Column combinator

```
A || B
```

### Pseudo

#### Pseudo classes

- `:active`: 	a:active 	Selects the active link
- `:any-link`:
- `:autofill`:
- `:checked`: 	input:checked 	Selects every checked <input> element
- `:default`: 	input:default 	Selects the default <input> element
- `:defined`:
- `:dir()`:
- `:disabled`: 	input:disabled 	Selects every disabled <input> element
- `:empty`: 	p:empty 	Selects every <p> element that has no children (including text nodes)
- `:enabled`: 	input:enabled 	Selects every enabled <input> element
- `:first-child`: 	p:first-child 	Selects every <p> element that is the first child of its parent
- `:first-of-type`: 	p:first-of-type 	Selects every <p> element that is the first <p> element of its parent
- `:first`:
- `:focus-visible`:
- `:focus-within`:
- `:focus`: 	input:focus 	Selects the input element which has focus
- `:fullscreen`: 	:fullscreen 	Selects the element that is in full-screen mode
- `:has()`:
- `:host-context()`:
- `:host()`:
- `:host`:
- `:hover`: 	a:hover 	Selects links on mouse over
- `:in-range`: 	input:in-range 	Selects input elements with a value within a specified range
- `:indeterminate`: 	input:indeterminate 	Selects input elements that are in an indeterminate state
- `:invalid`: 	input:invalid 	Selects all input elements with an invalid value
- `:is() (:matches(), :any())`:
- `:lang()`:
- `:lang(language)`: 	p:lang(it) 	Selects every <p> element with a lang attribute equal to "it" (Italian)
- `:last-child`: 	p:last-child 	Selects every <p> element that is the last child of its parent
- `:last-of-type`: 	p:last-of-type 	Selects every <p> element that is the last <p> element of its parent
- `:left`:
- `:link`: 	a:link 	Selects all unvisited links
- `:not(selector)`: 	:not(p) 	Selects every element that is not a <p> element
- `:nth-child(n)`: 	p:nth-child(2) 	Selects every <p> element that is the second child of its parent
- `:nth-last-child(n)`: 	p:nth-last-child(2) 	Selects every <p> element that is the second child of its parent, counting from the last child
- `:nth-last-of-type(n)`: 	p:nth-last-of-type(2) 	Selects every <p> element that is the second <p> element of its parent, counting from the last child
- `:nth-of-type(n)`: 	p:nth-of-type(2) 	Selects every <p> element that is the second <p> element of its parent
- `:only-child`: 	p:only-child 	Selects every <p> element that is the only child of its parent
- `:only-of-type`: 	p:only-of-type 	Selects every <p> element that is the only <p> element of its parent
- `:optional`: 	input:optional 	Selects input elements with no "required" attribute
- `:out-of-range`: 	input:out-of-range 	Selects input elements with a value outside a specified range
- `:paused`:
- `:picture-in-picture`:
- `:placeholder-shown`:
- `:playing`:
- `:read-only`: 	input:read-only 	Selects input elements with the "readonly" attribute specified
- `:read-write`: 	input:read-write 	Selects input elements with the "readonly" attribute NOT specified
- `:required`: 	input:required 	Selects input elements with the "required" attribute specified
- `:right`:
- `:root`: 	:root 	Selects the document's root element
- `:scope`:
- `:target`: 	#news:target 	Selects the current active #news element (clicked on a URL containing that anchor name)
- `:user-invalid (:-moz-ui-invalid)`:
- `:user-valid (:-moz-ui-valid)`:
- `:valid`: 	input:valid 	Selects all input elements with a valid value
- `:visited`: 	a:visited 	Selects all visited links
- `:where()`:

#### Pseudo elements

- `::after`: 	p::after 	Insert something after the content of each <p> element
- `::backdrop`:
- `::before`: 	p::before 	Insert something before the content of each <p> element
- `::cue`:
- `::cue-region`:
- `::file-selector-button`
- `::first-letter`: 	p::first-letter 	Selects the first letter of every <p> element
- `::first-line`: 	p::first-line 	Selects the first line of every <p> element
- `::marker`: 	::marker 	Selects the markers of list items
- `::part()`:
- `::placeholder`: 	input::placeholder 	Selects input elements with the "placeholder" attribute specified
- `::selection`: 	::selection 	Selects the portion of an element that is selected by a user
- `::slotted()`:
- `::target-text`:
