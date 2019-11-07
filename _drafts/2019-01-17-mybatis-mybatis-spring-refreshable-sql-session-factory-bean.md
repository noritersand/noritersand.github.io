---
layout: post
date: 2019-01-17 17:25:00 +0900
title: '[Mybatis] MyBatis-Spring: RefreshableSqlSessionFactoryBean'
categories:
  - mybatis
tags:
  - java
  - mybatis
  - spring
  - hot deploy
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://www.mybatis.org/spring/ko/factorybean.html](http://www.mybatis.org/spring/ko/factorybean.html)
- [http://bryan7.tistory.com/117](http://bryan7.tistory.com/117)
- [https://gist.github.com/sbcoba/a51a66a64d3441a88558](https://gist.github.com/sbcoba/a51a66a64d3441a88558)

매퍼 수정되면 자동으로 갱신하는 SessionFactory.

구글링 결과가 한글만 있는걸 봐선 공식 기능은 아닌걸로 추정된다. `RefreshableSqlSessionFactoryBean`이라는 이름도.
