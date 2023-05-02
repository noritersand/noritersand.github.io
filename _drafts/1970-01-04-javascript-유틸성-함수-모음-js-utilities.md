---
layout: post
date: 1970-01-01 00:00:00 +0900
title: '[JavaScript] 유틸성 함수 모음'
categories:
  - javascript
tags:
  - draft-permanantly
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

`input`이나 `keyup`, `keydown` 이벤트 핸들러로 쓸 수 있음


## 숫자만 제한하기

```js
class Restrictor {
  static onlyNumeric({value = 0, max = 10, min = 0}) {
    value = this.#removeNonNumeric(value);
    if (value === 0 || value === '-') {
      return value
    }
    if (!value) {
      return null;
    }
    if (isFinite(value)) {
      return this.#cut(parseInt(value), min, max);
    }
    return value;
  }

  static #cut(v, min, max) {
    return v < min ? min : max < v ? max : v;
  }

  static #removeNonNumeric(v) {
    return typeof v === 'string' ? v.replace(/[^0-9\-]/g, '') : v;
  }
}

Restrictor.onlyNumeric({value: 0, max: 10, min: -10}); // 0
Restrictor.onlyNumeric({value: 5, max: 10, min: -10}); // 5
Restrictor.onlyNumeric({value: -5, max: 10, min: -10}); // -5
Restrictor.onlyNumeric({value: 25, max: 10, min: -10}); // 10
Restrictor.onlyNumeric({value: -25, max: 10, min: -10}); // -10
Restrictor.onlyNumeric({value: '-', max: 10, min: -10}); // -
Restrictor.onlyNumeric({value: ' - ', max: 10, min: -10}); // -
Restrictor.onlyNumeric({value: '99', max: 10, min: -10}); // 10
Restrictor.onlyNumeric({value: ' 999 ', max: 10, min: -10}); // 10
Restrictor.onlyNumeric({value: ' ', max: 10, min: -10}); // null
Restrictor.onlyNumeric({value: '   ', max: 10, min: -10}); // null
Restrictor.onlyNumeric({value: '-9', max: 10, min: -10}); // -9
Restrictor.onlyNumeric({value: 'qwer', max: 10, min: -10}); // null
```
