---
layout: post
date: 2015-10-14 10:01:00 +0900
title: 'AngularJS: $window와 window, $location과 location의 차이'
categories:
  - angularjs
tags:
  - javascript
  - angularjs
  - window
  - location
---

* Kramdown table of contents
{:toc .toc}

#### $window

angularJs는 자바스크립트 브라우저 객체인 window를 래핑한 `$window`를 제공한다.


#### $location

그렇다면 `$location`과 location은 무슨 차이가 있을까.

우선 가장 큰 차이점은 `$location`으로 주소를 변경했을때 페이지 리로드가 발생하지 않는다는 것이다. angularJs는 페이지 리로드를 원할 경우 `$window.location.href`를 사용하라고 권장한다.
