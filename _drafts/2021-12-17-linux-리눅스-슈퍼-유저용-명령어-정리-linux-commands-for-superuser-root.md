---
layout: post
date: 2021-12-17 14:55:05 +0900
title: '[Linux] 리눅스 슈퍼 유저용 명령어 정리'
categories:
  - linux
tags:
  - linux
  - os
  - root
  - sudo
  - commands
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [Official Ubuntu Documentation](https://help.ubuntu.com/)

#### 버전 정보

- 대부분의 명령은 Ubuntu 20.x 기준으로 작성됨

## 개요

리눅스 명령어 중 루트 권한이 필요하거나 시스템 관리와 관련된 명령어/기능만 정리한 글.

## sudo

명령을 루트(슈퍼 유저) 권한으로 실행하는 키워드

```bash
sudo vi httpd.conf  # 루트권한으로 편집기 실행
```

가령 수정권한이 부족한 디렉터리를 삭제하려는 경우:

```bash
sudo rm remove-me -r
```

라고 치면 루트 비번 입력으로 임시 권한을 획득하여 삭제할 수 있게 된다.

아예 루트로 유저를 전환하려면:

```bash
# 루트 유저로 환경 변수 등의 설정까지 모두 전환
sudo su -

# 유저의 환경을 유지하면서 루트 유저로 전환
sudo su
```

## 설정 파일 위치

### alias

일반적으로

- 유저 `/home/user/.bashrc`
- 루트 `/etc/rc.d/rc.local`

에 작성. 단, 위처럼 작성 할 경우 유저계정과 루트계정이 서로 독립적인 설정을 갖는다.

[지역에 관한 내용 링크](http://originalchoi.tistory.com/19)

### 유저, 그룹

보통 유저는 `/etc/passwd`, 그룹은 `/etc/group` 파일에 텍스트로 나열돼 있음.

## 바라보는 DNS 서버 바꾸기

- [https://unix.stackexchange.com/questions/128220/how-do-i-set-my-dns-when-resolv-conf-is-being-overwritten](https://unix.stackexchange.com/questions/128220/how-do-i-set-my-dns-when-resolv-conf-is-being-overwritten)
- [https://unix.stackexchange.com/questions/146463/specifying-dns-settings-to-override-those-of-dhcp](https://unix.stackexchange.com/questions/146463/specifying-dns-settings-to-override-those-of-dhcp)

대충 검색해보면 몇 가지 방법이 있는데 환경에 따라 적용 여부가 달라지는 것 같다. `/etc/resolvconf/resolv.conf.d/head` 혹은 `/etc/resolvconf/resolv.conf.d/base`를 수정해서 `/etc/resolvconf/resolv.conf`가 바뀌도록 하라는 말도 있고, `/etc/dhcp/dhclient.conf`를 수정하라는 말도 있다.

`/etc/dhcp/dhclient.conf`를 수정한다고 하면:

```bash
prepend domain-name-servers 1.2.3.4;
append domain-name-servers 1.2.3.4;
supersede domain-name-servers 1.2.3.4;
```

우선순위에 따라 요 셋 중에 골라서 하면 되는걸로 보임.

## chmod

![](/images/chmod-chown-umask-image.png)

```bash
chmod 700 file  # 오너(owner, 파일을 만든 사람)에게만 모든 권한 부여
chmod 770 file  # 오너와 오너가 속한 그룹에 모든 권한 부여
chmod 777 file  # 오너와 오너의 그룹, 그 외 모든 유저에게 모든 권한 부여
```

#### options

- `-R` `--recursive`: 재귀 실행
- `-c` `--changes`: 변경된 내용만 출력
- `-f` `--silent` `--quiet`: 에러 메시지를 출력하지 않음
- `-v` `--verbose`: 모든 메시지를 출력함

아래 `chown`도 같은 옵션을 사용함.

## chown

```bash
chown 소유권자:그룹식별자 대상

# SOME_DIRECTORY와 그 하위 모든 파일에 USER_NAME 소유와 GROUP_NAME 그룹을 지정
chown -R USER_NAME:GROUP_NAME ./SOME_DIRECTORY
```

## apt

APT, Advanced Packaging Tool

- [메뉴얼: apt](https://manpages.ubuntu.com/manpages/xenial/man8/apt.8.html)
- [메뉴얼: apt-get](https://manpages.ubuntu.com/manpages/xenial/man8/apt-get.8.html)

Debian/Ubuntu 계열 리눅스의 패키지 설치 명령어.

`apt`는 [설치 경로를 지정할 수 없다고 한다](https://www.quora.com/How-do-I-choose-the-installation-path-for-apt-get).

```bash
apt install PACKAGE_NAME

# 설치된 패키지 목록 표시
apt list --installed

# 업그레이드 가능한 패키지 표시
apt list --upgradeable

# PACKAGE_NAME이 사용하는 로컬 저장소를 삭제한다. 쉽게 말해 캐시를 지운다 보면 됨. (잠금 파일은 제외)
apt clean PACKAGE_NAME

# clean과 비슷하지만, 더 이상 다운로드 불가능하고 대강 봐서 쓸모 없는 것만 지운다고 함
apt autoclean PACKAGE_NAME
```

### apt와 apt-get의 차이

https://askubuntu.com/questions/445384/what-is-the-difference-between-apt-and-apt-get

`apt-get`도 있는데 검색해보니 대충 `apt`가 최종 사용자용이라고 한다. 그리고 별 차이도 없다 하니 그냥 `apt`만 쳐도 된다.

### apt 저장소 업데이트

https://stackoverflow.com/questions/29929534/docker-error-unable-to-locate-package-git

만약 패키지를 찾을 수 없다는 메시지(Unable to locate package)가 나오면 apt 저장소를 업데이트를 해보자:

```bash
apt update
```

### apt로 패키지 삭제

```bash
# PACKAGE_NAME 삭제
apt remove PACKAGE_NAME

# PACKAGE_NAME과 종속 패키지들 모두 삭제
apt autoremove PACKAGE_NAME
apt remove PACKAGE_NAME --auto-remove
```

패키지 삭제 명령은 `remove`, `autoremove`, `purge` 세 개가 있다.

`remove`와 `autoremove`는 단순히 패키지만 삭제한다. 이 경우 설정 파일 등의 잔여물이 남을 수 있다. 반면 `purge`는 설정 파일을 포함해 모두 삭제한다고 한다.

> apt remove just removes the binaries of a package. It leaves residue configuration files.  
> apt purge removes everything related to a package including the configuration files.  
> 출처: https://itsfoss.com/apt-command-guide/

따라서 깨끗한 삭제를 하려면 `clean` 후:

```bash
apt purge --auto-remove PACKAGE_NAME
```

하면 됨.

## umask

TODO

## df

```bash
df -h # 현재 시스템 파티션별 하드디스크 용량을 %및 Byte로 표시
```

#### options

- `-h`: 크기를 KB, MB, GM 으로 자동 전환하여 표시
- `-i`: 용량 대신 inode 조회 (inode는 유닉스 파일시스템 내부에서 사용되는 파일의 식별번호다.)

## 메모리

```bash
cat /proc/meminfo | grep MemTotal
```

```bash
top -p 12345 # 12345 프로세스에 대한 CPU/메모리 점유율 확인
```

## 디스크

```bash
free
```

## audit 디스크 액세스 관리

- [https://linux.die.net/man/8/auditctl](https://linux.die.net/man/8/auditctl)
- [https://www.lesstif.com/pages/viewpage.action?pageId=20776444](https://www.lesstif.com/pages/viewpage.action?pageId=20776444)
- [http://amerika.tistory.com/entry/audit설정을-통합-리눅스-파일-보안강화](http://amerika.tistory.com/entry/audit%EC%84%A4%EC%A0%95%EC%9D%84-%ED%86%B5%ED%95%9C-%EB%A6%AC%EB%88%85%EC%8A%A4-%ED%8C%8C%EC%9D%BC-%EB%B3%B4%EC%95%88%EA%B0%95%ED%99%94)
- [https://techintguru.blogspot.kr/2017/04/audit.html](https://techintguru.blogspot.kr/2017/04/audit.html)

여러가지 할 수 있는걸로 보이는데, 일단 아는건 파일 시스템의 변화를 감지하고 로그를 남길 수 있음.

## 기본 셸 변경

```bash
# 로그인 셸을 Bash로 바꾸기
chsh -s $(which bash)

# 로그인 셸을 zsh로 바꾸기
chsh -s $(which zsh)


# 확인#1
echo $SHELL

# 확인#2
env | grep SHELL
```

만약에 안되면 `/etc/passwd` 파일을 직접 수정해도 됨.

## iptables/ip6tables

리눅스 커널(kernel)에 있는 IP 패킷 필터링 규칙들의 집합인 IP 테이블을 관리하는 명령어.

```bash
iptables -A FORWARD -m account --aname mynetwork --aaddr 192.168.0.0/24

iptables -A INPUT -p tcp --dport 80 -m account --aname mywwwserver --aaddr 192.168.0.0/24 --ashort

iptables -A OUTPUT -p tcp --sport 80 -m account --aname mywwwserver --aaddr 192.168.0.0/24 --ashort
```

## ufw

UFW(Uncomplicated FireWall) 방화벽 설정. 기본 정책은 아웃바운드를 모두 허용, 인바운드를 모두 차단한다고 한다. 관련 설정 파일은 `/etc/ufw` 아래에 있다.

```bash
# 방화벽 활성화(켜기)
ufw enable

# 끄기
ufw disable

# 방화벽 상태 확인
ufw status

# ufw로 방화벽 허용/차단 가능한 앱 목록
ufw app list

# 'Apache'의 방화벽 허용 룰 추가
ufw allow 'Apache'

# 방화벽 다시 불러오기?(정확히 뭔지는 몲)
ufw reload
```

### 특정 포트 허용/차단

```bash
# TCP 443 포트 통신 허용 룰 추가
ufw allow 443/tcp

# UDP 67 포트 통신 차단 룰 추가
ufw deny 67/udp
```

### 룰 삭제

```bash
# TCP 443 포트 통신 허용룰 삭제
ufw delete allow 443/tcp

# ssh 차단 룰 삭제
ufw delete deny ssh
```

## sysctl

TODO 뭔가를 하는건데...
아래 `systemctl`의 약어인 것 같지만 전혀 아니다.

```bash
# read values from file. 현재 세션에 변경사항 반영인데... 새로고침 대상이 뭘까?
sysctl -p
```

## systemctl

[systemmd](https://www.google.com/search?client=firefox-b-d&q=systemmd란) 시스템과 서비스 매니저를 관리하는 명령어. 도움말에는 "쿼리나 컨트롤 명령을 시스템 매니저에 전송"한다고 나온다.

**리눅스 데몬을 관리**하는 명령어로 보인다. 도움말에서는 서비스나 데몬이 아닌 '유닛'이라는 단위를 쓴다.

```bash
# 시스템 매니저가 관리하는 모든 유닛 보기
systemctl list-units

# 위와 같음
systemctl

# 서비스만 보기
systemctl --type=service

# active 상태인 유닛만 보기
systemctl --state=active
```

```bash
# apache2 상태 확인
systemctl status apache2

# apache2 시작
systemctl start apache2

# UNIT_NAME 유닛 재시작
systemctl restart UNIT_NAME

# apache2 중지
systemctl stop apache2

# 유닛이 서버 재시작 후 자동으로 시작되게 함
# enable은 유닛 인스턴스를 활성화, symlink 생성한다.
systemctl enable UNIT_NAME

# 유닛의 자동 시작 끄기
systemctl disable UNIT_NAME

# 새로 고침 "파일 시스템에서 변경된 구성을 다시 가져와 종속성 트리를 재생성"
systemctl daemon-reload
```

#### 서비스 등록

어떤 프로세스를 서비스로 등록하려면 (우분투의 경우) `/usr/lib/systemd/system` 경로에 `서비스이름.service` 파일을 생성하고 리로드(`systemctl daemon-reload`)하면 된다.

서비스 파일 예시(`tomcat.service`):

```bash
[Unit]
Description=Tomcat
After=network.target syslog.target

[Service]
Type=forking

User=me
Group=mygroup

ExecStart=/svc/tomcat/bin/startup.sh
ExecStop=/svc/tomcat/bin/shutdown.sh

UMask=0007
RestartSec=10
Restart=always

SuccessExitStatus=143

[Install]
WantedBy=multi-user.target
```

## journalctl

`systemctl` 관련 로그 보는 명령인 것 같은디?

```bash
journalctl -xe

journalctl -fb -u SERVICE_NAME.SERVICE

journalctl -xfb -u codedeploy-agent.service
```

#### options

- `-u`: 특정 유닛의 로그만 보기
- `-x`: 가능한 경우 메시지 설명도 같이 출력
- `-e`: 로그 마지막으로 바로 이동
- `-b`: 현재 부트 혹은 지정된 부트의 로그만 보기
- `-f`: Follow the journal. 명령 실행을 끝내지 않고 새로운 로그를 계속 출력한다.

## service

```bash
# 모든 서비스 목록
service --status-all

# 서비스 상태 확인
service SERVICE_NAME status

# 서비스 재시작
service SERVICE_NAME restart
```

## useradd, userdel, passwd

유저 생성과 삭제, 비밀번호 설정.

```bash
# 홈 디렉터리와 함께 유저 생성
useradd -m USER_ID

# 유저 삭제
userdel USER_ID

# 유저의 로그인 비밀번호 설정
passwd USER_ID
```

루트 비번을 변경하려면 간단히 루트 권한을 획득한 상태에서 `root` ID의 비밀번호를 변경하면 된다.

## usermod

유저의 로그인 정보를 수정하는 명령어

```bash
# GROUP_ID 그룹에 USER_ID 추가
usermod -a -G GROUP_ID USER_ID
```
