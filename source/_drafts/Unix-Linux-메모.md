---
title: 'Unix/Linux: 메모'
categories:
  - os
  - unix/linux
tags:
  - todo
  - unix
  - linux
---

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
