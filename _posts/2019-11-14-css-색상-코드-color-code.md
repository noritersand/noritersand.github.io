---
layout: post
date: 2019-11-14 19:25:00 +0900
title: '[CSS] 색상 코드 color code'
categories:
  - misc
tags:
  - tag me
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [https://htmlcolorcodes.com/](https://htmlcolorcodes.com/)
- [https://www.w3schools.com/colors/colors_hsl.asp](https://www.w3schools.com/colors/colors_hsl.asp)

CSS의 색상 코드는 세 개로 나뉨.

#### RGB

빨간색, 녹색, 파란색의 수치를 0~255 사이의 10진수로 표현한 코드. 표기 방법은 다음과 같다:

```css
div {
  /* 순수한 빨간색(red)의 RGB 코드 */
  color: rgb(255, 0, 0)
}
```

#### HEX

RGB 코드를 00~FF 사이의 16진수로 표현한 코드다. 코드 앞에 `#`을 붙여 구분한다:

```css
/* rgb(255, 0, 0)의 HEX 코드 */
div {
  color: #FF0000
}
```

#### HSL

색상(hue), 채도(saturation), 명도(lightness)를 표현한 코드. 색상의 표현 범위는 0~359이며 360은 0과 같다. 채도는 0~100% 까지인데 100%에 가까울 수록 색이 진하다. 명도는 0%에 가까울 수록 검고 100%에 가까울 수록 하얗다. 명도의 표현범위는 채도와 같다.

```css
/* rgb(255, 0, 0)의 HSL 코드 */
div {
  color: hsl(0, 100%, 50%)
}
```
