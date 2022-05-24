---
layout: post
date: 2022-05-24 13:28:52 +0900
title: '[misc] 썬더버드 IMAP/SMTP 설정 모음'
categories:
  - misc
tags:
  - thunderbird
---

* Kramdown table of contents
{:toc .toc}

#### 버전 정보

- Thunderbird 91

## Outlook

### IMAP

- server: outlook.office365.com
- port: 993
- 보안 연결: SSL/TLS
- 인증 방식: 암호화된 비밀번호

### SMTP

- server: smtp.office365.com
- port: 587
- 보안 연결: STARTTLS
- 인증 방식: 암호화된 비밀번호

만약 2FA 사용 중이면 보내기가 실패할 것이다. '평문 비밀번호' 인증 방식으로 변경한 뒤 메일 보내기를 다시 하면 로그인 실패 메시지가 뜰텐데, 여기에 `MS 웹 > 내 계정 페이지 > 보안 > 새로운 앱 암호`에서 생성된 비밀번호를 입력한다.
