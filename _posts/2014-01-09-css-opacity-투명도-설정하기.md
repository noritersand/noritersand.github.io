---
layout: post
date: 2014-01-09 17:17:00 +0900
title: '[CSS] opacity 투명도 설정하기'
categories:
  - css
tags:
  - css
  - opacity
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://www.w3schools.com/cssref/css3_pr_opacity.asp](https://www.w3schools.com/cssref/css3_pr_opacity.asp)
- [https://www.w3schools.com/css/css_image_transparency.asp](https://www.w3schools.com/css/css_image_transparency.asp)

```html
<div style="filter:alpha(opacity=15); opacity:0.15;
        width: 200px; height: 60px; background-color: black;">
    <br/>거어어어어어어어의 안보임
</div>
```

<div class="outline">
    <div style="filter:alpha(opacity=15); opacity:0.15;
            width: 200px; height: 60px; background-color: black;">
        <br/>거어어어어어어어의 안보임
    </div>
</div>

0에 가까울수록 투명하며 1에 가까울수록 불투명해진다. (`filter:alpha`는 0-100)

opacity만 설정할 경우 일부 브라우저에서 호환성 문제가 발생하기 때문에 filter를 포함해야 한다고 카더라.
