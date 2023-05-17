---
layout: post
date: 2018-07-06 15:53:54 +0900
title: '[Windows] pscp 원격 서버에 파일 업로드'
categories:
  - windows
tags:
  - os
  - windows
  - pscp
---


## PSCP, PuTTY Secure Copy client

윈도우에서는 putty 인스톨 버전(pscp가 포함되어 있음)을 설치하거나 pscp.exe를 직접 다운로드 받아서 명령 프롬프트나 파워셸에서 사용할 수 있다.
[putty 다운로드](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

**TODO OpenSSH만 설치하면 scp도 되는지 확인**

**TODO `_posts\2018-03-19-windows-파워셸에서-ssh를-openssh.md` 파일에 합쳐**

```bash
pscp 보낼파일 사용자@호스트:목적지경로
```

포트번호가 22번이 아니면 `-P` 옵션으로 포트번호를 지정한다.
```bash
pscp -P 포트번호 보낼파일 사용자@호스트:목적지경로
```

`-pw` 옵션으로 비밀번호를 미리 입력할 수 있다.
```bash
pscp -pw 비밀번호 보낼파일 사용자@호스트:목적지경로
```

와일드카드 `*`를 이용해서 폴더까지 업로드하고 싶으면 `-r` 옵션을 사용한다.

### example

```bash
pscp C:/project/workspace/myfile.js someuser@12.34.56.78:/test/myfile.js # 비밀번호 별도 입력
pscp -pw P@ssWord C:/project/workspace/myfile.js someuser@12.34.56.78:/test/myfile.js # 비밀번호도 같이
pscp -r -pw P@ssWord C:/project/workspace/some-directory/* someuser@12.34.56.78:/test-directory # 비밀번호를 미리 입력하며 폴더까지 재귀업로드
```
