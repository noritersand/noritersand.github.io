---
layout: post
date: 2013-08-24 00:00:00 +0900
title: '[Node.js] Mocha, JavaScript test Framework'
categories:
  - nodejs
tags:
  - javascript
  - nodejs
  - mocha
  - unit-test
---

#### 참고한 문서

- [nodejs - TDD (mocha) framework #2.pdf](/attachment/nodejs-TDD-mocha-framework-2.pdf)
- [https://mochajs.org/](https://mochajs.org/)

### 설치

```bash
npm install mocha -g
npm install should -g # should가 필요한 경우에만
```

### 작성 예시

```js
var assert = require('assert');
describe('Array', function() {
  describe('#indexOf()', function() {
    it('should return -1 when the value is not present', function() {
      assert.equal([1, 2, 3].indexOf(4), -1);
    });
  });
});
```

### 실행

```bash
mocha test/**
# test 폴더의 모든 파일 테스트
```
