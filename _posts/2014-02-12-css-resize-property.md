---
layout: post
date: 2014-02-12 13:29:00 +0900
title: '[CSS] resize property'
categories:
  - css
tags:
  - css
  - resize
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 사이트와 문서

- [http://www.w3schools.com/cssref/css3_pr_resize.asp](http://www.w3schools.com/cssref/css3_pr_resize.asp)
- [http://www.w3schools.com/cssref/playit.asp?filename=playcss_resize&preval=both](http://www.w3schools.com/cssref/playit.asp?filename=playcss_resize&preval=both)


요소의 사이즈 조절 가능 여부를 설정한다. **IE와 오페라는 지원하지 않는다.**

```
resize: none|both|horizontal|vertical|initial|inherit:
```

```html
<textarea rows="5" cols="30" readonly="readonly"
        style="resize: horizontal; background-color: whitesmoke;">
```

<div class="outline">
    <textarea rows="5" cols="30" readonly="readonly" style="resize: horizontal; background-color: whitesmoke;">
    수평 사이즈만 조절 가능한 텍스트에리어.
    </textarea>
</div>
