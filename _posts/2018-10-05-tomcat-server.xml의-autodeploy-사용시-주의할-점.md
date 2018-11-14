---
layout: post
date: 2018-10-05 15:17:00 +0900
title: 'Tomcat: server.xml의 autoDeploy 사용시 주의할 점'
categories:
  - was/webserver
  - tomcat
tags:
  - tomcat
  - autodeploy
---

* Kramdown table of contents
{:toc .toc}

autoDeploy가 true이고 war가 교체되었을 때, 그리고 docBase 폴더 내에 심볼릭 링크가 존재할 때 톰캣이 심볼릭 링크 내의 파일을 리커시브로 삭제해버리는 현상이 발생한다.

따라서 docBase내에 심볼릭 링크가 있을 경우 autoDeploy를 false로 설정하던가, 아니면 심볼릭 링크를 appBase 부터 아예 다른 경로로 이동하고 docBase에서 해당 경로를 참조하게 해야 한다.
