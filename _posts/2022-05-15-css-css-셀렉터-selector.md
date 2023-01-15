---
layout: post
date: 2022-05-15 16:10:09 +0900
title: '[CSS] CSS 셀렉터'
categories:
  - css
tags:
  - css
  - css-selector
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [CSS selectors](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors)
- [https://www.w3schools.com/cssref/css_selectors.asp](https://www.w3schools.com/cssref/css_selectors.asp)


## 개요

CSS 셀렉터를 정리한 글.

```css
태그명 { 스타일코드 }
#아이디 { 스타일코드 }
.클래스명 { 스타일코드 }
```


## Basic selectors

### Universal selector

지정한 요소 기준 하위의 모든 태그를 의미함. 지정한 요소가 없다면 루트가 기준이 되므로 세상 모든 태그가 선택된다.

```
*
```

### Type selector

일치하는 태그를 찾는다.

```
TAG_NAME
```

### Class selector

일치하는 클래스가 적용된 태그를 찾는다. ~참 쉽죠?~

```
.CLASS_NAME
```

### ID selector

일치하는 `id` 속성이 있는 태그를 찾는다.

```
#ID
```

### Attribute selector

```
[attr] [attr=value] [attr~=value] [attr|=value] [attr^=value] [attr$=value] [attr*=value]
```

⚠️ `value`는 따옴표로 감싸야 함.

- `[attr]`: 지정한 속성이 정의되어 있는 요소 선택(값이 없더라도)
- `=`: 속성과 값이 완전히 일치하는 요소를 선택
- `~=`: TODO word selector
- `|=`: TODO prefix selector
- `^=`: 속성의 값이 지정된 값으로 시작하는 요소를 선택
- `$=`: 속성의 값이 지정된 값으로 끝나는 요소를 선택
- `*=`: 속성의 값이 지정된 값을 포함하는 요소를 선택


## Grouping selectors

### Selector list

```
A, B
```


## Combinators

### Descendant combinator

```
A B
```

### Child combinator

```
A > B
```

### General sibling combinator

```
A ~ B
```

### Adjacent sibling combinator

```
A + B
```

### Column combinator

```
A || B
```


## Pseudo classes

### :active

a:active 	Selects the active link

### :any-link

TODO

### :autofill

TODO

### :checked

input:checked 	Selects every checked <input> element

### :default

input:default 	Selects the default <input> element

### :defined

TODO

### :dir()

TODO

### :disabled

input:disabled 	Selects every disabled <input> element

### :empty

p:empty 	Selects every <p> element that has no children (including text nodes)

### :enabled

input:enabled 	Selects every enabled <input> element

### :first-child

p:first-child 	Selects every <p> element that is the first child of its parent

### :first-of-type

p:first-of-type 	Selects every <p> element that is the first <p> element of its parent

### :first

TODO

### :focus-visible

TODO

### :focus-within

TODO

### :focus

input:focus 	Selects the input element which has focus

### :fullscreen

:fullscreen 	Selects the element that is in full-screen mode

### :has()

TODO

### :host-context()

TODO

### :host()

TODO

### :host

TODO

### :hover

a:hover 	Selects links on mouse over

### :in-range

input:in-range 	Selects input elements with a value within a specified range

### :indeterminate

input:indeterminate 	Selects input elements that are in an indeterminate state

### :invalid

input:invalid 	Selects all input elements with an invalid value

### :is() (:matches(), :any())

TODO

### :lang()

TODO

### :lang(language)

p:lang(it) 	Selects every <p> element with a lang attribute equal to "it" (Italian)

### :last-child

p:last-child 	Selects every <p> element that is the last child of its parent

### :last-of-type

p:last-of-type 	Selects every <p> element that is the last <p> element of its parent

### :left

TODO

### :link

a:link 	Selects all unvisited links

### :not(selector)

`selector`가 아닌 요소만 선택한다.

- `:not(p)`: `<p>`가 아닌 태그를 선택
- `input:not(:first-child)`: `<input>` 중 첫 번째 자식이 아닌 것만 선택

### :nth-child(n)

요소의 부모 기준 n번째 자식 요소를 모두 선택한다. **⚠️ zero-based numbering 아님. 1부터 시작**

- `td:nth-child(1)`: (일반적으로 `<tr>` 아래에 있으므로) 부모 태그인 `<tr>`의 첫 번째 자식에 해당하는 `<td>`만 모두 선택

### :nth-last-child(n)

`:nth-child(n)`와 같으나 순서를 역으로 적용한다.

### :nth-of-type(n)

요소의 형제들 기준으로 n번째 요소를 모두 선택한다.

- `p:nth-of-type(2)`: 같은 레벨에 있는 `<p>` 중 두 번째에 해당하는 요소만 선택

### :nth-last-of-type(n)

`:nth-of-type(n)`와 같으나 순서를 역으로 적용한다.

### :only-child

p:only-child 	Selects every <p> element that is the only child of its parent

### :only-of-type

p:only-of-type 	Selects every <p> element that is the only <p> element of its parent

### :optional

input:optional 	Selects input elements with no "required" attribute

### :out-of-range

input:out-of-range 	Selects input elements with a value outside a specified range

### :paused

TODO

### :picture-in-picture

TODO

### :placeholder-shown

TODO

### :playing

TODO

### :read-only

input:read-only 	Selects input elements with the "readonly" attribute specified

### :read-write

input:read-write 	Selects input elements with the "readonly" attribute NOT specified

### :required

input:required 	Selects input elements with the "required" attribute specified

### :right

TODO

### :root

:root 	Selects the document's root element

### :scope

TODO

### :target

- `#news:target`: Selects the current active #news element (clicked on a URL containing that anchor name)

### :user-invalid (:-moz-ui-invalid)

TODO

### :user-valid (:-moz-ui-valid)

TODO

### :valid

input:valid 	Selects all input elements with a valid value

### :visited

a:visited 	Selects all visited links

### :where()

TODO


## Pseudo elements

### ::after

Insert something after the content of each <p> element

- `p::after`

### ::backdrop

TODO

### ::before

Insert something before the content of each <p> element

- `p::before`

### ::cue

TODO

### ::cue-region

TODO

### ::file-selector-button

TODO

### ::first-letter

Selects the first letter of every <p> element

- `p::first-letter`

### ::first-line

Selects the first line of every <p> element

- `p::first-line`

### ::marker

Selects the markers of list items

- `::marker`

### ::part()

TODO

### ::placeholder

Selects input elements with the "placeholder" attribute specified

- `input::placeholder`

### ::selection

Selects the portion of an element that is selected by a user

- `::selection`

### ::slotted()

TODO

### ::target-text

TODO
