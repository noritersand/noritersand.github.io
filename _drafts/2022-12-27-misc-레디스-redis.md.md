---
layout: post
date: 2022-12-27 18:27:47 +0900
title: '[misc] 레디스 Redis'
categories:
  - misc
tags:
  - redis
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [Redis](https://redis.io/)
- [Redis Documentation](https://redis.io/docs/)

#### 버전 정보

- Ubuntu 20.x


## 개요

인메모리 데이터베이스인 레디스의 사용 방법 간단 정리.


## 설치

[\[Redis\] Install Redis on Windows](https://redis.io/docs/getting-started/installation/install-redis-on-windows/)

```bash
url -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis
```


## 서버 시작

```bash
sudo service redis-server start

# CLI 터미널 접속
redis-cli

# 살아있는 지 확인
ping
```


## 데이터 타입

### Keys

데이터의 키가 되는 타입. 레디스는 'binary safe' 하다고 한다. 뭔 소리냐면, 일반적인 문자열(빈 문자열 포함)부터 JPEG 파일까지의 모든 바이너리 시퀀스를 키로 사용할 수 있다. 

너무 긴 데이터는 성능에 악영향을 끼친다. 적당히 짧으면서 가독성 있는 값을 사용하라고 한다. 레디스가 권장하는 좋은 키 값이란 이런 모양이다:

- `object-type:id`
- `user:1000`
- `comment:4321:reply.to`
- `comment:4321:reply-to`

키 타입의 최대 크기는 512 MB다.

**TODO** 근데 바이너리 시퀀스가 뭘까...

### Strings

### Lists

### Sets

### Hashes

### Sorted sets

### Streams

### Geospatial

### HyperLogLog

### Bitmaps

### Bitfields


## 주요 명령어

