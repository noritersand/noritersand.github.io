---
layout: post
date: 2018-03-19 18:27:24 +0900
title: '[Windows] 파워셸에서 SSH를: OpenSSH'
categories:
  - windows
tags:
  - windows
  - ssh
  - terminal
  - powershell
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [\[Microsoft\] Windows의 OpenSSH](https://docs.microsoft.com/ko-kr/windows-server/administration/openssh/openssh_overview)
- [\[Microsoft\] OpenSSH 설치](https://docs.microsoft.com/ko-kr/windows-server/administration/openssh/openssh_install_firstuse)
- [\[Microsoft\] OpenSSH 키 관리](https://docs.microsoft.com/ko-kr/windows-server/administration/openssh/openssh_keymanagement)


## 개요

2018년에 윈도우 정식 기능으로 포함된 오픈소스인 OpenSSH 관련 간단 정리글. OpenSSH는 하위 기능으로 `ssh`, `sftp`, `ssh-keygen` 등을 포함하고 있다.


## ssh

putty는 안녕. 터미널은 이제 파워셸로 접속한다. ~~사실 WSL이 더 편하다~~

우선 설치를 하자.

#### GUI

윈도우 설정 > 앱 > 선택적 기능 > 선택적 기능 추가 > 'OpenSSH 클라이언트' 검색해서 설치

#### CLI

파워셸(관리자 권한)에서 다음 줄 실행:

```bash
# OpenSSH 설치
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'
```

클라이언트 기능을 활성화하고:

```bash
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

접속한다:

```bash
# 접속
ssh username@servername

# servername 서버에 22번 포트로 접속
ssh username@servername -p 22
```

만약 SSH 서버를 구성하려면 아래 명령어로 서버 기능을 활성화하면 되는데:

```bash
# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

나머지 설명은 [도움말](https://docs.microsoft.com/ko-kr/windows-server/administration/openssh/openssh_install_firstuse)을 보자.


## 개인키로 SSH 접속

키는 어떻게 만들었다 치고, 아래처럼 한다

```bash
ssh -i PRIVATE_KEY_FILE.pem ubuntu@101.202.303.404
```

이 때 "UNPROTECTED PRIVATE KEY FILE!" 경고가 출력되면서 접속이 안될 수 있는데, 이 때는 해당 파일의 권한을 아래 둘 중 하나의 방법으로 조정해주면 됨.

#### GUI

1. 탐색기에서 파일 속성 > 보안 > 고급
2. 상속 사용 안 함 > "이 개체에서 상속된 사용 권한을 모두 제거합니다."
3. 추가 > 보안 주체 선택 > 윈도우 계정명 적고 확인
4. 기본 권한 중 '읽기 및 실행', '읽기'만 체크
5. 끟

#### CLI

파워셸에서 아래 스크립트 실행:

```bash
# Set Key File Variable:
New-Variable -Name Key -Value "./PRIVATE_KEY_FILE.pem"

# Remove Inheritance:
Icacls $Key /c /t /Inheritance:d

# Set Ownership to Owner:
  # Key's within $env:UserProfile:
  Icacls $Key /c /t /Grant ${env:UserName}:F

  # Key's outside of $env:UserProfile:
  TakeOwn /F $Key
  Icacls $Key /c /t /Grant:r ${env:UserName}:F

# Remove All Users, except for Owner:
Icacls $Key /c /t /Remove:g Administrator "Authenticated Users" BUILTIN\Administrators BUILTIN Everyone System Users

# Verify:
Icacls $Key

# Remove Variable:
Remove-Variable -Name Key
```

스크립트 출처: [https://superuser.com/questions/1296024/windows-ssh-permissions-for-private-key-are-too-open](https://superuser.com/questions/1296024/windows-ssh-permissions-for-private-key-are-too-open)


## ssh-keygen

```bash
ssh-keygen
```

RSA 키 페어를 생성하는 명령어. 명령 실행 시 이름과 비밀번호를 묻는 프롬프트가 나타나며, 입력을 마치면 현재 경로에 공개키와 비공개키 하나씩 생성된다. 생성 단계에서 묻는 비밀번호는 2단계 인증용 비밀번호이며 입력하지 않아도 된다.

```bash
PS C:\Users\user\.ssh> ssh-keygen

Generating public/private rsa key pair.
Enter file in which to save the key (C:\Users\user/.ssh/id_rsa): noritersand-test
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in noritersand-test.
Your public key has been saved in noritersand-test.pub.
The key fingerprint is:
SHA256:uDDyhvysiAgiFBxuX47fZ+P3drfdtn3Ws8eFXTIkl/k user@noritersand-desktop
The key\'s randomart image is:
+---[RSA 2048]----+
| .             o |
|o .         . =  |
| =   .       + . |
|. o +  .      o E|
| ..oo.. S      =.|
|.. +.o..      . o|
|+ o o... +     .o|
|*. +    + .. . o%|
|+ ..o    .. o..*%|
+----[SHA256]-----+

PS C:\Users\user\.ssh> ls

    디렉터리: C:\Users\user\.ssh

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----     2021-02-02  오후 12:48           1766 noritersand-test
-a----     2021-02-02  오후 12:48            403 noritersand-test.pub
```


## ssh-agent, ssh-add

비공개키를 관리하는 서비스와 명령어.

```bash
# Make sure you're running as an Administrator
Start-Service ssh-agent

# This should return a status of Running
Get-Service ssh-agent

# Now load your key files into ssh-agent
ssh-add ~\.ssh\noritersand-ssh-test
```

이렇게 추가한 비공개키는 Window 보안 컨텍스트에 저장된다고 한다. 마소는 이 작업 후 로컬 시스템에서 비공개키를 삭제하길 권장하고 있다.


## sftp

OpenSSH 설치하면 `sftp`도 쓸 수 있음.

#### 다운로드

```bash
# secure shell 인증은 PRIVATE_KEY_FILE.pem으로 하고, 101.202.303.404 서버에서 ubuntu 유저의 홈경로/DOWNLOAD_ME.md를 다운로드
sftp -i .\PRIVATE_KEY_FILE.pem ubuntu@101.202.303.404:SOME_DIRECTORY/DOWNLOAD_ME.md $env:userprofile\Downloads
```

#### 업로드: 대화형으로 put

```bash
PS> sftp -i .\PRIVATE_KEY_FILE.pem ubuntu@101.202.303.404
Connected to 101.202.303.404.

sftp> cd temp

sftp> put ./upload.me
Uploading ./upload.me to /home/ubuntu/temp/upload.me
./upload.me                                        100%   14     0.2KB/s   00:00

sftp> ls
upload.me

sftp> quit
```

파워셸에서 명렁어 한 줄로 업로드하는 건 못찾음. WSL에선 [여기](https://stackoverflow.com/questions/16721891/single-line-sftp-from-terminal) 보면 됨.


## scp

**TODO OpenSSH 설치하면 되는지 확인**

**TODO scp로 다운로드는 안되나**

#### 업로드

```
scp [options] source target
```

```bash
scp -i .\PRIVATE_KEY_FILE.pem .\upload.me ubuntu@101.202.303.404:/home/ubuntu/temp/upload.me
```
