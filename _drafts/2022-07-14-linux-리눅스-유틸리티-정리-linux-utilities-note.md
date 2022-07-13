---
layout: post
date: 2022-07-14 14:01:26 +0900
title: '[Linux] 리눅스 유틸리티 정리'
categories:
  - linux
tags:
  - linux
  - os
  - utility
  - service
  - note
---

* Kramdown table of contents
{:toc .toc}


#### 버전 정보

- 대부분 Ubuntu 20.x 기준으로 작성됨

## 개요

리눅스 서버에서 자주 설치해서 쓰는 유틸리티 적어둠.

## logrotate

로그 파일을 관리해주는 명령어...는 아니고 별도로 설치해야 할 수도 있는 시스템 유틸리티. 일정 규칙에 따라 파일을 압축/삭제/이름변경 등을 자동으로 수행하도록 할 수 있음. 예를 들어 매일 자정마다 특정 파일을 압축하고 지운다던지...

`logrotate.timer`를 서비스로 등록해 자동 실행되도록 하는 식으로 사용한다. 기본 설정 파일 경로는 `/etc/logrotate.conf`, 각 로그파일 별 설정은 `/etc/logrotate.d` 디렉터리 아래에 생성한다. 예를 들면 톰캣 로그 파일의 로테이션은 다음처럼 작성할 수 있다:

```
/svc/tomcat/logs/catalina.out{
 copytruncate
 daily
 rotate 14
 compress
 missingok
 notifempty
 dateext
}
```

## ntp

Network Time Protocol, 네트워크로 서버 시간을 조정하는 시스템 유틸리티. 우분투는 apt로 설치한다.

설치 후 설정파일 `/etc/ntp.conf` 에서 서버 풀(혹은 그냥 단독으로 하나)을 원하는 타임 서버로 지정하면 된다.

```bash
# 서울 시간을 제공하는 타임 서버와 동기화
server xxx iburst
```

`xxx`에 서버 아이피 혹은 도메인을 넣어주면 된다.

그 다음 서비스를 재시작하면 KST로 조회돼야 하는데:

```bash
$ date
Thu Jul 14 05:07:22 UTC 2022
```

만약 안된다면 심볼릭링크 `/etc/localtime`의 대상을 `/usr/share/zoneinfo/Asia/Seoul`로 변경한다.

```bash
$ ll /etc/localtime
$ rm /etc/localtime
$ ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime
```
