---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'Unix/Linux: 권한 설정 chmod, chown, umask'
categories:
  - os
  - unix/linux
tags:
  - todo
  - unix
  - linux
---

![](/images/Unix-Linux-권한-설정-chmod-chown-umask-image.png)
[이미지 출처](http://endlessgeek.com/2014/02/chmod-explained-linux-file-permissions)

## chmod
```
chmod [-Rcfv] [--recursive] [--changes] [--silent] [--quiet] [--verbose] [--help] [--version] mode file
```
```bash
chmod 700 file  # 오너(owner, 파일을 만든 사람)에게만 모든 권한 부여
chmod 770 file  # 오너와 오너가 속한 그룹에 모든 권한 부여
chmod 777 file  # 오너와 오너의 그룹, 그 외 모든 유저에게 모든 권한 부여
```

## chown
```
chown [-Rcfv] [--recursive] [--changes] [--help] [--version] [--silent] [--quiet] [--verbose] [user][:.][group] file...

chown 소유권자:그룹식별자  바꾸고 싶은 파일 이름
```
```bash
chown tbs:tbs ./some-folder

```

## umask
TODO
