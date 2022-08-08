---
layout: post
date: 2022-08-08 22:01:43 +0900
title: '[java] 롬복 Project Lombok'
categories:
  - java
tags:
  - java
  - lombok
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://projectlombok.org/](https://projectlombok.org/)

#### 버전 정보

- x.x.x

## 개요

어쩌구

TODO

## 제목

### @Builder

빌더 패턴으로 클래스를 초기화할 때 사용한다.

```java
@Data
@Builder
class BuildMe {
    private String a;
    private int b;
}

...

BuildMe instnc = BuildMe.builder().a("yo").b(123).build();
```

근데 이렇게 쓰다보면 마이바티스라던지 스프링 바인딩 등에서 기본 생성자가 없다며 에러가 발생할 수 있다. 따라서 기본생성자를 만들어주는 `@NoArgsConstructor`와 함께 쓴다.

그런데 또 이렇게만 하면 `@Builder` 어노테이션에서 에러를 뿜뿜하며 작동하지 않으므로 `@AllArgsConstructor` 어노테이션도 덧붙여야 한다. 🤣

그래서 결론은 빌더 패턴을 쓰려면 `@Builder`, `@NoArgsConstructor`, `@AllArgsConstructor` 세 개를 다 쓰는게 속 편하다는 것.

```java
@Data
@Builder
@NoArgsConstructor
@AllArgsConstructor
class BuildMe {
    private String a;
    private int b;
}
```
