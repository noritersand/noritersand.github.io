---
layout: post
date: 2013-08-24 00:00:00 +0900
title: '[Node.js] Chai Assertion Library'
categories:
  - nodejs
tags:
  - javascript
  - nodejs
  - chai
  - unit-test
  - assertion
---

#### 참고 문서

- [https://www.chaijs.com](https://www.chaijs.com)
- [https://www.chaijs.com/api/assert/](https://www.chaijs.com/api/assert/)

## 설치

```bash
npm install chai --save-dev
```

## 작성 예시

```js
var chai = require('chai');
var expect = chai.expect;
var should = chai.should();
var assert = chai.assert;

expect({a: 1}).to.not.have.property('b');
'foo'.should.have.lengthOf(3);
assert.equal('1', 1);
```

## TODO

Chai는 assert API를 제공하는 라이브러리이 테스트 프레임웤이 아니다. 실행은 결국 모카로 해야 함. 즉, 모카 문서에 이거 합쳐야됨.
