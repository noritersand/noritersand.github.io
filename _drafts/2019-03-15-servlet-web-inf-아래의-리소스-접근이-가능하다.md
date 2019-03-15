---
layout: post
date: 2019-03-15 17:37:00 +0900
title: 'Servlet: WEB-INF 아래의 리소스 접근이 가능하다?'
categories:
  - servlet
tags:
  - java
  - servlet
  - web-inf
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [somewhere](/somewhere)

spring-boot + maven 환경이었는데 어떻게 한건지는 모름.

`src/main/resources` 경로에 있는 스크립트 파일들이 `WEB-INF/classes`로 배포가 되며, 이 스크립트를 브라우저에서 직접 접근 가능하다.
