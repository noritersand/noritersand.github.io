---
layout: post
date: 2019-03-18 09:48:00 +0900
title: '[JavaScript] private 프로퍼티와 메서드 선언'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
  - private
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [somewhere](somewhere)

여러 방법이 있는것 같은데..

## 1

```js
var WeirdMath = (function() {
  // private variable
  let c = 1;
  // private method
  function plusOne(t) {
    return t + c;
  }

  return { // public method
    sum: function(a, b) {
      return plusOne(a + b);
    }
  };
})();

console.log(WeirdMath.sum(1, 2)); // 4
typeof WeirdMath.sum // function
typeof WeirdMath.c // undefined
typeof WeirdMath.plusOne // undefined
```
