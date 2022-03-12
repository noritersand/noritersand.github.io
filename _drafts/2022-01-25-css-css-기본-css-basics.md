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

TODO

#### Pseudo elements

TODO
