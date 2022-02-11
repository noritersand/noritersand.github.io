---
layout: post
date: 2017-03-24 17:51:00 +0900
title: '[web] put과 post의 차이'
categories:
  - web
tags:
  - web
  - rest
  - put
  - post
---

* Kramdown table of contents
{:toc .toc}

[https://1ambda.github.io/javascripts/rest-api-put-vs-post/](https://1ambda.github.io/javascripts/rest-api-put-vs-post/)

한 줄 요약:

- post는 idempotent하지 않고, put은 idempotent하다.

> idempotent
> 1. 멱등(冪等)의 2. 멱등원(元)
>
> [네이버 영어사전: idempotent](https://en.dict.naver.com/#/entry/enko/df125e744fd141d89c385d7b1b5063c1)

두 줄 요약:

- post는 idempotent하지 않다. 이 말은 연산이 수행될 때마다 새로운 리소스가 생긴다는 뜻이다. (= create)
- put은 idempotent하다. 이 말은 연산이 계속 수행되더라도 리소스가 새로 생기지 않으며, 항상 결과가 같다는 뜻이다. (= update)
