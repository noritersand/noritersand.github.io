---
layout: post
date: 2022-12-27 18:27:47 +0900
title: '[redis] 레디스 Redis'
categories:
  - redis
tags:
  - misc
  - redis
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Redis \| Documentation](https://redis.io/docs/)

#### 테스트 환경 정보

- 테스트 환경: Windows WSL-Ubuntu 20.x
- Redis 7.2.x


## 개요

레디스는 인메모리 데이터베이스다. 그러니까 흔히들 메모리라 부르는 주기억 장치에 데이터를 저장하여 입출력 속도가 빠르지만, 전력이 끊기면 모조리 지워지는 특징이 있는 데이터베이스다.

그러나 레디스는 디스크에 별도로 데이터를 보관하는 기능을 제공하기 때문에 (설정에 따라) 데이터 보존을 기대할 수 있다.

BSD 라이선스라서 저작권자 표기, 보증 부인을 지키는 한 개작/배포 제한이 없다.

빈 값에 대한 표현은 `null`이 아닌 `(nil)`이다.


## 참고

어떤 데이터를 레디스 메모리에 저장한다는 표현은 다음처럼 작성하면 적절함:

> (데이터 타입이 Strings 일 때) 
> 레디스 키 `a`의 값 `b`를 저장한다. (혹은) 레디스에 값 `b`를 저장한다.
>
> (데이터 타입이 Hashes 일 때)
> 레디스 키 `a`의 해시 필드 `b`를 저장한다. (혹은) 레디스 해시에 `b` 필드를 저장한다.


## 설치

[Redis \| Install Redis on Windows](https://redis.io/docs/getting-started/installation/install-redis-on-windows/)

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


## 원격 접속

Redis CLI 클라이언트로 원격 서버에 접속해보자.

```bash
redis-cli -h HOST_NAME -p PORT_NUMBER

# 접속 후 인증
auth USERNAME PASSWORD
```

`redis-cli` 명령에 아이디 비번을 붙여서 사용하는 방법이 있긴 한데 보안문제로 권장하지 않는다 함. (그러면 옵션을 왜 만들어놨어 😒)

**TODO** 스프링 부트의 내장 레디스 로컬 서버에는 접속이 안 되는데 확인 필요. (데이터그립으론 잘 되는데)


## 클라이언트 라이브러리

[Redis \| Clients](https://redis.io/docs/data-types/timeseries/clients/)

리눅스 터미널의 CLI 클라이언트 외에 제공되는 각 언어별 클라이언트. 예로 Java용 클라이언트는 Jedis, JRedisBloom가 있고 JavaScript용 클라이언트는 rebloom이 있음.


## 보안 강화

레디스의 모든 인터페이스는 기본적으로 인증 절차를 필요로 하지 않는다. 따라서 보안 강화를 위해 다음 절차를 권장하고 있다:

1. 레디스로 통하는 포트를 외부에서 연결할 수 없도록 방화벽을 설정할 것(레디스의 통신 포트는 6379, 클러스터 모드에선 16379, 센티널(?)에선 26379번이 기본값임).
2. `bind` 디렉티브로 특정 네트워크와의 통신만 허용하도록 설정할 것.
3. `requirepass` 옵션으로 클라이언트가 `AUTH` 명령을 사용해 인증 단계를 수행하도록 설정할 것.
4. [spiped](http://www.tarsnap.com/spiped.html) 같은 SSL 터널링 소프트웨어를 사용해 클라이언트와 서버 간 통신을 암호화 할 것.


## 설정 바꾸기

[Redis \| Redis configuration](https://redis.io/docs/management/config/)

### 로컬 서버 포트 변경

`/etc/redis/redis.conf` 파일의 `port` 항목을 변경하고 재시작하면 된다.


## 클러스터링

레디스 서버 2대 이상을 클러스터링으로 구성하면 RDB 옵션이어도 디스크 저장 없이 재난에 대처할 수 있다. 죽었다 살아난 서버가 다른 클러스터의 데이터를 불러오도록 할 수 있기 때문.

**TODO**


## 데이터셋을 디스크에 저장하기

[Redis \| Redis persistence](https://redis.io/docs/management/persistence/)

`save` 명령으로 덤프 파일을 만드는 것을 의미한다. 저장 옵션 RDB, AOF, No persistence, RDB + AOF 하나를 선택할 수 있다.

RDB는 저장 옵션(persistence options)의 기본값이며 주기적으로 스냅샷을 파일로 저장하는 방식이다. 이렇게만 말하면 자동으로 될 것 같지만 백업 주기는 수동으로 설정해줘야 한다. (아마도)

AOF는 로그 방식이라고 부르며, 수신된 모든 쓰기 작업을 디스크에 저장한다. RDB에 비해서 처리가 느리지만 정전 등의 재난에 대처하기 쉽다는 장점이 있다.


## 불러오기

**TODO** 이게 자동으로 되는건지 모르겠네...


## 데이터 타입 Data Types (자료형)

[Redis \| Understand Redis data types](https://redis.io/docs/data-types/)

### Keys

[Redis \| Understand Redis data types#keys](https://redis.io/docs/data-types/tutorial/#keys)

데이터의 키가 되는 타입. 레디스는 'binary safe' 하다고 한다. 뭔 소리냐면, 일반적인 문자열(빈 문자열 포함)부터 JPEG 파일까지의 모든 바이너리 시퀀스를 키로 사용할 수 있다. 

너무 긴 데이터는 성능에 악영향을 끼치니 적당히 짧으면서 가독성 있는 값을 사용하라고 한다. 권장되는 키의 모양은 아래와 같다:

- `object-type:id`
- `user:1000`
- `comment:4321:reply.to`
- `comment:4321:reply-to`

여기선 그냥 이름의 일부로 사용했지만, 콜론`:`으로 나눠지는 부분을 keyspace라고 한다. 이걸 이용한 알림 기능이 있는 모양이다. [Redis \| Redis keyspace notifications
](https://redis.io/docs/manual/keyspace-notifications/)

키 타입의 최대 크기는 512 MB다.

**TODO** 근데 바이너리 시퀀스가 뭘까... (`0`과 `1`로 이뤄진 연속된 값이 아닐까?)

### 문자열 Strings

일반적인 문자열을 생각하면 되는데, `set` 명령은 공백으로 인자를 구분하기 때문에 값이 공백이 있는 경우 따옴표로 감싸야 함:

```bash
set mykey 'qwer asdf'
OK

get mykey
"qwer asdf"

set mykey2 qwer asdf
(error) ERR syntax error
```

작은따옴표와 큰따옴표의 차이는 없다. [Redis \| Redis CLI#string-quoting-and-escaping](https://redis.io/docs/manual/cli/#string-quoting-and-escaping)

### JSON

### 리스트 Lists

스택과 큐를 구현한 데이터 타입. JavaScript의 배열과 비슷하다. 도움말에선 문자열 값으로 연결된 리스트(linked lists of string values)라고 한다.

### 셋 Sets

중복을 허용하지 않는 값들의 집합. 이 자료형의 구성 요소(값)는 멤버(members)라고 부른다. 도움말에선 순서가 지정되지 않은 고유 문자열 모음(unordered collection of unique strings)이라 설명한다.

### 해시 Hashes

특정 키 아래에 필드와 값을 쌍(field-value pair)으로 저장하는 데이터 타입이다. 다른 언어의 맵(Map), 딕셔너리(Dictionary), 혹은 연관 배열(Associative Array)과 유사하다.

### 정렬된 셋 Sorted Sets

**TODO**

### 스트림 Streams

**TODO**

### Geospatial

**TODO**

### 비트맵 Bitmaps

**TODO**

### 비트필드 Bitfields

**TODO**

### Probabilistic

**TODO**

### Time series

**TODO**


## 자주 쓰는 명령어

[Redis \| Commands](https://redis.io/commands/)

레디스의 명령어는 대소문자를 구분하지 않으며, 일부 명령어에서 쓰이는 패턴 문자열은 글롭 패턴(glob patterns)으로 입력한다. 

ℹ️ 글롭 패턴 요약: 모든 글자`*`, 한 글자`?`, 문자 범위 지정`[]`

### MONITOR

레디스 서버에서 처리되는 모든 명령을 다시 스트리밍한다. 어떤 명령이 실행되고 있는지 디버깅할 때 쓴다. 관리자 기능이나 `QUIT`은 기록되지 않는다.

⚠️ `MONITOR`는 서버 부하를 크게 증가시키니 운영 환경에서는 주의해서 사용할 것

```bash
# 스프링 부트 서버가 시작될 때의 레디스 로그
> monitor
OK
1766146681.448195 [0 127.0.0.1:41184] "HELLO" "3"
1766146681.459792 [0 127.0.0.1:41184] "CLIENT" "SETINFO" "lib-name" "Lettuce"
1766146681.459806 [0 127.0.0.1:41184] "CLIENT" "SETINFO" "lib-ver" "6.3.2.RELEASE/8941aea"
1766146684.002978 [0 127.0.0.1:41184] "HMSET" "spring:session:sessions:0dce35b1-9cd3-4ded-9675-82e56b27baa4" "lastAccessedTime" "\xac\xed\x00\x05sr\x00\x0ejava.lang.Long;\x8b\xe4\x90\xcc\x8f#\xdf\x02\x00\x01J\x00\x05valuexr\x00\x10java.lang.Number\x86\xac\x95\x1d\x0b\x94\xe0\x8b\x02\x00\x00xp\x00\x00\x01\x9b6\x8b\xb4#" "creationTime" "\xac\xed\x00\x05sr\x00\x0ejava.lang.Long;\x8b\xe4\x90\xcc\x8f#\xdf\x02\x00\x01J\x00\x05valuexr\x00\x10java.lang.Number\x86\xac\x95\x1d\x0b\x94\xe0\x8b\x02\x00\x00xp\x00\x00\x01\x9b6\x8b\xb4#" "maxInactiveInterval" "\xac\xed\x00\x05sr\x00\x11java.lang.Integer\x12\xe2\xa0\xa4\xf7\x81\x878\x02\x00\x01I\x00\x05valuexr\x00\x10java.lang.Number\x86\xac\x95\x1d\x0b\x94\xe0\x8b\x02\x00\x00xp\x00\x00\x0e\x10"
1766146684.006092 [0 127.0.0.1:41184] "PEXPIREAT" "spring:session:sessions:0dce35b1-9cd3-4ded-9675-82e56b27baa4" "1766150283939"
1766146684.017743 [0 127.0.0.1:41184] "EXISTS" "spring:session:sessions:0dce35b1-9cd3-4ded-9675-82e56b27baa4"
```

### INFO

```
info
info section
info section section2 section3 ...
```

서버에 대한 정보와 통계를 반환하는 명령어. `info`만 입력하면 모든 정보가 출력된다. `section`은 일종의 카테고리다. 가령 `info server`라고 입력해 레디스 버전과 시스템 OS 정보만 출력할 수 있다.

`section`으로 입력 가능한 키워드는 아래와 같다:

- `server`: General information about the Redis server
- `clients`: Client connections section
- `memory`: Memory consumption related information
- `persistence`: RDB and AOF related information
- `stats`: General statistics
- `replication`: Master/replica replication information
- `cpu`: CPU consumption statistics
- `commandstats`: Redis command statistics
- `latencystats`: Redis command latency percentile distribution statistics
- `sentinel`: Redis Sentinel section (only applicable to Sentinel instances)
- `cluster`: Redis Cluster section
- `modules`: Modules section
- `keyspace`: Database related statistics
- `errorstats`: Redis error statistics

**TODO** 번역

### CONFIG

설정값을 변경하거나 가져온다.

```bash
config get dir
1) "dir"
2) "/var/lib/redis"

# databases 설정 조회
config get databases

# user가 포함된 모든 설정 조회
config get *user*
```

### SELECT

데이터베이스를 선택하는 명령이다.

```bash
# 첫 번째 데이터베이스 선택
select 0

# 두 번째 데이터베이스 선택
select 1
```

레디스에는 (건드린 게 없다면) 16개의 데이터베이스가 있고, 접속 시 첫 번째(index=0) 데이터베이스가 기본값으로 선택된다. 

어떤 개발자의 말에 따르면 데이터베이스는 테스트하고 놀 때나 쓰고 차라리 인스턴스를 여러 개 만들라고 한다. (트랜잭션 때문이라나 뭐라나...)

### SAVE, BGSAVE

데이터를 즉시 디스크에 저장(dump the dataset to disk)하도록 하는 명령어. 저장된 파일의 경로는 설정을 건드리지 않았으면 `/var/lib/redis/dump.rdb`이다. 

```bash
# 즉시 저장(완료될 때까지 기다림)
save

# 백그라운드 저장(안기다림)
bgsave
```

예약을 걸 수도 있는데, 다음 명령은:

```bash
save 60 1000
```

매 60초 마다 천 건의 키가 변경된 경우 자동으로 디스크에 저장한다.

### FLUSHALL

현재 선택한 데이터베이스를 포함한 모든 데이터베이스의 키를 전부 삭제한다.

```bash
flushall
```

### KEYS

주어진 패턴과 일치하는 모든 키를 반환한다.

```
KEYS pattern
```

- `pattern`: 찾으려는 키의 패턴. 생략할 수 없고 모든 키를 반환하게 하려면 `*` 라고 작성한다.

```bash
keys *
1) "qwe"
2) "asd"
```

⚠️ 도움말에서 이 명령은 디버깅 및 키스페이스 레이아웃 변경과 같은 특수 작업을 위한 것이며, 단순히 키를 찾기 위해 사용하지 말라고 하고 있다. 데이터베이스 전체를 '풀 스캔'하기 때문에 데이터가 많을 수록 성능에 악영향을 주기 때문. 키를 찾고 싶으면 `SCAN`을 쓸 것.

### SCAN

[Redis \| SCAN](https://redis.io/commands/scan/)

```
SCAN cursor [MATCH pattern] [COUNT count] [TYPE type]
```

- `cursor`: 순회중인 데이터 집합의 현재 위치(여기까지 검색했다는 뜻). `0`은 시작지점을 의미한다. 그러니 처음부터 검색하려면 `0`으로 지정하면 됨.
- `pattern`: 찾으려는 키의 패턴. **생략하면 모든 데이터**가 `count`만큼 반환된다.
- `count`: 반환되는 데이터의 수를 지정하는 옵션. `SCAN`은 `count`로 지정된 만큼 반환하려고 하지만 내부 알고리즘과 데이터베이스의 상태에 따라 근사치로 반환될 수 있다. 이 옵션을 생략하면 대략 10개 씩 가져옴.
- `type`: **TODO**

```bash
# 커서 위치 10부터 모든 키 스캔
scan 10

# 이름이 정확히 'aaa'인 키를 커서 0부터 20개 스캔
scan 0 match aaa count 20

# 이름이 'mykey:'로 시작하는 모든 키를 커서 0부터 스캔
scan 0 match mykey:*

# 이름이 세 글자이고 두 번째 문자가 'a'인 키를 커서 5부터 스캔
scan 5 match ?a?
```

🚨 명령을 여러 번 실행하는 게 번거롭다고 `count`를 `99999`로 지정하면 안됨. 99999개를 한 번에 처리하기 위해 CPU를 점유하게 되기 때문

#### 점진적 탐색 알고리즘:

```
> scan 0
1) "7"
2)  1) "If you ask me."
    2) "qwe"
    3) "test"
    4) "I'm waldo"
    5) "asd"
    6) "Mighty fine morning."
    7) "bar"
    8) "q:w:e:r"
    9) "foo"
   10) "123:/home:456"
```

`SCAN`은 검색 패턴과 일치하는 키를 일정량 만큼 반환하며, 현재 위치를 의미하는 커서 값을 반환한다. 위 예시에서는 `7`이 반환됐는데, 아직 탐색할 데이터가 남았으니 `7`부터 다시 찾으라는 말로 이해하면 된다.

```
> scan 7
1) "0"
2) 1) "Hello there!"
   2) "qwe:zxc"
```

더 이상 찾을 데이터가 없을 때 반환되는 커서 값은 `0`이다.

#### 검색은 0이 나올 때까지

범위가 좁은 패턴을 입력했다 해도 SCAN의 특성상 빈 배열과 커서 위치만 알려주는 경우가 있다:

```
> scan 0 match aaa
1) "5"
2) (empty array)

> scan 5 match aaa
1) "0"
2) 1) "aaa"
```

이럴 때는 반환된 커서 위치만 바꿔서 이전과 같은 검색 패턴으로 다시 검색해야 한다. 언제까지? `0`이 나올 때까지. 뭔가 일반적이지 않지만, 레디스가 원래 이렇게 설계되었다. '대규모 데이터베이스 환경에서 성능을 보호하고 데이터를 효과적으로 탐색하기 위함'이라나 뭐라나

### SET

문자열 값을 지정한 키로 메모리에 새로 저장한다.

```
SET key value [NX | XX | IFEQ ifeq-value | IFNE ifne-value |
  IFDEQ ifdeq-digest | IFDNE ifdne-digest] [GET] [EX seconds |
  PX milliseconds | EXAT unix-time-seconds |
  PXAT unix-time-milliseconds | KEEPTTL]
```

```bash
set mykey 'qwer'

set q:w:e:r 1234
```

만료시간을 지정하려면 `ex` 혹은 `px` 옵션을 덧붙인다:

```bash
# 만료시간 5초 설정(초 단위)
set qwe 'asd' ex 5

# 만료시간 5초 설정(밀리초 단위)
set foo 'bar' px 5000
```

### GET

지정한 키로 저장된 문자열 값을 반환한다.

```bash
get mykey
"qwer"

get q:w:e:r
"1234"
```

### TYPE

특정 키의 데이터 타입을 문자열로 반환한다.

```bash
type mykey
```

키가 없으면 `none`, 키가 있으면 `string`, `list`, `set`, `zset`, `hash`, `stream` 중에 하나를 반환한다.

### DEL

특정 키를 삭제한다.

```bash
del mykey
del mykey1 mykey2 mykey3
```

삭제에 성공하면 1, 지정한 키를 찾을 수 없으면 `0`을 반환한다.

⚠️ 이 명령은 키 삭제와 메모리 회수를 블로킹 방식으로 처리하기 때문에, 운영 환경에서는 사용하지 않는 게 좋다.

### UNLINK

`DEL`처럼 지정한 키를 삭제하되, 메모리 회수를 백그라운드에서 수행한다.

```bash
unlink mykey
unlink mykey1 mykey2 mykey3
```

### EXPIRE

특정 키에 저장된 데이터의 만료시간을 설정하고, 시간이 지나면 자동으로 삭제하게 하는 명령어. 초 단위로 지정한다.

```bash
# mykey의 만료시간을 30초로 설정
expire mykey 30
```

없는 키를 지정하면 `0`이 반환된다.

#### Options

`expire`의 옵션은 모두 만료시간을 설정할지 말지를 결정하는 조건으로 쓰인다.

- `nx`: 설정된 만료시간이 없을 때만
- `xx`: 설정된 만료시간이 있을 때만
- `gt`: 새 만료시간이 기존보다 클 경우에만
- `lt`: 새 만료시간이 기존보다 작은 경우에만

### TTL

```
TTL key
```

특정 키에대한 만료시간을 반환한다. 만약 키가 존재하지 않으면 `-2`를, 키가 존재하나 만료시간이 없으면 `-1`을 반환한다.

### LPUSH

리스트(Lists)의 앞에 요소(element)를 추가한다.

### RPUSH

리스트의 뒤에 요소를 추가한다. lpush의 반대격

### LPOP

리스트의 맨 앞 요소를 꺼낸다.

### RPOP

리스트의 맨 뒤 요소를 꺼냄.

### LLEN

리스트의 길이 반환

### LMOVE

element의 위치를 변경하는 명령어

### LRANGE

주어진 offset의 모든 요소를 반환한다. offset은 시작과 끝의 인덱스로 지정하는 방식이며, 인덱스는 음수일 수도 있는데 이 경우 끝에서부터 몇 번째인지를 의미함.

offset이 `-3 2`면 끝에서 3번째 요소부터 처음에서 3번째 요소까지를 출력하라는 뜻이다. 그러니까 그냥 `0 2`와 같음.

### LTRIM

주어진 offset의 모든 요소를 삭제한다.

### SADD

지정한 키의 셋(Sets)에 멤버(member, 셋의 구성 요소를 의미함)를 추가한다. 

```bash
sadd foo var
sadd foo var2
```

### SMEMBERS

지정한 키로 저장된 셋의 모든 멤버를 반환한다.

```bash
smembers foo
1) "var2"
2) "var"
```

### SISMEMBER

특정 멤버가 있는지 확인

### SINTER

둘 이상의 셋 사이에 존재하는 공통 멤버를 반환함

### SCARD

특정 셋의 멤버 수를 반환

### SSCAN

셋의 멤버를 대상으로 패턴 검색을 수행한다. 특정 키 내부에서 검색한다는 점만 빼면 `scan`과 나머지는 같다.

```
SSCAN key cursor [MATCH pattern] [COUNT count]
```

```bash
> sscan foo 0 MATCH var*
1) "0"
2) 1) "var1"
   2) "var2"
   3) "var3"
```

### HSET

특정 키의 해시 필드를 저장한다.

```
HSET key field value
HSET key field value field2 value2
...
```

```bash
hset map a 'aaa'
hset map b 'bbb' c 'ccc'
```

### HGET

특정 키의 해시 필드를 출력한다.

```
HGET key field
```

```bash
> hset map a 'aaa'

> hget map a
"aaa"
```

⚠️ 해시를 `GET`으로 꺼내려 하면 타입 에러가 발생한다:

```bash
> hset something a 'bcd'
(integer) 1

> get something
(error) WRONGTYPE Operation against a key holding the wrong kind of value
```

### HDEL

특정 키의 해시 필드를 삭제한다.

```
HDEL key field [field ...]
```

```bash
# 새로 저장
> hset something a 'bcd'
(integer) 1

# 특정 필드 삭제
> hdel something a
(integer) 1

> hget something a
(nil)
```

### HKEYS

특정 키의 모든 해시 필드의 이름을 출력한다.

```
HKEYS key
```

### HGETALL

특정 키의 모든 해시 필드의 이름과 값을 출력한다.

```
HGETALL key
```

```bash
> hset map a 'aaa' b 'bbb'

> hgetall map
1) "a"
2) "aaa"
3) "b"
4) "bbb"
```

### HSCAN

`scan`의 해시 버전. 다른 점은 특정 해시 키의 필드 이름이나 필드 값이 패턴과 일치하면 모두 출력한다는 것.

```
HSCAN key cursor [MATCH pattern] [COUNT count]
```

```bash
> hset map a 'aaa'

> hscan map 0 match a
1) "0"
2) 1) "a"
   2) "aaa"
```

### ZSCAN

sorted sets 타입 패턴 검색 명령어


## ACL(Access Control List) 시스템

- [Redis \| ACL](https://redis.io/docs/management/security/acl/)
- [Redis \| Commands: ACL](https://redis.io/commands/acl/)

`ACL`은 레디스 6.0 버전에 추가된 권한 관리 기능이다. ACL을 통해 각각 다른 권한을 가진 여러 사용자를 생성할 수 있으며, 특정 명령어 또는 키에 대한 액세스 권한을 제어할 수 있다.

```bash
# 모든 키에 대한 set 명령어 사용 권한이 있는 verginia 사용자 생성
# 이 사용자는 활성상태가 되며, 이미 존재하는 사용자면 활성상태로 바꾸고 set 권한을 부여한다
acl setuser user_name on allkeys +set

# 사용자에게 get 명령어 사용 권한을 부여한다
# 하지만 어떤 키에도 접근할 수 없어서 get 사용은 불가
acl setuser user_name +get

# 사용자 활성화(활성화 전에는 로그인 불가)
acl setuser user_name on

# 사용자 비활성화
acl setuser user_name off

# 사용자 삭제
acl deluser user_name

# 현재 연결이 인증된(로그인한) 사용자의 이름을 반환한다
acl whoami

# 사용자의 비밀번호를 1234로 설정한다. 
# `>` 기호가 비밀번호를 나타낸다.
acl setuser user_name >1234

# user_name에게 모든 키(~*)에 대한 모든 명령어 사용 권한 부여
acl setuser user_name on +@all ~*

# user_name에게 모든 키(~*)에 대해 read 카테고리 read 카테고리에서 fast 카테고리를 제외한 명령어의 사용 권한 부여
acl setuser user_name on +@read -@fast ~*
```

### AUTH

일종의 로그인 기능. `ACL`이 누가 어떤 명령어를 쓸 수 있는지 제한하는 기능이라면, `AUTH`는 해당 정책을 적용받기 위해 신원을 증명하는 절차다. 만약 모든 명령어가 사용자 `SUPER`에게 할당되어 있다면, `SUPER`에 로그인하기 전에는 아무 명령도 실행할 수 없다.

```bash
# USER_NAME 사용자의 연결을 인증(= 로그인)한다. 이 때 비밀번호로 1234를 사용한다.
auth USER_NAME 1234

# 비밀번호가 없는 사용자 로그인 방법
auth default nopass
```


## Redis Pub/sub

`PUBLISH`와 `SUBSCRIBE` 명령으로 실시간으로 메시지를 주고받는 기능. 이 기능으로 레디스가 일종의 메시지 브로커를 수행하도록 할 수 있다. 구조가 매우 단순하고 빠른게 장점이지만, 메시지 전달 보장이 안되며 과거 메시지를 조회하는 기능 같은 건 없다.

### 사용 방법 예시

우선 `SUBSCRIBE`로 `notice.published` 채널을 구독한다:

```
> SUBSCRIBE notice.published

1) "subscribe"
2) "notice.published"
3) (integer) 1
Reading messages... (press Ctrl-C to quit or any key to type command)
```

이제 이 터미널은 그대로 두고 새 터미널을 열어서, `PUBLISH`로 `notice.published` 채널에 메시지를 발행한다:

```
> PUBLISH notice.published hello

(integer) 1
```

문제가 없다면, 구독중이던 터미널에서 다음처럼 출력된다:

```
1) "message"
2) "notice.published"
3) "hello"
```

### 관련 명령어

```bash
# 특정 채널 구독
SUBSCRIBE channel [channel ...]

# 특정 채널 구독 해제
UNSUBSCRIBE [channel [channel ...]]

# 채널 이름 패턴으로 구독
PSUBSCRIBE pattern [pattern ...]

# 채널 이름 패턴으로 구독 해제
PUNSUBSCRIBE [pattern [pattern ...]]

# pubsub 명령어 도움말 보기
PUBSUB HELP

# 패턴으로 모든 활성화 채널 조회. 누군가가 구독중이어야 활성화 채널로 간주함
PUBSUB CHANNELS [pattern]
```
