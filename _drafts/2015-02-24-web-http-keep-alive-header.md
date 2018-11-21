---
layout: post
date: 2015-02-24 17:29:00 +0900
title: 'web: HTTP keep-alive header'
categories:
  - web
tags:
  - http
  - keep alive
---

* Kramdown table of contents
{:toc .toc}

HTTP 1.1에서 지원하는 옵션으로 매 요청마다 소켓이 생성되어 자원이 낭비됨을 방지하기 위해 클라이언트와 서버가 모두 keep-alive를 헤더로 지정할 경우 특정 시간이나 횟수를 지정하여 정해진 횟수나 시간만큼의 소켓을 유지하며 이 소켓이 유지되는 동안은 같은 클라이언트가 반복 요청할 땐 남아있는 소켓을 재활용한다.

HTTP에서는 TCP/IP와 달리 양방향 통신의 유지를 의미하지는 않는다.
