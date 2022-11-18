---
layout: post
date: 2021-12-13 22:05:44 +0900
title: '[misc] 네트워크 관련 노트'
categories:
  - misc
tags:
  - misc
  - network
---

* Kramdown table of contents
{:toc .toc}


## CIDR, Classless Inter-Domain Routing

```bash
# IPv4
A.B.C.D/R

# IPv6
A:B:C:D:E:F:G:H/R
```

TODO 설명 필요

관련 문서:

- [https://namu.wiki/w/CIDR](https://namu.wiki/w/CIDR)
- [https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#CIDR_notation](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#CIDR_notation)


## NAT, Network address translation

네트워크 패킷의 IP 헤더에 있는 주소 정보(목적지 혹은 출발지 등)를 수정해 매핑을 변경하는 방법... 이라고 한다.

TODO

TODO NAT Gateway


## Name Server와 DNS의 차이

- [http://www.differencebetween.net/technology/difference-between-name-server-and-dns/](http://www.differencebetween.net/technology/difference-between-name-server-and-dns/)
- [https://www.quora.com/What-is-the-difference-if-any-between-DNS-server-and-name-server](https://www.quora.com/What-is-the-difference-if-any-between-DNS-server-and-name-server)

대충 DNS(Domain Name System)는 도메인을 아이피로 바꿔주는 시스템, 네임 서버는 DNS 정보가 저장되는 서버를 말함.


## hostname

인터넷 상의 서버 이름이다. 리눅스 서버는 `/etc/hostname` 파일에서 관리한다.

구성에 따라 서브 도메인의 '서브'를 의미할 수도 있다. ~~사실잘모른다~~


## IPv6

[https://datatracker.ietf.org/doc/html/rfc4291](https://datatracker.ietf.org/doc/html/rfc4291)

```
2606:2800:220:1:248:1893:25c8:1946
```

IPv4를 대체하는 새로운 주소 체계. 16진수 숫자 4자리를 사용하는 필드 8개로 표현한다.

0은 경우에 따라 생략할 수 있는데, 우선 가장 높은 자리수가 0인 경우다. 위의 예시는 example.com의 IPv6 주소인데, 생략 전 원래의 값은 `2606:2800:0220:0001:0248:1893:25c8:1946`이다.

그리고 필드 하나가 통으로 0이면서 연속으로 이어지는 경우다. 이 때는 콜론 두 개 `::`로 표현한다. 예를 들어 one.one.one.one의 IPv6 주소는 `2606:4700:4700::1001`인데 `4700`과 `1001`의 사이가 모두 0이라서 `::`로 생략한 것. 그래서 이 주소는 `2606:4700:4700:0000:0000:0000:0000:1001`과 같다.

'16진수 숫자 4자리가 총 8개'라는 규칙과 '생략한 자리는 0으로 채운다'라는 것만 기억하면 된다.


## Domain Apex

도메인의 루트 레벨을 말한다. 가령 example.com이란 도메인에서 'example.com'이 도메인 네임 계층의 루트 레벨에 해당하며 Domain Apex라고 부른다. 그러니께 a.example.com, b.example.com 같은 서브 도메인들은 Domain Apex가 아니다.

Domain Apex는 그냥 루트 도메인이라고도 부르며 이것 말고도 Zone Apex, Naked Domain이라고도 한다. 다 같은 말이다. 🥲


## DNS, Domain Name System

### VPN + DNS

젠킨스, 깃 사설 서버, 테스트 서버 등(이하 대상 서버) 개발에 필요한 서버들을 외부로부터의 접근을 제한해야 할 일이 있었음. 인가된 내부 인원인 개발자나 테스터한테는 허용해야 하니까 사설 VPN 서버를 구축하기로 함. 그리고 대상 서버들은 외부 접근이 불가능하며 내부 네트워크를 통해서만 접근이 가능하도록 만들어야 함. 즉, 대상 서버들의 공인 IP를 없애고 작업자 PC에서 VPN 서버로, VPN에서 대상 서버들로 통신하는 환경이 필요함. (이거시터널링인가?)

VPN을 구축할 서버(이하 VPN 서버)는 내부 네트워크에 있으면서 공인 IP가 부여된 상태. 우선 VPN 서버가 될 인스턴스에 OpenVPN으로 VPN 환경 구성함. 이 때 OpenVPN 설정 중 DNS는 VPN 서버 자신의 IP로 잡음(루프백 IP로 해도 될지 몰라). 왜냐면 같은 서버가 DNS 서버 역할도 할 꺼니께.

DNS 서버는 BIND9으로 구축함(근데 문제 있어서 나중에 AWS로 옮김) OpenVPN은 크게 어려운 게 없었으나 BIND9 설정은 DNS 시스템을 이해해야 해서 머리 깨진다.

OpenVPN 설정은 [여기](https://dejavuqa.tistory.com/243?category=299614), BIND9 설정은 [여기](https://lindarex.github.io/bind9/ubuntu-bind9-setting/)와 [여기](https://joungkyun.gitbook.io/annyung-3-user-guide/chapter5/chapter5-1-basic)를 참고함.

### 도메인 오리진

```bash
@  IN  SOA  ns.example.net. admin.example.net. (1 604800 86400 2419200 86400)
@  IN  NS   ns.example.net.
```

위 내용은 zone 파일의 일부이며 도메인 오리진(위 예시에서는 example.net에 해당하며 `@`가 이를 지칭한다.)의 SOA 레코드와 NS 레코드를 정의하는 내용이다.

도메인 오리진은 `$ORIGIN`으로 별도 지정하지 않는한 SOA 레코드의 루트 도메인인 example.net이다. (잠깐, 이러면 순환 참조 아닌가? 😵‍💫)

### 존 파일에서 도메인에 점을 찍는 이유

존 파일에서 도메인 뒤에 점`.`이 찍혀 있으면 해당 도메인은 완전하다는 의미가 된다. 만약 점을 빼면 그 뒤에 도메인 오리진이 자동으로 붙는다.

예를 들어 오리진이 example.com일 때 `www.example.com`이라고 쓰면 www.example.com.example.com 으로 인식한다는 것.

### CNAME 레코드

BIND9 설정에서 문제가 있었던 부분은 CNAME이었다. CNAME 레코드는 도메인에 대한 별칭을 설정할 때 사용하는데, 이 때 루트 도메인을 지정할 수 없다.

예를 들면, 아래는 틀린 문법이고:

```bash
@             IN  CNAME  example.com.
example.com.  IN  CNAME  example.net.
abc           IN  CNAME  @
```

아래처럼은 된다:

```bash
www                IN  CNAME  ns
git                IN  CNAME  gitlab-123456789.ap-northeast-999.elb.amazonaws.com.
mail.example.com.  IN  CNAME  mail.daum.net.
tis.not.exist.subdomain.example.com.  IN  CNAME  www.github.com.
```

사실 위 코드에서 아래 두 줄은 Zone Apex가 example.com일 때만 가능. Zone Apex는 존 파일 당 하나만 존재하는 루트 도메인으로 추정된다(구글링 결과를 종합해보면 Zone Apex는 원래 루트 도메인과 같은 말이지만 존 파일에서는 약간 다르게 쓰이는 걸로 보인다).

그러니 더 정확하게 설명하자면, CNAME 레코드의 별칭이나 대상은 루트 도메인으로 지정할 수 없으며 별칭은 Zone Apex를 벗어날 수 없다고 하는게 맞겠다.

[그리고 이렇게도 안된다고 한다](https://joungkyun.gitbook.io/annyung-3-user-guide/chapter5/chapter5-2-add-domain):

```bash
@          IN  NS    ns
ns         IN  CNAME @

mail       IN  CNAME @
           IN  MX 10 mail
```


## VPN, Virtual Private Network

TODO VPN 관련 노트

### TUN/TAP

TODO tap은 브릿징(layer 2)고 tun은 라우팅(layer 3)이라고 하는데 머선 말일까...


## OpenVPN

오픈 소스 OpenVPN으로 사설 VPN 서버를 구축한 게 있는데 관련 내용 적어둠.

설정 파일은 두 개가 있는데: 

- `/etc/openvpn/server.conf`
- `/etc/sysctl.conf`

`sysctl.conf`에서는 `net.ipv4.ip_forward`만 1로 바꿔줬고 나머진 `server.conf`에 해당함

### 중복 접속 허용

```bash
# Uncomment this directive if multiple clients
# might connect with the same certificate/key
# files or common names.  This is recommended
# only for testing purposes.  For production use,
# each client should have its own certificate/key
# pair.
#
# IF YOU HAVE NOT GENERATED INDIVIDUAL
# CERTIFICATE/KEY PAIRS FOR EACH CLIENT,
# EACH HAVING ITS OWN UNIQUE "COMMON NAME",
# UNCOMMENT THIS LINE OUT.
duplicate-cn
```

코멘트 처리 풀어주면 같은 키로 중복 접속 가능해짐

### 클라이언트의 DNS 변경 Push DNS Changes

VPN 서버에서 바라볼 DNS 서버 IP를 지정하는 설정

```bash
# Certain Windows-specific network settings
# can be pushed to clients, such as DNS
# or WINS server addresses.  CAVEAT:
# http://openvpn.net/faq.html#dhcpcaveats
# The addresses below refer to the public
# DNS servers provided by opendns.com.
# push "dhcp-option DNS 1.1.1.1"
# push "dhcp-option DNS 8.8.8.8"
push "dhcp-option DNS 172.31.0.226"
```

\* `172.31.0.226`는 사설 IP

### Redirect All Traffic Through the VPN

```bash
# If enabled, this directive will configure
# all clients to redirect their default
# network gateway through the VPN, causing
# all IP traffic such as web browsing and
# and DNS lookups to go through the VPN
# (The OpenVPN server machine may need to NAT
# or bridge the TUN/TAP interface to the internet
# in order for this to work properly).
push "redirect-gateway def1 bypass-dhcp"
````

이렇게 하면 모든 통신이 VPN을 통해 나가게 된다. 그런데 문제가 특정 사이트에서 (VPN 서버가 국외 IP라면) 해외 접속 차단 등의 제약이 걸리는 경우가 있음. 

그래서 이렇게 바꿔줬는데:

```bash
push "route 172.31.0.0 255.255.0.0 vpn_gateway"
```

TODO 그 다음부턴 차단 문제가 없어지긴 했지만 도데체 왜 이렇게 되는건지는 잘 몲

누군가의 설명에 따르면 이 설정으로 172.31.x.x(내 경우 AWS private subnet)에 접속할 때만 VPN 서버를 경유하게 된다 함.

TODO tun/tap 차이도 영향이 있는 것 같으니 확인 필요한데, 위 작업을 할 때 tun으로 설정돼 있었음.
