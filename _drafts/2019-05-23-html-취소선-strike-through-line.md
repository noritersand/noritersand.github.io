---
layout: post
date: 2019-05-23 11:48:00 +0900
title: '[HTML] 취소선 strike through line'
categories:
  - html
tags:
  - html
  - s
  - del
  - ins
  - strike
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [MDN: HTML reference](https://developer.mozilla.org/ko/docs/Web/HTML/Reference)
- [w3schools: HTML Element Reference](https://www.w3schools.com/tags/default.asp)

HTML5 전에는 취소선을 표현하는 태그로 `<strike>`가 있었으나 HTML5부터 `<s>`, `<del>` 태그로 대체되었다.

`<s>` 태그는 더 이상 정답이 아니거나 정확하지 않거나 연관성이 없는 텍스트를 의미한다. (strikethrough의 약자인 모양)

`<del>` 태그는 교체되었거나 삭제된 텍스트를 의미한다. 교체되었을 경우 `<ins>` 태그로 새 텍스트를 표시한다.

여담으로 CSS로는 다음처럼 취소선을 표현할 수 있다:

```css
p {
  text-decoration: line-through;
}
```
