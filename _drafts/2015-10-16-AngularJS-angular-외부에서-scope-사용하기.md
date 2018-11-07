---
layout: post
date: 2015-10-16 18:27:00 +0900
title: 'AngularJS: angular 외부(?)에서 $scope 사용하기'
categories:
  - javascript
  - angularjs
tags:
  - angularjs
  - window
  - location
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [AngularJs 안의 작은 jQuery - angular.element()](http://programmingsummaries.tistory.com/141)

```js
var scope = angular.element('html').scope()
```
