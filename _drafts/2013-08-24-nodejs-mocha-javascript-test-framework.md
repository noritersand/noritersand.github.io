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
      assert.strictEqual([1, 2, 3].indexOf(4), -1);
    });
  });
});
```

Mocha는 Java의 JUnit과 다르게 **기댓값이 우측**이다😒:

```js
var assert = require('assert');
var foo = 'bar';
describe('test', function() {
  it('should be fail', function() {
    assert.strictEqual(foo, 'E');
  });
});
```

```bash
$ mocha test.js

  ...

  1) test
       should be fail:

      AssertionError [ERR_ASSERTION]: Expected values to be strictly equal:

'bar' !== 'E'

      + expected - actual

      -bar
      +E
```

### 실행

```bash
mocha test/**
# test 폴더의 모든 파일 테스트
```
