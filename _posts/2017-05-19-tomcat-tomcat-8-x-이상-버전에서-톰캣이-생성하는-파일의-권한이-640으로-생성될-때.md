---
layout: post
date: 2017-05-19 22:26:00 +0900
title: '[Tomcat] tomcat 8.x 이상 버전에서 톰캣이 생성하는 파일의 권한이 640으로 생성될 때'
categories:
  - tomcat
tags:
  - was
  - tomcat
  - umask
  - chmod
  - debugging-log
---

* Kramdown table of contents
{:toc .toc}

톰캣으로 구동하는 웹 어플에서 파일을 업로드 할 때 other 사용자한테 읽을 수 있도록 권한이 부여된 파일이 생성되어야 하는데 이상하게도 폴더는 750, 파일은 640으로 생성된다.

`bin/catalina.sh`를 보면 umask를 027로 설정하는것이 원인이었고 이 부분을 코멘트 처리하고 재시작하니 644로 생성하더라.
