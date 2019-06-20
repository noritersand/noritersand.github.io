---
layout: post
date: 2017-06-23 18:01:00 +0900
title: 'Tomcat: 서브도메인간 세션 공유: JSESSIONID를 .도메인 쿠키로 만들기'
categories:
  - tomcat
tags:
  - was
  - tomcat
  - apache
  - jsessionid
  - session
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- 몰라

#### 테스트 환경

- 톰캣이겠지?

server.xml이랑 context.xml에 sessionCookieDomain 설정해야하는 모양.
