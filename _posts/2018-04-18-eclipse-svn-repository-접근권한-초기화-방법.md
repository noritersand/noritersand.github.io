---
layout: post
date: 2018-04-18 11:30:42 +0900
title: 'eclipse: svn repository 접근권한 초기화 방법'
categories:
  - devtool
tags:
  - eclipse
---

![](/images/image-svn-pswd-reset-1.png)
`Window` > `Preferences` > `General` > `Security` > `Secure Storage`에 리셋 기능이 있음.

만약 위 방법으로 안된다면 아래 경로에 있는 설정파일을 삭제하면 된다.
```
%APPDATA%\Subversion\auth
```
![](/images/image-svn-pswd-reset-2.png)
하위 폴더를 모두 삭제 ... 하지 말고 모든 하위 폴더 내의 파일만 삭제한다.
