---
layout: post
date: 2024-06-01 21:25:07 +0900
title: '[Web] 데이터 URL(data URLs)'
categories:
  - web
tags:
  - data-urls
  - data-uris
  - data-uri-scheme
  - web-standard
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Data URLs - HTTP \| MDN](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URLs)


## 개요

데이터 URL에 대한 간단 정리 글.


## 설명

```
data:[<mediatype>][;base64],<data>
```

텍스트나 이미지 등의 데이터를 URL 형태로 나타낸 것. 이렇게 생겼다:

```
data:image/png;base64,iVBORw0KGgoAAAANSU....
```

예전에는 'data URIs' 혹은 'data URI scheme'이란 이름으로 알려졌었지만 WHATWG에 의해 폐기되었다고 한다.

관련 문서:

- [Wikipedia \| Data_URI_scheme](https://en.wikipedia.org/wiki/Data_URI_scheme)
- [https://stackoverflow.com/questions/19696418/what-does-it-means-dataimage-png-in-the-source-of-an-image](https://stackoverflow.com/questions/19696418/what-does-it-means-dataimage-png-in-the-source-of-an-image)
- [https://datatracker.ietf.org/doc/html/rfc2397](https://datatracker.ietf.org/doc/html/rfc2397)

이미지의 소스를 URL로 링크하는 대신, 직접 HTML 혹은 CSS 코드에 인라인으로 포함시킬 때 사용한다.

가령 아래 코드는:

```html
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxgljNBAAO9TXL0Y4OHwAAAABJRU5ErkJggg=="
    alt="Red dot"/>
```

작은 빨간색 점 이미지를 base64로 인코딩하여 `src` 속성에 데이터로 끼워넣은(?) 것이다. 이렇게 하면 외부 자원인 이미지를 웹으로 요청하거나 다운로드하는 과정을 생략하고 바로 볼 수 있게 된다. 물론 이걸 그리는 건 브라우저가 알아서 할 일이다.

[오류 처리를 위한 적절한 수단이 없고, 브라우저마다 최대 길이 제한이 다르며, 쿼리스트링을 쓸 수 없는 등의 제약](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Data_URLs#common_problems)이 있다. 또 피싱같은 보안 문제가 있어서 [특정 내용의 data URL은 브라우저가 차단](https://blog.mozilla.org/security/2017/11/27/blocking-top-level-navigations-data-urls-firefox-59/)하기도 한다. 
