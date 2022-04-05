---
layout: post
date: 1970-01-02 00:00:00 +0900
title: '[css] reset.css'
categories:
  - css
tags:
  - css
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

브라우저의 기본 스타일을 죄 삭제하는 CSS 코드:

```css
body {font-size:12px; color:#666; overflow-y:scroll; line-height:19px; font-family:'Malgun Gothic',-apple-system, 'dotum', 'sans-serif';}
body, html {height:100%; width:100%;}
li {list-style:none;}
fieldset, ul, li, h1, h2, h3, h4, h5, ol, dl, dt, dd, p, body, input {margin:0; padding:0;}
fieldset {border:0;}
legend {display:none;}
iframe {border:none;}
img {border:0; vertical-align:middle;}
button {border:0; cursor:pointer;}
::selection {background:#ffdda8; color:#967540;}
a {text-decoration:none;}
a:link {color:#656565;}
a:visited {color:#888888;}
a:hover {color:#676767;}
a:active {color:#777; }
table {border-collapse:collapse;}
table caption {display:none;}
input, textarea, button, select {font-size:12px; color:#666; vertical-align:middle; font-family:'Malgun Gothic', -apple-system, 'dotum', 'sans-serif';}
h1, h2, h3, h4, h5, h6 {font-size:12px; color:#666;}
th {font-weight:normal; text-align:left;}
input[type='month'], input[type='url'], input[type='tel'], input[type='color'], input[type='datetime'], input[type='datetime-local'],
input[type='text'], input[type='number'], input[type='password'], input[type='date'], input[type='email'] {border:1px solid #ccc; height:26px; width:100%; padding:0 5px; line-height:26px; vertical-align:middle;}
textarea {width:100%; padding:0 5px; border:1px solid #aaa; line-height:16px; height:52px; white-space:pre-line;}
select {line-height:40px; border:1px solid #ccc; height:26px; padding:0 5px; margin:0; box-sizing:border-box; vertical-align:middle; box-sizing:border-box;}
input[readonly='readonly'] {background-color:#eee;}
div, a, li, ol, ul, dl, dt, dd, span, strong, p, h1, h2, h3, h4, h5, img, input, button, iframe, textarea {box-sizing:border-box; } /* transition:0.3s all linear; transition:all 0.3s linear 0.9s; */
```

끗.
