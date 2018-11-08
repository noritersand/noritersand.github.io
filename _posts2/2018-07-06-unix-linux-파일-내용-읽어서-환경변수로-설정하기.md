---
layout: post
date: 2018-07-06 16:13:07 +0900
title: 'Unix/Linux: 파일 내용 읽어서 환경변수로 설정하기'
categories:
  - os
  - unix/linux
tags:
  - unix
  - linux
  - evironment variable
---

쉽죠잉

```
export VARIABLE_NAME = $( cat FILE_LOCATION )
```

```bash
export PID=$(cat /usr/local/tomcat8.5-6/bin/catalina.pid)
echo $PID
```
