---
layout: post
date: 2023-07-31 15:01:31 +0900
title: '[WEB] MIME 타입'
categories:
  - web
tags:
  - web-api
  - mime-types
  - content-type
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서


- [https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types)
- [https://www.iana.org/assignments/media-types/media-types.xhtml](https://www.iana.org/assignments/media-types/media-types.xhtml)
- [https://www.rfc-editor.org/rfc/rfc2046.html](https://www.rfc-editor.org/rfc/rfc2046.html)

#### 버전 정보

- x.x.x


## 개요

MIME 타입은 원래 이메일에서 사용하던 일종의 인코딩 규칙인데 HTTP에서도 그대로 사용함.

HTTP에선 `Content-Type`이란 이름으로 사용한다. (검증 필요)


## 제목

```
type/subtype
```

TODO

가능한 타입은 [여기](https://www.iana.org/assignments/media-types/media-types.xhtml#text)를 보자.


## 주로 사용되는 MIME 타입 목록

- 텍스트 관련 타입:
  - 'text/plain': 일반 텍스트 데이터
  - 'text/html': HTML 데이터
  - 'text/xml': XML 데이터
  - 'text/css': CSS 스타일시트 데이터
- 이미지 관련 타입:
  - 'image/jpeg': JPEG 이미지
  - 'image/png': PNG 이미지
  - 'image/gif': GIF 이미지
  - 'image/bmp': BMP 이미지
- 오디오/비디오 관련 타입:
  - 'audio/mpeg': MP3 오디오
  - 'audio/wav': WAV 오디오
  - 'video/mp4': MP4 비디오
- 기타:
  - 'application/json': JSON 데이터
  - 'application/pdf': PDF 문서를 나타냅니다.
  - 'application/zip': ZIP 압축 파일을 나타냅니다.
  - 'application/octet-stream': 일반적으로 알려지지 않은 바이너리 데이터의 기본 타입이라고 한다. misc 같은 늭김.
