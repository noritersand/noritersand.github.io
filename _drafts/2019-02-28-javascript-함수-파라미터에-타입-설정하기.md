---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'untitled'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - parameter
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://stackoverflow.com/questions/8407622/set-type-for-function-parameters](/https://stackoverflow.com/questions/8407622/set-type-for-function-parameters)

```js
/**
 * @param {Date} myDate The date
 * @param {string} myString The string
 */
function myFunction(myDate, myString) {
    //do stuff
}
```

뭐 요딴식으로 하면 편집기에 따라 툴팁으로 파라미터의 타입이 보이기도 한다. (일단 vscode는 됨) 그렇다고 해도 애초에 언어 자체가 타입 체크를 하지 않으니까 큰 의미는 없지만...
