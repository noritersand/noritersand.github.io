---
layout: post
date: 2023-05-17 15:31:05 +0900
title: '[misc] 인코딩의 모든 것'
categories:
  - misc
tags:
  - misc
  - encoding
  - decoding
  - unicode
  - ascii
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [somewhere](somewhere)


## 개요

사실 모든 것은 아니고 그냥 인코딩 관련 내용 모아두는 글.


## 인코딩은 암호화가 아니다.

encoding은 encryption과 개념과 용도가 다르기때문에 둘 다 암호화라고 부르는 것은 정확한 의사소통을 위해서 피하는 게 좋다. encoding은 부호화, encryption은 암호화라고 하자.

그런데 decoding과 decryption은 뭐라고 불러야할까. decoding은 복호화, decryption은 해독 혹은 해독화라고 하는 것이 옳다.


## BOM, byte of mark

바이트 순서 표식은 유니코드 계열의 인코딩으로 저장된 파일이 정확히 어떤 인코딩인지를 나타낸다. 파일의 가장 처음에 위치하며 일반적인 편집기에서는 보이지 않고, 바이너리를 직접 읽거나 HEX 모드 등으로 읽어야 보이는 일종의 메타 정보다. BOM이 생략되는 경우도 많은데, 이 경우 편집기는 처음의 몇 바이트를 읽어서 유추한다.

|인코딩|BOM|
|---|---|
|UTF-8|`EF BB BF`|
|UTF-16 big-endian|`FE FF`|
|UTF-16 little-endian|`FF FE`|
|UTF-32 big-endian|`00 00 FE FF`|
|UTF-32 little-endian|`FF FE 00 00`|


## Base64

Base64는 바이너리 데이터를 라틴 문자(=알파벳), 숫자, 덧셈 기호`+`, 슬래시`/`의 조합으로 바꾸는 인코딩 방식이다. UTF-8 같은 케릭터 셋의 영향을 받지 않기 때문에 데이터 저장과 전송에 적합하다고 한다. 패딩 문자[^1]로 등호`=`가 사용된다.

Base64는 부호화(encoding) 알고리즘이지 암호화(encryption) 알고리즘이 아니다. 그래서 거꾸로 해독하기 매우 쉬우며, 비밀번호 같은 민감한 정보의 암호화에는 사용되지 않는다. 다만, 암호화 알고리즘 안에서 데이터 전송과 보존(Preserve raw bytes of cryptographic functions)을 위해 사용할 수는 있다고 함.

[^1]: 문자의 길이를 조정하기 위해 사용되는 의미 없는 문자. 예를 들어, 요구되는 일정한 길이보다 실제 값의 길이가 짧을 때 빈 자리를 채우는 용도로 사용할 수 있다.
