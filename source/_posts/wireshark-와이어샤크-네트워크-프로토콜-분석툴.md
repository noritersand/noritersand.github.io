---
title: 'wireshark: 와이어샤크 네트워크 프로토콜 분석툴'
categories:
  - tools
tags:
  - wireshark
  - todo
date: 2018-04-18 18:06:23
---


#### 관련 문서
- https://www.wireshark.org/download.html
- https://wiki.wireshark.org/DisplayFilters#Examples
- https://openmaniak.com/kr/wireshark_filters.php#display
- http://www.ktman.pe.kr/internet/58491
- http://blog.daum.net/coy486/9
- [로컬호스트 트래픽 캡쳐하기](http://credemol.blogspot.kr/2012/10/wireshark-localhost.html)

```js
tcp.port eq 80
and http
and (ip.src eq 127.0.0.1 or ip.dst eq 127.0.0.1)
and http.request.uri eq "/product/productDetail"
```
tcp 이면서 http 이고 port가 80이며 127.0.0.1 IP와 주거나 받은것 중 URI가 '/product/productDetail' 것만 필터링.
