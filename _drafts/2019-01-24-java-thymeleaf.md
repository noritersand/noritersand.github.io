---
layout: post
date: 2019-01-24 15:04:00 +0900
title: 'Java: Thymeleaf'
categories:
  - java
tags:
  - java
  - thymeleaf
  - template engine
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [http://www.thymeleaf.org/](http://www.thymeleaf.org/)
- [https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html)

## 표현식

### 변수 표현식 `${}`

Variable expressions. 객체 표현식으로 봐도 된다.

### 선택 변수 표현식 `*{}`

Selection variable expressions. 이건 객체의 프로퍼티 표현식이다.

### 메시지 표현식 `#{}`

Message expressions.

### 링크 표현식 `@{}`

Link URL expressions.

## 연산자

### 문자열 연산

```html
<!-- 덧셈 연산자 사용 -->
<div th:text="'Hello!' + ${waldo.name}"><div>

<!-- 파이프로 감싸기 -->
<div th:text="|'Hello!', ${waldo.name}|"><div>
```
