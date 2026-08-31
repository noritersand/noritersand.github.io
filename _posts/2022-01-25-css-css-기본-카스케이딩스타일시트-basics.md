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

- [CSS: Cascading Style Sheets \| MDN](https://developer.mozilla.org/en-US/docs/Web/CSS)


## 개요

CSS(Cascading Sytle Sheet) 기본 사용법 정리.


## 용어

### At-rules

```css
@charset "utf-8";

h1 { 
  font-size: 37.2px;
  border-bottom: 3px solid black;
}
```

`@charset`처럼 `@`로 시작하는 선언을 *At-rule* 이라 부른다. `h1`은 셀렉터, `font-size`, `border-bottom`은 프로퍼티다.

### 박스 모델 Box Model

CSS의 박스 모델은 웹 엘리먼트를 콘텐츠, 패딩, 테두리, 마진 네 가지 영역으로 나누어 엘리먼트의 레이아웃을 설정하는 개념. 각 영역은 엘리먼트의 크기와 배치를 결정한다.

박스의 유형은 크게 블록 박스와 인라인 박스로 나뉜다.

### 뷰포트 Viewport

CSS에서 뷰포트는 브라우저의 윈도우 내에 웹 페이지가 실제로 표시되는 영역을 말한다. 여기에는 스크롤바나 도구 모음 같은 브라우저 UI의 일부는 포함되지 않는다. 뷰포트의 크기는 브라우저의 크기와 사용자가 설정한 확대/축소 비율에 따라 달라진다.


## 스타일 적용하기

### inline

HTML 태그의 `style` 속성을 사용하여 직접 지정한다.

```html
<div style="color:red;">여기는 적색으로 나타난다.</div>
```

### embedded(internal)

스타일 시트의 기본적인 사용 방법으로 html의 `<head></head>` 사이에 삽입하여 `<style type="text/css"><style>` 사이에 작성한다. 또한 같은 스타일을 중복해서 지정 했을 때는 나중에 지정 한 것이 적용된다.

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

우선순위(혹은 명시도, 우선권)를 결정하는 엘리먼트는 여러 가지가 있다. 크게 보면 `!important`가 가장 우선 적용되고 나머지는 선언 방식이나 얼마나 구체적으로 셀렉터를 작성했느냐에 따라 달라진다. **만약 우선순위가 동일한 경우 나중에 불러온 스타일이 적용**된다.

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


## 레이아웃 모드

레이아웃 모드란 CSS에서 박스 모델의 크기와 위치를 계산하여 화면에 배치하는 레이아웃 알고리즘을 말한다. 크게 다섯가지가 있다.

### 블록 레벨 레이아웃 Block-level Layout

문서의 일반적인 흐름(normal flow)을 구성하는 1차원 레이아웃 방식이다. 블록 박스는 블록 축 방향으로 순차적으로 배치되며, 기본적으로 사용할 수 있는 가로 공간을 채우도록 크기가 결정된다.

### 절대 위치 레이아웃 Absolutely-positioned Layout

일반적인 문서 흐름(normal flow)에서 제외되어 배치되는 방식이다. 가장 가까운 Containing Block을 기준으로 `top`, `right`, `bottom`, `left` 오프셋을 계산하여 위치를 결정한다.

### 테이블 레이아웃 Table Layout

행과 열로 구성된 표 형태의 레이아웃 방식이다. 셀의 크기와 열 너비를 함께 계산하며, 셀 간 높이 맞춤과 `vertical-align`을 통한 수직 정렬을 지원한다.

### 플렉스박스 레이아웃 Flexbox Layout(Flexible Box Layout)

주축(main axis)과 교차 축(cross axis)을 기준으로 엘리먼트를 한 방향으로 배치하는 1차원 레이아웃 모델이다. justify-content와 align-items 등을 사용해 공간 분배와 정렬을 유연하게 제어할 수 있다.

### 그리드 레이아웃 Grid Layout

행(row)과 열(column)을 동시에 제어하는 2차원 레이아웃 모델이다. `grid-template-columns`와 `grid-template-rows` 등을 사용해 격자 트랙을 정의하고, 엘리먼트를 원하는 셀이나 영역에 배치할 수 있다.


## 상속

- <https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance>
- <https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade>

자식 엘리먼트가 부모 엘리먼트의 CSS 프로퍼티 값을 그대로 사용하는 것을 말함.

모든 프로퍼티가 상속되는 것은 아님. (e.g., `width: 50%`)


<https://developer.mozilla.org/ko/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance>

### 상속 제어

<https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#controlling_inheritance>

CSS는 상속을 제어하기 위한 특수 범용 프로퍼티 값(special universal property values)을 제공한다. 모든 CSS 프로퍼티에서 유효하다.

- `initial`: CSS 명세에 정의된 기본 초깃값으로 설정한다. 속성마다 초깃값이 다르다. 예를 들어 `display: initial`은 `display` 프로퍼티의 기본값인 `inline`으로 되돌린다.
- `inherit`: 부모 엘리먼트로부터 프로퍼티 값을 상속받는다. 명시적으로 부모의 값을 따르도록 만들어, 기본적으로 상속되지 않는 프로퍼티도 강제로 상속 효과를 내도록 할 수 있다.
- `revert`: 현재 엘리먼트에 적용된 스타일 선언이 없으면 **원래 적용되었을 값**으로 되돌린다. 즉, 만약 현재의 CSS 규칙이 없다면 브라우저 기본 스타일(또는 사용자 스타일) 같은 다른 출처의 스타일 규칙이 적용되었을 텐데 그 값으로 되돌린다.
- `revert-layer`: CSS Cascade Layers 기능을 사용할 때 유효한 값. 현재 레이어의 스타일 선언을 무시하고, 바로 아래(우선순위가 낮은, 뒤에 있는 레이어) 레이어에 설정된 값으로 되돌린다
- `unset`: 프로퍼티를 초기 상태로 되돌리는 설정. 상속되는 프로퍼티는 `inherit` 처럼 작동하고, 상속되지 않는 속성은 `initial` 처럼 작동한다. (공식 용어는 아니지만 natural 값이라고도 하는 모양)


## 값과 단위

[CSS values and units - Learn web development \| MDN](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Values_and_units)

**TODO**

### 길이 Lengths

#### 절대 길이 단위 Absolute length units

- `cm`: 센티미터 단위. 1센티미터 = 37.8픽셀 = 25.2/64인치
- `mm`: 밀리미터 단위. 1밀리미터 = 0.1센티미터
- `Q`: 쿼터밀리미터 단위. 1쿼터밀리미터 = 1/40센티미터(= 0.025)
- `in`: 인치 단위. 1인치 = 2.54센티미터 = 96픽셀
- `pc`: ~~피까?~~ 피카 단위. 1피카 = 1/6인치(= 0.1666666667)
- `pt`: 포인트 단위. 1포인트 = 1/72인치(= 0.0138888889)
- `px`: 픽셀 단위. 1픽셀 = 1/96인치(= 0.0104166667)

#### 상대 길이 단위 Relative length units

**TODO**

- `em`: 
- `vw`: 뷰포트 너비(Vieport Width)의 1%
- `vh`: 뷰포트 높이(Vieport Height)의 1%
- `dvh`: 동적 뷰포트 높이(Dynamic Viewport Height)의 1%. `vh`와 다르게, 모바일 브라우저 환경에서 주소창이나 툴바에 따라 동적으로 변한다.
- `svh`: 주소창과 툴바가 모두 노출된 상태의 뷰포트 높이
- `lvh`: 주소창과 툴바가 모두 축소된 상태의 뷰포트 높이. `vh`와 유사하다.

### 비율

#### flex

```css
.grid-container {
  display: grid;
  grid-template-columns: 1fr 1fr 2.5fr 1.5fr;
}
```

`grid-template-columns`나 `grid-template-rows` 프로퍼티에서 쓰이는 단위. grid container 내부에서 고정 크기를 가진 엘리먼트들을 제외한 남은 공간(leftover space) 중 grid track이 차지할 비율을 나타내는 값이다. 숫자 뒤에 `fr`을 붙어서 표기한다.

ℹ️ Grid Layout에선 행 전체 한 줄을 grid row track, 열 전체 한 줄을 grid column track이라 부른다.

fr은 Fraction의 줄임말이다. 그리고 이름이 flex라서 Flexbox에서 쓸 것 같지만 Grid Layout에서만 쓴다.
