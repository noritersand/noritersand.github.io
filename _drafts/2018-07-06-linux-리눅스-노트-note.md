---
layout: post
date: 2018-07-06 16:13:07 +0900
title: '[Linux] 리눅스 노트'
categories:
  - linux
tags:
  - linux
  - os
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [어디](어디)


## 개요

리눅스 관련 이런저런 잡지식 모음.


## 환경 변수

### 환경 변수 추가

```bash
export LANG = "ko.KR.UTF-8"
```

### 파일 내용 읽어서 환경 변수로 설정하기

```
export VARIABLE_NAME = $( cat FILE_LOCATION )
```

```bash
export PID=$(cat /usr/local/tomcat8.5-6/bin/catalina.pid)
echo $PID
```


## 원하는 파일을 검색해서 특정 디렉터리로 이동하기(find + mv)

```
aaa_dir/a.png
aaa_dir/b.png
aaa_dir/bbb_dir/c.png
```

위를

```
zzz_dir/a.png
zzz_dir/b.png
zzz_dir/c.png
```

이렇게 바꾸려고 할 때

find 명령어로 이동하려는 파일 검색:

```bash
find aaa_dir(절대경로 또는 상대경로) -name "*.png"
```

여기서 파일명만 추려냄:

```bash
find aaa_dir -name "*.png" -printf "%f\n"
```

마지막으로 파일 이동까지:

```bash
find aaa_dir -name "*.png" -printf "%f\n" -exec mv {} zzz_dir/ \;
```

끗.


## 네트워크 인터페이스

컴퓨터와 네트워크를 연결하는 장치를 의미하지만 물리적 형태가 없을 수도 있음.

`eth0`은 네트워크 인터페이스의 식별자로 자주 사용되는데 `eth`는 Ethernet을 의미하며 0은 첫 번째 Ethernet 인터페이스라는 의미다.

리눅스에서만 쓰는 개념은 아니고 윈도우 등의 다른 운영 체제에도 존재한다.

### Network bonding

리눅스에서 네트워크 인터페이스를 결합하여 가상 인터페이스(혹은 논리적 인터페이스)를 구성하는 기술이다. 인터페이스 간 로드 밸런싱, 장애조치, 대역폭 확장, 가용성(HA, High Availability) 및 신뢰성 향상을 위해 사용한다.

### enp0sX

리눅스에서 네트워크 인터페이스를 식별하는 데 사용되는 이름이다. `enp0s`는 네트워크 인터페이스를, `X`는 숫자로 대체되며 속도를 나타낸다. 이름은 이 밖에도 `eth0`, `eth1`, `wlan0`와 같은 형식일 수 있는데, 최근 버전의 리눅스에선 `enp0sX` 또는 `ensX`를 사용하는 경우가 많다고 한다.


## Vim 단축키

[http://www.joinc.co.kr/modules/moniwiki/wiki.php/Site/Vim/Documents/UsedVim](http://www.joinc.co.kr/modules/moniwiki/wiki.php/Site/Vim/Documents/UsedVim)

![](/images/vim-hotkey-1.png)
![](/images/vim-hotkey-2.jpg)

### 복사 후 붙이기

<kbd>v</kbd> 혹은 <kbd>V</kbd>로 블록 지정 후 <kbd>y</kbd>로 복사. 다음으로 <kbd>p</kbd> 혹은 <kbd>P</kbd>를 눌러 붙여넣기.

### 잘라내서 붙이기

줄 단위로 삭제할 땐 <kbd>d + d</kbd>로, 블록 지정하고 싶으면 <kbd>v</kbd>로 블록 지정 후 <kbd>d</kbd>를 누른다.  
옛날 OS면 <kbd>x</kbd>로 레지스터를 지정해야 저장되는데 요즘 OS는 그냥 이렇게 지워도 자동으로 클립보드에 저장되는 모양

그리고 원하는 지점에서 <kbd>p</kbd>를 눌러 붙여넣으면 끗.

### 되돌리기/다시 되돌리기 Undo/Redo

- <kbd>u</kbd>
- <kbd>ctrl + u</kbd>


## 뭔가 이상할 때

### 서버 접속이 안되거나 서비스가 죽었거나 암튼 뭐가 이상할 때

syslog 혹은 `kernel.log` 파일에 무슨 일이 일어났는지 기록되어있을 가능성이 있다.

`kernel.log`는 보통 `/var/log` 디렉터리 아래에 있다.
