---
layout: post
date: 2018-10-05 15:17:00 +0900
title: '[Tomcat] server.xml의 autoDeploy 사용시 주의할 점'
categories:
  - tomcat
tags:
  - was
  - tomcat
  - autodeploy
---

* Kramdown table of contents
{:toc .toc}

autoDeploy가 true이고 war가 교체되었을 때, 그리고 docBase 폴더 내에 심볼릭 링크가 존재할 때 톰캣이 심볼릭 링크 내의 파일까지도 재귀 삭제해버리는 현상이 발생한다.

심볼릭 링크의 실제 경로를 일회성 혹은 임시 폴더로 사용한다면 문제가 없겠지만, 파일을 모아놓고 지속적으로 사용하는 경로라면 낭패를 볼 수 있다.

따라서 docBase내에 심볼릭 링크가 있을 경우 autoDeploy를 false로 설정하던가, 아니면 심볼릭 링크를 appBase 부터 아예 다른 경로로 이동하고 docBase에서 해당 경로를 참조하게 해야 한다.
