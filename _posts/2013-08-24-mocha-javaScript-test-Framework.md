---
layout: post
date: 2013-08-24 00:00:00 +0900
title: "mocha: javaScript test Framework"
categories:
  - javascript
  - nodejs
tags:
  - nodejs
  - mocha
  - unit test
---

[nodejs - TDD (mocha) framework #2.pdf](/attachment/nodejs-TDD-mocha-framework-2.pdf)

### 설치

```bash
npm install should -g # 모카 사용에 필요한 자바스크립트 모듈
npm install mocha -g
```

### 작성

#### fibofibo.js

```js
exports.ge

tFibo = function (num) {   // 여기서 getFibo는 모듈객체의 프로퍼티(메서드)
  return -1;
};

describe('BDD style', function() {
  before(function() {
    // excuted before test suite
  });

  after(function() {
    // excuted after test suite
  });

  beforeEach(function() {
    // excuted before every test
  });

  afterEach(function() {
    // excuted after every test
  });

  describe('#example', function() {
    it('this is a test.', function() {
      // write test logic
    });
  });
});
```

#### tdd/unitTest.js

```js
var should = require('should');
var fibo = require('../fibonacchi');    // 모듈에 등록한 실제파일에서 js확장자를 뺀 나머지 경로를 적는다.

describe('suite', function () {
  describe('test', function () {
    it('test', function () {
      var result = fibo.getFibo(10);

      result.should.be.eql(10);
      //result.should.be.true;
      //assertfibo(expected, 10);
    });
  });
});
```

### 실행

```bash
mocha tdd/
```
