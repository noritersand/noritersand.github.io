---
layout: post
date: 1970-01-01 00:00:00 +0900
title: '[JavaScript] 유틸성 함수 모음'
categories:
  - javascript
tags:
  - ecmascript
  - javascript
  - code-snippet
---

* Kramdown table of contents
{:toc .toc}

#### 버전 정보

- ECMAScript Latest


## 숫자를 currency로

1억 보다 크면 1억으로 강제 설정하는 코드 첨가한 버전:

```js
var val = '1234567';
val = val.replaceAll(/\D/g, '');
if (isFinite(val)) {
  val = 10E7 < val ? 10E7 : val;
}
val = Intl.NumberFormat('en-US').format(val);
console.log(val); // 1,234,567
```

`keyup` 이벤트 핸들러로 쓸 수 있음
