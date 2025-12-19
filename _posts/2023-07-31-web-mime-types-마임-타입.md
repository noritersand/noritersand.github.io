---
layout: post
date: 2023-07-31 15:01:31 +0900
title: '[Web] MIME 타입'
categories:
  - web
tags:
  - web-api
  - mime-types
  - content-type
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Media types (MIME types) - HTTP \| MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/MIME_types)
- [Common media types - HTTP \| MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/MIME_types/Common_types)
- [Media Types \| iana](https://www.iana.org/assignments/media-types/media-types.xhtml)
- [RFC 2046: Multipurpose Internet Mail Extensions (MIME) Part Two: Media Types](https://www.rfc-editor.org/rfc/rfc2046.html)


## 개요

```
type/subtype
```

MIME(Multipurpose Internet Mail Extensions)은 원래 이메일 확장을 위해 만들어진 표준이다. 초기 SMTP는 ASCII 문자만 전송할 수 있었는데, MIME 표준과 Base64 같은 인코딩 기법이 도입되면서 텍스트 이외의 데이터도 SMTP 환경에서 전송할 수 있게 됐다. MIME은 여러 새로운 헤더 필드를 정의했는데, 그중 하나가 `Content-Type`이며 이 값을 **MIME 타입**이라고 한다.

MIME 타입은 HTTP에서도 그대로 사용된다. HTTP 응답과 요청에서 `Content-Type` 헤더를 통해 데이터 형식을 지정하거나 식별한다. 만약 `Content-Type`이 없거나 `application/octet-stream`처럼 지나치게 범용적인 타입이 들어오면 브라우저(혹은 서버)는 바이너리 시그니처나 앞부분을 검사해 형식을 추론하려고 시도하는데, 이를 **MIME 타입 스니핑**이라 한다.

MIME 타입은 웹이 아닌 곳에서도 사용된다. 예를 들어 운영체제는 보통 '파일 확장자 - MIME 타입 매핑 테이블'을 통해 파일의 유형을 식별하고, 브라우저도 파일 업로드 시 같은 방식으로 MIME 타입을 결정한다.


## 주의사항

- MIME 타입 스니핑은 XSS 공격에 활용될 수 있기 때문에 `X-Content-Type-Options: nosniff` 헤더로 비활성화하는 게 일반적이다.
- MIME 타입은 조작 가능하기 때문에, 서버는 클라이언트가 전송한 MIME 타입 외에도 파일 내용 기반의 검증을 반드시 추가해야 한다. (확장자 whitelist, Magic Number나 바이너리 시그니처 검사)
- 서버는 별도의 매핑 파일을 통해 MIME 타입을 결정한다. (nginx는 `mime.types`, Apache는 `.htaccess` 혹은 `mime.types`)
- 텍스트 콘텐츠는 `text/plain; charset=utf-8`처럼 케릭터 셋을 명시하는 게 안전하다.

ℹ️ MIME 타입 스니핑은 막으라면서 서버에선 내용 기반 검사를 하라니... 모순처럼 보일 수 있는데, 브라우저 스니핑은 악성 코드 렌더링이나 자동 실행을 막기 위해 비활성화해야 하는 기능이고, 서버의 내용 기반 검사는 MIME 타입과 실제 파일 내용이 일치하는지 확인하기 위한 검증 단계일 뿐이다. 목적이 다르다.


## 기본값

대부분의 환경에서 알 수 없는 텍스트는 기본적으로 `text/plain`, 알 수 없는 바이너리는 `application/octet-stream`으로 매핑된다. `application/octet-stream`은 '정체 불명의 바이너리 blob'의 기본 타입으로 취급한다. misc 같은 늭김.


## 파일 다운로드 유도하기

`Content-Type`이 `application/octet-stream`이면 대부분의 브라우저는 콘텐츠를 직접 렌더링하지 않고 파일 다운로드를 시도한다. 그래서 의도적으로 이 타입을 쓰기도 하며, 이 경우 `Content-Disposition: attachment; filename="name.ext"` 헤더도 추가하면 좋다.

ℹ️ 다운로드를 유도할 때, 파일 이름에 non-ASCII 문자(한글, 이모지 등)가 들어가면 `filename*=UTF-8''%ED%8C%8C%EC%9D%BC.ext` 형식(RFC 5987)으로 인코딩해야 파일명이 깨지지 않는다.

⚠️ 사용자가 업로드하거나 생성한 파일(user-generated content)은 직접 노출하지 말고, 다운로드 전에 확장자 제한, 매직 넘버 검사 등으로 파일 유형을 검증해야 한다. 또한 브라우저의 MIME 스니핑을 막기 위해 `X-Content-Type-Options: nosniff` 헤더를 설정하는 게 안전하다.


## 주요 MIME 타입 목록

- `.txt`: `text/plain`
- `.csv`: `text/csv`
- `.pdf`: `application/pdf`
- `.jpg` `.jpeg`: `image/jpeg`
- `.gif`: `image/gif`
- `.png`: `image/png`
- `.webp`: `image/webp`
- `.bmp`: `image/bmp`, `image/x-ms-bmp`
- `.zip`: `application/zip`
- `.doc`: `application/msword`
- `.docx`: `application/vnd.openxmlformats-officedocument.wordprocessingml.document`
- `.xls`: `application/vnd.ms-excel`
- `.xlsx`: `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`
- `.ppt`: `application/vnd.ms-powerpoint`
- `.pptx`: `application/vnd.openxmlformats-officedocument.presentationml.presentation`
- `.htm` `.html`: `text/html`
- `.xml`: 표준은 `application/xml`인데, 간혹 `text/xml`이 쓰이는 경우가 있음
- `.css`: `text/css`
- `.mp3`: `audio/mpeg`
- `.mp4`: `video/mp4`
- `.mpeg`: `video/mpeg`
- `.wav`: `audio/wav`
- `.json`: `application/json`
- `.bin`: `application/octet-stream`

### 특이케이스

한글(`.hwp`) 문서는 다음 중 하나다~~엠뵹~~:

- `application/x-hwp`
- `application/haansofthwp`
- `application/haansofthwpx`
- `application/vnd.hancom.hwp`
- `application/vnd.hancom.hwpx`
