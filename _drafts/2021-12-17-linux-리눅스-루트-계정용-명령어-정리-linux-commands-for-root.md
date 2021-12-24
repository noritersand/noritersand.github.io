---
layout: post
date: 2021-12-17 14:55:05 +0900
title: '[Linux] 리눅스 루트 계정용 명령어 정리 Linux Commands for root'
categories:
  - linux
tags:
  - linux
  - os
  - root
  - sudo
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [Official Ubuntu Documentation](https://help.ubuntu.com/)

#### 버전 정보

- 대부분의 명령은 Ubuntu 20.x 기준으로 작성됨

리눅스 명령어 중 루트 권한이 필요하거나 시스템 관리와 관련된 명령어/기능만 골라 정리한 글.

## sudo

명령을 루트(슈퍼유저) 권한으로 실행하는 키워드

```bash
sudo vi httpd.conf  # 루트권한으로 편집기 실행
```

가령 수정권한이 부족한 디렉터리를 삭제하려는 경우:

```bash
sudo rm remove-me -r
```

라고 치면 루트 비번 입력으로 임시 권한을 획득하여 삭제할 수 있게 된다.

아예 루트로 사용자를 전환하려면:

```bash
# 루트 사용자로 환경변수 등의 설정까지 모두 전환
sudo su -

# 이것로 루트 로그인인데 뭔 차이야...
su -  
```

## 설정 파일 위치

### alias

일반적으로

- 유저 `/home/user/.bashrc`
- 루트 `/etc/rc.d/rc.local`

에 작성. 단, 위처럼 작성 할 경우 유저계정과 루트계정이 서로 독립적인 설정을 갖는다.

[지역에 관한 내용 링크](http://originalchoi.tistory.com/19)

### 사용자, 그룹

보통 사용자는 `/etc/passwd`, 그룹은 `/etc/group` 파일에 텍스트로 나열돼 있음.

## chmod

![](/images/chmod-chown-umask-image.png)

[이미지 출처](http://endlessgeek.com/2014/02/chmod-explained-linux-file-permissions)

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

## APT<sup>Advanced Packaging Tool</sup>

Debian/Ubuntu 계열 리눅스의 패키지 설치 명령어.

```bash
sudo apt install nodejs
```

`apt`는 [설치 경로를 지정할 수 없다고 한다](https://www.quora.com/How-do-I-choose-the-installation-path-for-apt-get).

`apt-get`도 있는데 [검색해보니](https://askubuntu.com/questions/445384/what-is-the-difference-between-apt-and-apt-get) 대충 `apt`가 최종 사용자용이라고 한다. 그리고 별 차이도 없다 하니 그냥 `apt`만 쳐도 된다.

만약 패키지를 찾을 수 없다는 메시지(Unable to locate package)가 나오면 [apt 저장소를 업데이트](https://stackoverflow.com/questions/29929534/docker-error-unable-to-locate-package-git)를 해보자:

```bash
apt update
```

## whereis

설치된 패키지의 파일들이 어디어디 있는지 알려준다.

```bash
whereis nodejs
```

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

## 기본 쉘 변경

```bash
# 로그인 쉘을 sh로 바꾸기
chsh -s $(which sh)

# 로그인 쉘을 Bash로 바꾸기
chsh -s $(which bash)

# 확인#1
echo $SHELL

# 확인#2
env | grep SHELL
```

만약에 안되면 `/etc/passwd` 파일을 직접 수정해도 됨.

## UFW<sup>Uncomplicated FireWall</sup>

방화벽 설정. 기본 정책은 아웃바운드는 모두 허용, 인바운드는 모두 차단이라고 한다.

TODO 추가 설명 필요

예시:

```bash
# 설정 가능한 앱 확인
sudo ufw app list

# 'Apache'의 방화벽 허용
sudo ufw allow 'Apache'

# 443 포트 TCP 통신 허용
sudo ufw allow 443/tcp

# ufw 활성화
sudo ufw enable

# 확인
sudo ufw status


```

## sysctl

TODO 뭔가를 하는건데...
아래 `systemctl`의 약어인 것 같지만 전혀 아니다.

```bash
# read values from file
sudo sysctl -p
```

## systemctl

도움말에는 "쿼리나 컨트롤 명령을 시스템 매니저에 전송"한다고 나온다.

대충 패키지로 설치된 앱에 시스템 매니저를 통한 명령 실행인 것 같음.

```bash
# apache2 상태 확인
systemctl status apache2

# apache2 시작
systemctl start apache2

# apache2 중지
systemctl stop apache2

# 유닛 인스턴스를 활성화, symlink 생성.
systemctl enable openvpn@server
```

`enable`하면 서버가 리부트돼도 자동 시작한다고 함.

## service

서비스 관리
