---
layout: post
date: 2019-11-14 19:25:00 +0900
title: '[CSS] 색상 코드 color code'
categories:
  - css
tags:
  - css
  - color
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [MDN: CSS color](https://developer.mozilla.org/en-US/docs/Web/CSS/color)
- [https://www.w3schools.com/cssref/css_colors_legal.asp](https://www.w3schools.com/cssref/css_colors_legal.asp)
- [https://htmlcolorcodes.com/](https://htmlcolorcodes.com/)
- [https://www.w3schools.com/colors/colors_rgb.asp](https://www.w3schools.com/colors/colors_rgb.asp)
- [https://www.w3schools.com/colors/colors_hexadecimal.asp](https://www.w3schools.com/colors/colors_hexadecimal.asp)
- [https://www.w3schools.com/colors/colors_hsl.asp](https://www.w3schools.com/colors/colors_hsl.asp)

CSS 색상 코드를 정리한 글.

#### 색상 이름

red, blue 등의 미리 정해진 색상 이름을 사용한다. 사용 가능한 이름은 [여기](https://www.w3schools.com/colors/colors_names.asp)에서 확인.

```css
div { color: red; }
```

#### HEX

빨간색, 녹색, 파란색의 수치를 0 \~ FF 사이의 16진수로 표현한 코드다. 코드 앞에 `#`을 붙여 구분한다:

```css
/* red의 HEX 코드 */
div { color: #FF0000; }
```

#### RGB

빨간색, 녹색, 파란색의 수치를 0 \~ 255 사이의 10진수로 표현한 코드. 표기 방법은 다음과 같다:

```css
/* red의 RGB 코드 */
div { color: rgb(255, 0, 0); }
```

#### HSL

색상(hue), 채도(saturation), 명도(lightness)를 표현한 코드. 색상의 표현 범위는 0 \~ 359이며 360은 0과 같다. 채도의 범위는 0% \~ 100%인데 100%에 가까울 수록 색이 진하다. 명도의 범위는 채도와 같으며, 0%에 가까울 수록 어둡고 100%에 가까울 수록 밝다.

```css
/* red의 HSL 코드 */
div { color: hsl(0, 100%, 50%); }
```

#### RGBA, HSLA

A는 ALPHA의 줄임말이며 불투명도(opacity)를 의미한다. RGB와 HSL 코드 끝에 불투명도를 지정하는 식이다. 불투명도의 표현 범위는 0.0부터 1.0까지다.

```css
/* red with opacity */
div { color: rgba(255, 0, 0, 0.3); }
/* green with opacity */
div { color: hsla(120, 100%, 50%, 0.3); }
```
