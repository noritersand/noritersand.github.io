---
layout: post
date: 2018-04-18 18:06:23 +0900
title: '[devtool] Wireshark 와이어샤크 필터 정리'
categories:
  - devtool
tags:
  - devtool
  - wireshark
---


#### 참고한 문서

- [https://www.wireshark.org/download.html](https://www.wireshark.org/download.html)
- [https://wiki.wireshark.org/DisplayFilters#Examples](https://wiki.wireshark.org/DisplayFilters#Examples)
- [https://openmaniak.com/kr/wireshark_filters.php#display](https://openmaniak.com/kr/wireshark_filters.php#display)
- [http://www.ktman.pe.kr/internet/58491](http://www.ktman.pe.kr/internet/58491)
- [http://blog.daum.net/coy486/9](http://blog.daum.net/coy486/9)
- [로컬호스트 트래픽 캡쳐하기](http://credemol.blogspot.kr/2012/10/wireshark-localhost.html)

## 개요

와이어샤크의 필터를 정리한 글

## 연산자

- `eq` `==`
- `and`
- `or`

TODO

## 키워드

### 프로토콜

```js
// TCP 통신만 보기
tcp

// HTTP 통신만 보기
http

// SSL/TLS만 보기
ssl
```

`ssl`은 (보안 프로토콜이니까 당연하지만) 통신이 있었다... 수준의 패킷만 보인다.

### 포트번호

```js
// 통신 포트 80으로 필터링
tcp.port eq 80
```

### IP 필터

```js
// IP 172.30.1.1 에서 출발했거나 도착한 통신만 보기
ip.addr eq 172.30.1.1

// 출발지 IP가 172.30.1.1
ip.src eq 172.30.1.1

// 도착지 IP가 172.30.1.1
ip.dst eq 172.30.1.1
```

### URI

TODO

## 필터 조합 예시

TCP 이면서 HTTP 이고 포트번호가 80이며 127.0.0.1 IP와 주거나 받은것 중 URI가 '/product/productDetail' 것만 필터링:

```js
tcp.port eq 80
and http
and (ip.src eq 127.0.0.1 or ip.dst eq 127.0.0.1)
and http.request.uri eq "/product/productDetail"
```
