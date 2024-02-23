---
layout: post
date: 2022-01-25 14:45:00 +0900
title: '[CSS] CSS 기본'
categories:
  - css
tags:
  - css
  - language
  - basics
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [MDN | CSS: Cascading Style Sheets](https://developer.mozilla.org/en-US/docs/Web/CSS)


## 개요

CSS(Cascading Sytle Sheet) 기본 사용법 정리.


## 용어

```css
@charset "utf-8";

h1 { 
  font-size: 37.2px;
  border-bottom: 3px solid black;
}
```

`@charset`처럼 `@`로 시작하는 선언은 *at-rule* 이라 부른다. `h1`은 셀렉터, `font-size`, `border-bottom`은 프로퍼티다.


## 스타일 적용하기

### inline

HTML 태그의 `style` 속성을 사용하여 직접 지정한다.

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
  p {background: yellow; color: black}
</style>
```

### Linked(external)

`<head> </head>` 사이에 Link Element 를 사용하여 CSS file(확장자가 .css 인 파일)을 연결 시켜서 사용하는 방식이다.

```html
<link rel="stylesheet" href="http://tistory.com/address.css" type="text/css"/>
```

### Imported

이 방식은 결과적으로 Linked와 같다.

```
@import url("파일명");
또는
@import "파일명";
```


## 우선순위(specificity, cascading order)

우선순위(혹은 명시도, 우선권)를 결정하는 요소는 여러가지가 있다. 크게 보면 `!important`가 가장 우선 적용되고 나머지는 선언 방식이나 얼마나 구체적으로 셀렉터를 작성했느냐에 따라 달라진다. **만약 우선순위가 동일한 경우 나중에 불러온 스타일이 적용**된다.

자세한 내용은 [여기](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity)를 볼 것.

### 스타일 선언 방식에 따른 우선순위

1. Inline
1. Embedded
1. Linked
1. Imported

### !important

스타일 적용 최우선순위를 알리는 예외 규칙이다. 심지어 인라인 스타일보다 우선 적용된다.

```css
table tr td {
  text-align: left !important;
}

.foo[style*="color: red"] {
  color: firebrick !important;
}
```

그런데 이걸로 해결하는 것은 좋지 않은 습관이니 자중하라고...


## 상속

- [https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)
- [https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade)

자식 요소가 부모 요소의 CSS 프로퍼티 값을 그대로 사용하는 것을 말함.

모든 프로퍼티가 상속되는 것은 아님. (e.g., `width: 50%`)


[https://developer.mozilla.org/ko/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance](https://developer.mozilla.org/ko/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance)

### 상속 제어

[https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#controlling_inheritance](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#controlling_inheritance)

CSS는 상속을 제어하기 위한 특수 범용 프로퍼티 값(special universal property values)을 제공한다. 모든 CSS 프로퍼티에서 유효하다.

- ~~auto: 브라우저가 알아서 결정~~
- `inherit`: 부모 요소의 프로퍼티 값과 동일하게 설정
- `initial`: 브라우저의 기본 스타일 시트 값으로 설정한다. **TODO** test
- `unset`: 상속되거나 그렇지 않으면 브라우저 기본값으로 설정한다. (이런 걸 natural 값이라고 하는듯?)
- `revert`: **TODO**
- `revert-layer`: **TODO**
