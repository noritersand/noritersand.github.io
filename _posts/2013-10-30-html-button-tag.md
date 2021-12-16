---
layout: post
date: 2013-10-30 12:23:00 +0900
title: '[HTML] button tag'
categories:
  - html
tags:
  - html
  - button
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [[MDN] \<button\>: The Button element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/button)
- [https://www.w3schools.com/tags/tag_button.asp](https://www.w3schools.com/tags/tag_button.asp)

`<button>`태그에 type 속성을 명시하지 않을 경우 type의 기본값은 submit으로 설정된다. 이 때문에 `<form>`안에 위치하게 되면 버튼 클릭 시 `HTMLFormElement.submit()` 메서드가 작동하게 된다.

따라서 아래처럼 작성해야 submit을 방지할 수 있다:

```html
<form>
    <button type="button"></button>
</form>
```
