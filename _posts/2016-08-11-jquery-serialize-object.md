---
layout: post
date: 2016-08-11 00:00:00 +0900
title: '[jQuery] serialize object'
categories:
  - jquery
tags:
  - javascript
  - jquery
  - code-snippet
  - json
  - serialize
  - object
  - form
---

* Kramdown table of contents
{:toc .toc}

```js
function serializeObject(targetElement) {
  var obj = [];
  $(targetElement).find('input, textarea, select').each(function(idx, ele) {
    var $ele = $(ele);
    var name = $ele.attr('name'),
      type = $ele.attr('type'),
      checked = $ele.prop('checked'),
      disabled = $ele.prop('disabled'),
      val = $ele.val();
    if (typeof name === 'undefined') {
      return true; // jquery each continue
    }
    if (type == 'checkbox' || type == 'radio') {
      if (checked && !disabled) {
        obj[name] = val;
      }
    } else {
      if (!disabled) {
        obj[name] = val;
      }
    }
  });
  return obj;
}
```

jQuery 객체인 `targetElement` 자식, 손자들 중 input, textarea, select 태그를 찾아서 jsObject로 만드는 함수. 태그의 name 속성을 object의 키, value나 체크/선택 상태를 object의 값으로 사용한다.

깊이가 1이고 재귀탐색이 없어 **name 속성이 중복되면 나중의 것이 먼저 것을 덮어쓰는 한계가 있다.**

고쳐야 하는데 귀찮... 차라리 macek의 [jquery-serialize-object](https://github.com/macek/jquery-serialize-object)를 쓰는게 낫다.

끗.
