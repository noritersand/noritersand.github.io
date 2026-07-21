---
layout: post
date: 2023-12-29 15:42:21 +0900
title: '[CSS] CSS 함수와 @-규칙'
categories:
  - css
tags:
  - css
  - language
  - function
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [CSS: Cascading Style Sheets \| MDN](https://developer.mozilla.org/en-US/docs/Web/CSS)
- [CSS value functions \| MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Functions)


## 개요

CSS의 함수와 At-rules 모음. 


## 함수

말 그대로 CSS에서 사용하는 함수다. 공식 명칭은 *CSS value functions*

### calc()

```html
<!-- 상속받은 너비 전체에서 200px 뺀 나머지를 width 값으로 설정 -->
<div style="width: calc(100% - 200px);"><div>
```

**TODO**

### attr()

특정 엘리먼트의 속성값을 가져오는 함수

**TODO**

### light-dark()

다크 모드 구현할 때 유용한 새 함수

**TODO**

### var()

[https://developer.mozilla.org/en-US/docs/Web/CSS/var](https://developer.mozilla.org/en-US/docs/Web/CSS/var)

사용자가 정의한 커스텀 프로퍼티의 값을 반환하는 함수.

커스텀 프로퍼티란?

- 커스텀 프로퍼티는 'CSS 변수'라고도 함
- 사용자가 정의하는 CSS 프로퍼티. 이름은 반드시 `--`로 시작해야 한다.

스크립트를 통한 동적 제어도 가능하다:

```html
<style>
:root {
  --main-color: red;
}

div {
  background-color: var(--main-color);
}
</style>

<div id="myDiv">This is a div</div>

<script>
document.addEventListener("DOMContentLoaded", function () {
  const myDiv = document.getElementById("myDiv");

  // 커스텀 프로퍼티 값을 변경
  myDiv.style.setProperty("--main-color", "blue");
});
</script>
```


## [At-rules](https://developer.mozilla.org/en-US/docs/Web/CSS/At-rule)

CSS가 어떻게 작동해야하는지를 정의하는 지시어.

```
@IDENTIFIER (RULE);
```

```css
/* Example: tells browser to use UTF-8 character set */
@charset "utf-8";
```

### @import

**TODO**

### @supports

[https://developer.mozilla.org/en-US/docs/Web/CSS/@supports](https://developer.mozilla.org/en-US/docs/Web/CSS/@supports)

브라우저가 어떤 프로퍼티(+ 프로퍼티 값까지)을 지원하거나 지원하지 않을 때 적용할 스타일을 정의한는 방법이다.

```
@supports (<supports-condition>) {
  /* supports-condition이 true면 적용할 스타일 */
}

@supports (<supports-condition>) and (<supports-condition>) {
  /* supports-condition이 모두 true면 적용할 스타일 */
}

@supports (<supports-condition>) or (<supports-condition>) {
  /* supports-condition 중 하나가 true면 적용할 스타일 */
}

@supports not (<supports-condition>) {
  /* supports-condition이 false면 적용할 스타일 */
}
```

요런식으로 쓴다(코드 출처는 [니콜라스 유튜브 채널](https://www.youtube.com/watch?v=HZIcTZABMuc)과 MDN):

```css
/* 브라우저가 text-align:center를 지원하면 */
@supports (text-align:center) {
  /* 이 스타일을 적용함 */
  body {
    text-align:center;
  }
}

/* 브라우저가 display:grid를 지원하면 */
@supports (display:grid) {
  /* 이 스타일 적 */
  header {
    display: grid;
  }
}

/* 브라우저가 border-radius:10px을 지원하지 않으면 */
@supports not (border-radius:10px) {
  .old-browser-alert {
    display:block;
  }
}

@supports selector(:first-child) {
  li:first-child {
    /* ... */
  }
}

@supports (transform-origin: 5% 5%) {
}

@supports not (transform-origin: 10em 10em 10em) {
}

@supports not (not (transform-origin: 2px)) {
}

@supports (display: grid) and (not (display: inline-grid)) {
}
```

2022년에 정의된 표준이라 현재(2023-03-26)는 파이어폭스만 모두 지원하고, 나머지 브라우저는 일부 기능만 지원한다. [https://caniuse.com/?search=%40supports](https://caniuse.com/?search=%40supports)

### @property

[https://developer.mozilla.org/en-US/docs/Web/CSS/@property](https://developer.mozilla.org/en-US/docs/Web/CSS/@property)

**TODO**

2023년에 정의된 표준이며 현재(2023-03-26) 크로미움 계열만 지원함.

### @font-face

**TODO** https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face

컴퓨터에 설치하지 않은 폰트를 사용할 수 있게 해주는 규칙. 

```css
@font-face {
    font-family: 'Noto Sans KR';
    font-style: normal;
    font-weight: normal;
    font-display: auto;
    url("/css/webfonts/NotoSansKR-Regular.woff2") format("woff2"),
    url("/css/webfonts/NotoSansKR-Regular.woff") format("woff");
}
```

### @media

반응형 웹의 핵심으로 *미디어 쿼리*라 부른다. 미디어 쿼리는 장치의 미디어 유형(출력인지 모니터 화면인지)이나 해상도, 방향, 종횡비, 브라우저 뷰포트 너비나 높이 같은 사용자 설정이나 특성에 따라 CSS 스타일을 분리하여 적용할 때 사용한다.

미디어 쿼리는 미디어 유형이나 미디어 기능을 특정하는 방식으로 작성한다. 

ℹ️ 우선순위가 같은 스타일은 나중에 선언된 것이 최종 적용된다. 모바일 환경을 고려한 반응형 사이트라면, 미디어 쿼리를 다른 코드보다 아래에, 그리고 더 넓은 뷰포트일수록 아래에 위치하도록 하는 게 좋다.

#### 미디어 유형 media types

미디어 유형은 `all`, `print`, `screen` 중에 하나이기 때문에 출력 대상을 제외하면 거의 쓸 일이 없을 것이다. 그래도 적용 방법을 대충 알아보면:

아래는 프린트 모드일 때 적용되는 스타일이다:

```css
@media print {
  /*...*/
}
```

아래처럼 쓰면 화면 출력과 프린트 모드 둘 다 적용된다:

```css
@media screen, print {
  /*...*/
}
```

#### 미디어 기능 media features

사용 가능한 미디어 기능은 다음과 같다:

- `any-hover`
- `any-pointer`
- `aspect-ratio`
- `color`
- `color-gamut`
- `color-index`
- `display-mode`
- `dynamic-range`
- `forced-colors`
- `grid`
- `height`
- `hover`
- `inverted-colors`
- `monochrome`
- `orientation`
- `overflow-block`
- `overflow-inline`
- `pointer`
- `prefers-color-scheme`
- `prefers-contrast`
- `prefers-reduced-motion`
- `prefers-reduced-transparency`
- `resolution`
- `scripting`
- `update`
- `video-dynamic-range`
- `width`

미디어 기능은 키워드 단독 혹은 키워드와 콜론`:`으로 이어지는 갑으로 특정한다. 값은 미디어 기능에 따라 달라진다.

예를 들어 다음은 엘리먼트가 마우스 호버 상태일 때를 의미한다:

```css
@media (hover: hover) {
  /*...*/
}
```

미디어 유형과 같이, 출력 화면이면서 뷰포트 방향이 세로일 때를 특정하려면 다음처럼 작성한다.

```css
@media print and (orientation: portrait) {
  /*...*/
}
```

비교 연산자나 `min-`, `max-` 접두어를 통해 적용할 범위를 지정할 수 있다:

```css
/*뷰포트 너비 0px부터 1250px까지 적용*/
@media (max-width: 1250px) {
  /*...*/
}

/*뷰포트 너비가 30em 이상이고 50em 이하일 때 적용*/
@media (min-width: 30em) and (max-width: 50em) {
  /*...*/
}
```

최신 브라우저에서는 [Media Queries Level 4](https://www.w3.org/TR/mediaqueries-4/)에서 고안된 최신 문법을 적용할 수 있는데, 이쪽이 가독성이 훨씬 좋다: 

```css
/*뷰포트 너비 0부터 1239px 까지 적용(min-width: 1239px과 같음)*/
@media (1239px <= width) { /*...*/ }

/*뷰포트 너비가 1250px 이하일 때 적용(max-width: 1250px과 같음)*/
@media (width <= 1250px) {
  /*...*/
}

/*뷰포트 너비가 30em 이상이고 50em 이하일 때 적용*/
@media (30em <= width <= 50em) {
  /*...*/
}

/*뷰포트 너비가 30em을 초과하고 50em 미만일 때 적용*/
@media (30em < width < 50em) {
  /*...*/
}
```

아래는 키워드 단독으로 쓰이는 경우이며, 컬러 화면이 있는 모든 장치에서 적용된다:

```css
/**/
@media (color) {
  /*...*/
}
```
