---
layout: post
date: 2017-06-28 10:55:00 +0900
title: 'CSS: 클릭 방지용 레이어 (aka. 딤 dimm)'
categories:
  - css
tags:
  - css
  - stylesheet
  - opacity
  - dimm
  - layer
  - 코드모음
---

* Kramdown table of contents
{:toc .toc}

```html
<style>
div.clickPreventer {
	z-index: 999;
	width: 100%;
	height: 100%;
	position: fixed;
	opacity: 0.15;
	background-color: black;
}
</style>

<div class="clickPreventer">
</div>
```
