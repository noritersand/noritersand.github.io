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

MIME 타입은 원래 이메일에서 사용하던 일종의 인코딩 규칙이다. 보통 브라우저나 OS 파일 확장자-MIME 타입 매핑 테이블을 사용해 MIME 타입을 판단한다.

MIME 타입은 HTTP에서도 그대로 사용되는데, HTTP 헤더 중 `Content-Type` 필드로 타입을 지정하거나 식별한다. `Content-Type` 헤더가 없거나 `application/octet-stream` 처럼 너무 범용적인 타입이 전송되면 브라우저(혹은 서버)는 데이터의 내용(바이너리 시그니처나 파일의 앞부분)을 분석해서 데이터 형식을 추축하려고 시도하는데, 이걸 *MIME 타입 스니핑*이라고 한다.

모든 텍스트 기반 파일은 `text/plain`이 기본값이며, 이외에는 모두 `application/octet-stream`이 기본값이다. 

ℹ️ `application/octet-stream`은 알려지지 않은 바이너리 데이터의 기본 타입이기도 하다. misc 같은 늭김.


## 주요 MIME 타입 목록

- `.txt`: `text/plain`
- `.csv`: `text/csv`
- `.pdf`: `application/pdf`
- `.jpg` `.jpeg`: `image/jpeg`
- `.gif`: `image/gif`
- `.png`: `image/png`
- `.webp`: `image/webp`
- `.bmp`: `image/bmp`
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


## 특이케이스

한글(`.hwp`) 문서는 다음 중 하나다~~엠뵹~~:

- `application/x-hwp`
- `application/haansofthwp`
- `application/haansofthwpx`
- `application/vnd.hancom.hwp`
- `application/vnd.hancom.hwpx`
