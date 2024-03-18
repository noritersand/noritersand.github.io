---
layout: post
date: 2024-02-16 14:50:30 +0900
title: '[Web] CSP, Content Security Policy'
categories:
  - categorize-me
tags:
  - draft-permanantly
  - tag-me
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [MDN \| Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
- [MDN \| HTTP Headers: Content-Security-Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy)


## 개요

우리 말로는 컨텐츠 보안 정책. XSS 같은 웹 보안 취약점 공격을 탐지하고 완화(mitigate)하기 위해 사용되는 추가적인 보안 계층이다. CSP를 통해 웹 페이지에서 유효한 리소스(스크립트, CSS, 이미지, 폰트 등)의 정책을 명시하고, 브라우저는 이 정책에 따라 리소스를 허용하거나 차단하는 식으로 작동한다.

이 기술이 모질라 재단에 의해 소개된 것은 2004년이지만, CSP 1.0의 첫 공개 초안은 2012년이나 되어서야 발표되었다.

```bash
# 현재 도메인과 https://trusted.com에서 불러온 스크립트만 실행을 허용한다
Content-Security-Policy: script-src 'self' https://trusted.com;

# 현재 도메인에서 불러온 스크립트와 인라인으로 작성된 스크립트, CSS 실행을 허용한다
Content-Security-Policy: script-src 'self' 'unsafe-inline';
```

CSP는 HTTP 헤더 `Content-Security-Policy`를 통해 구현되며, 보통 웹 서버에서 설정한다. 구 버전에선 `X-Content-Security-Policy`라는 이름으로 쓰였다.

```html
<meta http-equiv="Content-Security-Policy"
    content="default-src 'self'; img-src https://*; child-src 'none';" />
```

다른 방법으로 `<meta>` 태그를 통해서 정책을 설정할 수 있지만, 서버 수준의 구현과 함께 고려되는 것이 권장된다.


## 정책 지정하기

```
Content-Security-Policy: directive <value>;
Content-Security-Policy: directive <value>; directive <value>; ...
Content-Security-Policy: directive <value> <value> ...;
Content-Security-Policy: directive <value> ...; directive <value> ...; ...
```

### Directive

디렉티브는 출처를 제한할 리소스의 유형을 나타내는 항목이다. 한 번에 여러 디렉티브를 지정할 수 있고, 디렉티브 사이의 구분은 세미콜론`;`으로 한다.

현재(2024-02-16) 디렉티브는 총 29개(...)가 있고, 주요 디렉티브는 다음과 같다.

fetch 디렉티브:

- `default-src`: 명시하지 않은 모든 리소스에 대한 출처
- `child-src`: 웹 워커와 `<frame>`, `<iframe>`에 대한 출처
- `connect-src`: XMLHttpRequest, WebSocket, Fetch 등의 출처
- `font-src`: 폰트 파일의 출처
- `frame-src`: `<frame>` 및 `<iframe>` 태그를 통해 불러올 수 있는 출처
- `img-src`: 이미지 파일 출처
- `manifest-src`: 매니페스트 파일의 출처
- `media-src`: 오디오 및 비디오 파일을 로드할 수 있는 출처
- `object-src`: `<object>`, `<embed>`, `<applet>` 태그를 통해 불러올 수 있는 리소스의 출처
- `script-src`: 자바스크립트에 대한 검증된 출처
- `style-src`: 스타일시트 출처
- `worker-src`: `Worker`, `SharedWorker`, `ServiceWorker` 스크립트를 불러올 수 있는 출처

...를 제한하거나 지정하는 디렉티브다.

문서 디렉티브:

- `base-uri`: `<base>` 태그에서 사용할 수 있는 URL을 제한한다.

탐색 디렉티브:

- `form-action`: `<form>`의 제출(submissions) 대상을 제한하는 지시어
- `frame-ancestors`: 이 페이지를 포함할 수 있는 상위 항목(`<frame>`, `<iframe>`, `<object>`, `<embed>`)을 지정한다.

보고 정책 디렉티브:

- `report-uri`: CSP 위반 보고를 전송할 URL을 지정한다. 이 항목은 deprecated 상태고, `report-to`가 권장된다.
- `report-to`: CSP 위반 보고를 전송할 그룹을 지정한다.

기타 디렉티브:

- `upgrade-insecure-requests`: **TODO**

### Value

리소스의 출처가 어디인지를 명시한다. 키워드가 미리 정해진 *keyword values*와 호스트 주소를 직접 작성하는 *hosts values*, 특수 규칙인 *other values*로 나뉜다. 

하나의 디렉티브에 여러 value를 지정할 수 있고, value 사이의 구분은 공백으로 한다. 그리고 *hosts values*를 제외한 나머지는 작은따옴표로 감싸야 한다.

Keyword Values:

- `'none'`: 어떠한 소스도 로드할 수 없도록 한다.
- `'self'`: 현재 출처(도메인)의 리소스만 허용한다.
- `'strict-dynamic'`: **TODO**
- `'report-sample'`: **TODO**
- `'inline-speculation-rules'`: **TODO**

Unsafe Keyword Values:

- `'unsafe-inline'`: 인라인 자바스크립트와 CSS를 허용한다.
- `'unsafe-eval'`:  `eval()`, `setTimeout()`, `window.execScript()` 같은 동적 코드 평가(evaluation) 사용을 허용한다. `setTimeout()`은 함수 자체가 아니라, `setTimeout('console.log(123)', 100);`처럼 첫 번째 인수가 문자열인 경우를 의미한다.
- `'unsafe-hashes'`: **TODO**
- `'wasm-unsafe-eval'`: **TODO**

Hosts Values:

- Host: 특정 호스트의 출처를 허용하는 방식. 이 값은 와일드 카드`*`로 범위를 지정할 수 있고, 스킴(=프로토콜), 포트번호, path(도메인 다음의 경로)를 특정할 수 있다. (예시: `example.com`, `*.example.com`, `https://*.example.com:12/path/to/file.js`) 슬래시`/`로 끝나는 path는 접두어처럼 작동하지만, 이 외의 path는 완전히 일치해야만 한다. 예를 들어 `example.com/api/`는 `example.com/api/users/new`와 일치한다. 그리고 `example.com/file.js`는 `https://example.com/file.js`와 일치하지만, `https://example.com/file.js.old`와는 일치하지 않는다.
- Scheme: 특정 스킴을 모두 허용하는 방식이다. 항상 콜론`:`으로 끝나야 한다. (예시: `https:`, `data:`, `blob:`)

Other Values:

- `'nonce-*'`: **TODO**
- `'sha*-*'`: sha256, sha384, sha512 중에 하나. 하이픈`-` 다음에 이어지는 값은 스크립트나 스타일 코드에 대한 해시값이다. (예시: `'sha256-abc123'`) 특정 인라인 코드를 허용할 때 사용한다. `eval()`은 해당 없음


## 예시

```bash
# eval(), setTimeout() 등의 동적 코드 평가와 인라인 스크립트, 인라인 CSS 그리고 data: 스킴에 대한 모든 요청을 허용하고,
# 리소스를 가져올 사이트를 현재 출처(=접속한 사이트)와 www.google-analytics.com, fonts.googleapis.com, fonts.gstatic.com, chart.apis.google.com만을 허용한다
Content-Security-Policy: default-src 'self' 'unsafe-eval' 'unsafe-inline' data: www.google-analytics.com fonts.googleapis.com fonts.gstatic.com;
```
