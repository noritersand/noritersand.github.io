---
layout: post
date: 2016-08-11 00:00:00 +0900
title: 'jQuery: Form to JSON'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - 코드모음
  - JSON
---

* Kramdown table of contents
{:toc .toc}

```js
function formToJson(targetElement) {
  var param = new Object();
  $(targetElement).find('input, textarea, select').each(function(idx, ele) {
    var name = $(ele).attr('name'),
      type = $(ele).attr('type'),
      checked = $(ele).prop('checked'),
      disabled = $(ele).prop('disabled'),
      val = $(ele).val();
    if (typeof name === 'undefined') {
      return true; // jquery each continue
    }
    if (type == 'checkbox' || type == 'radio') {
      if (checked && !disabled) {
        param[name] = val;
      }
    } else {
      if (!disabled) {
        param[name] = val;
      }
    }
  });
  return param;
}
```

끗.
