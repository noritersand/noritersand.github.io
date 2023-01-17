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

- 테스트 환경: Windows WSL-Ubuntu 20.x


## 개요

레디스는 인메모리 데이터베이스다. 그러니까 흔히들 메모리라 부르는 주기억 장치에 데이터를 저장하여 입출력 속도가 빠르지만, 전력이 끊기면 모조리 지워지는 특징이 있는 데이터베이스다.

그러나 레디스는 디스크에 별도로 데이터를 보관하는 명령을 제공하기 때문에 어느 정도의 데이터 보존을 기대할 수 있다. (왜 어느 정도냐면, 디스크 백업 기능에 뭔가 큰 버그가 있다는 제보가...)

BSD 라이선스라서 저작권자 표기, 보증 부인을 지키는 한 개작/배포 제한이 없다.

빈 값에 대한 표현은 `null`이 아닌 `(nil)`이다.


## 설치

[\[Redis\] Install Redis on Windows](https://redis.io/docs/getting-started/installation/install-redis-on-windows/)

```bash
url -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis
```


## 서버 시작/종료

```bash
sudo service redis-server start

# CLI 터미널 접속
redis-cli

# 살아있는 지 확인
ping

# 종료
shutdown
```


## 보안 강화

레디스의 모든 인터페이스는 기본적으로 인증 절차를 필요로 하지 않는다. 따라서 보안 강화를 위해 다음 절차를 권장하고 있다:

1. 레디스로 통하는 포트를 외부에서 연결할 수 없도록 방화벽을 설정할 것(레디스의 통신 포트는 6379, 클러스터 모드에선 16379, 센티널(?)에선 26379번이 기본값임).
2. `bind` 디렉티브로 특정 네트워크와의 통신만 허용하도록 설정할 것.
3. `requirepass` 옵션으로 클라이언트가 `AUTH` 명령을 사용해 인증 단계를 수행하도록 설정할 것.
4. [spiped](http://www.tarsnap.com/spiped.html) 같은 SSL 터널링 소프트웨어를 사용해 클라이언트와 서버 간 통신을 암호화 할 것.


## 설정 바꾸기

[https://redis.io/docs/management/config/](https://redis.io/docs/management/config/)

TODO


## 데이터셋을 디스크에 저장하기

[https://redis.io/docs/management/persistence/](https://redis.io/docs/management/persistence/)

`save` 명령으로 덤프 파일을 만드는 것을 의미한다. 저장 옵션의 기본값은 RDB.

TODO


## 불러오기

TODO 이게 자동으로 되는건지 모르겠네...


## 데이터 타입

### Keys

[https://redis.io/docs/data-types/tutorial/#keys](https://redis.io/docs/data-types/tutorial/#keys)

데이터의 키가 되는 타입. 레디스는 'binary safe' 하다고 한다. 뭔 소리냐면, 일반적인 문자열(빈 문자열 포함)부터 JPEG 파일까지의 모든 바이너리 시퀀스를 키로 사용할 수 있다. 

너무 긴 데이터는 성능에 악영향을 끼치니 적당히 짧으면서 가독성 있는 값을 사용하라고 한다. 권장되는 좋은 키는 아래와 같다:

- `object-type:id`
- `user:1000`
- `comment:4321:reply.to`
- `comment:4321:reply-to`

여기선 그냥 이름의 일부로 사용했지만, 콜론`:`으로 나눠지는 부분을 keyspace라고 한다. 이걸 이용한 알림 기능이 있는 모양이다. [https://redis.io/docs/manual/keyspace-notifications/](https://redis.io/docs/manual/keyspace-notifications/)

키 타입의 최대 크기는 512 MB다.

**TODO** 근데 바이너리 시퀀스가 뭘까...

### Strings

일반적인 문자열을 생각하면 되는데, `set` 명령은 공백으로 인자를 구분하기 때문에 값이 공백이 있는 경우 따옴표로 감싸야 함:

```bash
set mykey 'qwer asdf'
OK

get mykey
"qwer asdf"

set mykey2 qwer asdf
(error) ERR syntax error
```

홑따옴표, 쌍따옴표의 차이는 없다. [https://redis.io/docs/manual/cli/#string-quoting-and-escaping](https://redis.io/docs/manual/cli/#string-quoting-and-escaping)

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

[https://redis.io/commands/](https://redis.io/commands/)

아직 정확하진 않지만 권한에 따라 특정 명령셋을 제한할 수 있는 것 같다. (`config`나 `keys` 같은 관리자 명령에 가까운 것들)

### config

설정값을 변경하거나 가져온다.

```bash
config get dir
1) "dir"
2) "/var/lib/redis"

config get databases
```

### select

데이터베이스를 선택하는 명령이다.

```bash
# 첫 번째 데이터베이스 선택
select 0

# 두 번째 데이터베이스 선택
select 1
```

레디스에는 (건드린 게 없다면) 16개의 데이터베이스가 있고, 접속 시 첫 번째(index=0) 데이터베이스가 기본값으로 선택된다. 

어떤 개발자의 말에 따르면 데이터베이스는 테스트하고 놀 때나 쓰고 차라리 인스턴스를 여러 개 만들라고 한다. (트랜잭션 때문이라나 뭐라나...)

### save, bgsave

데이터를 즉시 디스크에 저장(dump the dataset to disk)하거나 백그라운드에서 처리하도록 하는 명령어. 저장된 파일의 경로는 설정을 건드리지 않았으면 `/var/lib/redis/dump.rdb`이다. 

```bash
# 즉시 저장(완료될 때까지 기다림)
save

# 백그라운드 저장(안기다림)
bgsave
```

예약을 걸 수도 있는데 가령 다음은:

```bash
save 60 1000
```

매 60초 마다 천 건의 키가 변경된 경우 자동으로 디스크에 저장한다.

### flushall

현재 선택한 데이터베이스를 포함한 모든 데이터베이스의 키를 전부 삭제한다.

```bash
flushall
```

### keys

주어진 패턴과 일치하는 모든 키 반환한다.

```bash
keys *
1) "qwe"
2) "asd"
```

이 명령은 제한적으로 사용하고 대신 `scan`을 사용하라고 한다.

### scan

TODO [https://redis.io/commands/scan/](https://redis.io/commands/scan/)

### set

문자열 값을 지정한 키로 메모리에 새로 저장한다.

```bash
set mykey 'qwer'

set q:w:e:r 1234
```

### get

지정한 키로 저장된 문자열 값을 반환한다.

```bash
get mykey
"qwer"

get q:w:e:r
"1234"
```

### sadd

지정한 키의 set에 구성원(members, set의 요소를 의미함)을 추가한다. 

```bash
sadd foo var
sadd foo var2
```

### smembers

지정한 키로 저장된 set의 모든 구성원을 반환한다.

```bash
smembers foo
1) "var2"
2) "var"
```


## 클라이언트 라이브러리

[https://redis.io/docs/stack/bloom/clients/](https://redis.io/docs/stack/bloom/clients/)

리눅스 터미널의 CLI 클라이언트 외에 제공되는 각 언어별 클라이언트. 가령 자바는 Jedis, JRedisBloom, 자바스크립트는 rebloom이 있음.


## 원격 접속

Redis CLI 클라이언트로 원격 서버에 접속해보자.

```bash
redis-cli -h HOST_NAME -p PORT_NUMBER

# 접속 후 인증
auth USERNAME PASSWORD
```

`redis-cli` 명령에 아이디 비번을 붙여서 사용하는 방법이 있긴 한데 보안문제로 권장하지 않는다 함. (그러면 옵션을 왜 만들어놨어 😒)
