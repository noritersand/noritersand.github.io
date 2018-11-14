---
layout: post
date: 1970-01-01 00:00:00 +0900
title: 'Unix/Linux: 메모'
categories:
  - os
  - linux
tags:
  - linux
  - memo
---

* Kramdown table of contents
{:toc .toc}

별칭같은 설정은 일반적으로
유저 `/home/user/.bashrc`
루트 `/etc/rc.d/rc.local`
에 작성. 단, 위처럼 작성 할 경우 유저계정과 루트계정이 서로 독립적인 설정을 갖는다.

[지역에 관한 내용 링크](http://originalchoi.tistory.com/19)

```bash
cat /proc/meminfo | grep MemTotal
```
```bash
top -p 12345 # 12345 프로세스에 대한 CPU/메모리 점유율 확인
```
```bash
free
```

## [NFS](https://en.wikipedia.org/wiki/Network_File_System)
Network File System. 원격지의 디렉토리/파일(혹은 드라이브)을 로컬 저장소에 있는것처럼 하는 것.

## [SSHFS](https://en.wikipedia.org/wiki/SSHFS)
SSH File System. SSH로 원격지에 연결하여 디렉토리/파일을 마운트하거나 상호작용하는 파일 시스템 클라이언트. (단순히 NFS에 SSH를 적용한건 아닌것 같다.)
