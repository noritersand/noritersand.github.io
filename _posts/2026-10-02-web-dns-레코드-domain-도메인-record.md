---
layout: post
date: 2026-10-02 18:26:43 +0900
title: '[Web] DNS 레코드 (DNS Record)'
categories:
  - web
tags:
  - web
  - domain
  - dns-records
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [IANA - Domain Name System (DNS) Parameters](https://www.iana.org/assignments/dns-parameters/dns-parameters.xhtml)
- [RFC 1034 - Domain names - concepts and facilities](https://www.rfc-editor.org/rfc/rfc1034)
- [RFC 1035 - Domain names - implementation and specification](https://www.rfc-editor.org/rfc/rfc1035)
- [RFC 2181 - Clarifications to the DNS Specification](https://www.rfc-editor.org/rfc/rfc2181)
- [RFC 2308 - Negative Caching of DNS Queries (DNS NCACHE)](https://www.rfc-editor.org/rfc/rfc2308)
- [RFC 2782 - A DNS RR for specifying the location of services (DNS SRV)](https://www.rfc-editor.org/rfc/rfc2782)
- [RFC 3596 - DNS Extensions to Support IP Version 6](https://www.rfc-editor.org/rfc/rfc3596)
- [RFC 3849 - IPv6 Address Prefix Reserved for Documentation](https://www.rfc-editor.org/rfc/rfc3849)
- [RFC 5321 - Simple Mail Transfer Protocol](https://www.rfc-editor.org/rfc/rfc5321)
- [RFC 5737 - IPv4 Address Blocks Reserved for Documentation](https://www.rfc-editor.org/rfc/rfc5737)
- [RFC 6376 - DomainKeys Identified Mail (DKIM) Signatures](https://www.rfc-editor.org/rfc/rfc6376)
- [RFC 7208 - Sender Policy Framework (SPF) for Authorizing Use of Domains in Email, Version 1](https://www.rfc-editor.org/rfc/rfc7208)
- [RFC 7505 - A "Null MX" No Service Resource Record for Domains That Accept No Mail](https://www.rfc-editor.org/rfc/rfc7505)
- [RFC 8305 - Happy Eyeballs Version 2: Better Connectivity Using Concurrency](https://www.rfc-editor.org/rfc/rfc8305)
- [RFC 8659 - DNS Certification Authority Authorization (CAA) Resource Record](https://www.rfc-editor.org/rfc/rfc8659)
- [RFC 9460 - Service Binding and Parameter Specification via the DNS (SVCB and HTTPS Resource Records)](https://www.rfc-editor.org/rfc/rfc9460)
- [RFC 9989 - Domain-Based Message Authentication, Reporting, and Conformance (DMARC)](https://www.rfc-editor.org/rfc/rfc9989)
- [Cloudflare - CNAME flattening](https://developers.cloudflare.com/dns/cname-flattening/)
- [Amazon Route 53 - Choosing between alias and non-alias records](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html)
- [Gmail Help - Email sender guidelines](https://support.google.com/a/answer/81126)


## 개요

도메인을 등록한 뒤 실제로 서비스에 연결하려면 먼저 DNS 레코드를 설정해야한다. 레코드는 "이 이름으로 질의가 오면 무엇을 돌려줄지"를 정의하며, 타입마다 돌려주는 값의 종류가 다르다.

레코드 한 줄은 `이름 TTL 클래스 타입 값` 순서로 쓴다. TTL은 리졸버가 응답을 캐시하는 시간(초)이고, 클래스는 사실상 항상 `IN`(Internet)이다.

```
example.com.      3600  IN  A      93.184.216.34
```

주로 쓰이는 레코드 타입은 다음과 같다.

- A: 도메인을 IPv4 주소에 연결한다.
- AAAA: 도메인을 IPv6 주소에 연결한다.
- CNAME: 도메인을 다른 도메인 이름의 별칭으로 만든다. 외부 서비스 연결에 주로 쓴다.
- MX: 메일을 받을 서버를 지정한다.
- TXT: 텍스트를 담는다. 소유권 확인, SPF, DKIM, DMARC에 쓴다.
- NS: 영역을 관리하는 네임서버를 지정한다. 위임에도 쓴다.
- SOA: 영역의 기본 관리 정보를 담는다. 대개 DNS 업체가 자동 관리한다.
- CAA: 인증서를 발급할 수 있는 인증기관을 제한한다.
- SRV: 특정 서비스의 호스트와 포트를 지정한다.
- PTR: IP 주소에서 도메인 이름을 찾는 역방향 조회에 쓴다.
- HTTPS: HTTP/3 지원, ECH 키 같은 연결 정보를 미리 알려 준다.


## A 레코드

도메인 이름을 IPv4 주소에 직접 연결한다.

```
example.com.      3600  IN  A      93.184.216.34
```

- 값은 항상 IP 주소다. 다른 도메인 이름을 넣을 수 없다.
- 하나의 이름에 A 레코드를 여러 개 둘 수 있으며, 리졸버는 그중 하나 또는 여러 개를 돌려준다(간단한 라운드 로빈 분산).
- IPv6 주소는 AAAA 레코드로 따로 설정한다.
- A는 Address의 약자다(RFC 1035). zone apex(루트 도메인)의 apex와는 관계없다.
- 서버 IP가 바뀌면 레코드를 직접 수정해야 한다.


## AAAA 레코드

도메인 이름을 IPv6 주소에 직접 연결한다. 역할은 A 레코드와 같고 값만 IPv6 주소다(RFC 3596).

```
example.com.      3600  IN  AAAA   2606:2800:220:1:248:1893:25c8:1946
```

- quad-A라고 읽는다. IPv6 주소가 128비트로 IPv4(32비트)의 4배 크기라서 A를 네 번 붙인 이름이다.
- A와 AAAA를 함께 두면 IPv6를 쓸 수 있는 클라이언트는 두 주소로 연결을 시도해 먼저 성공한 쪽을 쓴다(Happy Eyeballs, RFC 8305).
- 서버가 실제로 IPv6로 응답하지 않는데 AAAA를 등록하면 일부 사용자가 접속 지연이나 실패를 겪을 수 있다. IPv6를 실제로 서비스할 때만 등록한다.
- CNAME으로 연결된 이름은 대상의 A와 AAAA를 모두 따라가므로 따로 설정할 필요가 없다.


## CNAME 레코드

도메인 이름을 다른 도메인 이름의 별칭(alias)으로 만든다. CNAME은 Canonical Name(정식 이름)의 약자다.

```
www.example.com.  3600  IN  CNAME  example.com.
blog.example.com. 3600  IN  CNAME  myblog.github.io.
```

- 값은 항상 도메인 이름이다. IP 주소를 넣을 수 없다.
- 리졸버는 CNAME을 만나면 대상 이름으로 다시 질의해 최종 A/AAAA 레코드까지 따라간다. 단계가 하나 늘어나는 만큼 첫 조회가 약간 느려질 수 있다.
- 대상 쪽 IP가 바뀌어도 CNAME 쪽은 손댈 필요가 없다. 그래서 GitHub Pages, Vercel, Netlify, CDN 같은 외부 서비스에 서브도메인을 연결할 때 주로 쓴다.


## CNAME 제약

- 한 이름에 CNAME이 있으면 같은 이름에 다른 레코드(A, MX, TXT 등)를 둘 수 없다(RFC 1034 3.6.2절, RFC 2181 10.1절).
- 이 때문에 루트 도메인(zone apex, 예: `example.com`)에는 CNAME을 쓸 수 없다. 루트에는 SOA, NS 레코드가 반드시 있기 때문이다.
- MX, NS 레코드의 값으로 CNAME인 이름을 가리키면 안 된다(RFC 2181 10.3절).
- 루트 도메인을 외부 서비스에 연결해야 할 때는 DNS 업체가 제공하는 비표준 기능을 쓴다. 업체마다 이름이 다르며, 공통적으로 DNS 서버가 대상 이름을 대신 조회해 A/AAAA 레코드로 응답한다.
  - Cloudflare: CNAME Flattening
  - AWS Route 53: Alias 레코드
  - DNSimple, DNS Made Easy 등: ALIAS 또는 ANAME 레코드


## A와 CNAME 비교

- 값: A는 IPv4 주소, CNAME은 다른 도메인 이름이다.
- 루트 도메인 사용: A는 가능하고, CNAME은 불가능하다(업체별 대체 기능 필요).
- 다른 레코드와 공존: A는 가능하고, CNAME은 불가능하다.
- 대상 IP가 바뀔 때: A는 직접 수정해야 하고, CNAME은 수정할 필요가 없다.
- 주 용도: A는 직접 운영하는 서버 연결, CNAME은 외부 서비스 연결과 서브도메인 별칭이다.

### 선택 기준

- 고정 IP를 가진 서버(VPS 등)를 직접 운영한다면 A 레코드를 쓴다.
- 호스팅 업체가 "이 주소를 CNAME으로 설정하라"고 안내한다면 CNAME을 쓴다. 업체 IP는 예고 없이 바뀔 수 있으므로 A로 고정하지 않는다.
- 루트 도메인과 `www`를 함께 쓴다면, 루트는 A(또는 업체별 ALIAS 기능), `www`는 CNAME으로 루트나 서비스 주소를 가리키는 구성이 일반적이다.


## MX 레코드

이 도메인 앞으로 오는 메일(`user@example.com`)을 받을 메일 서버를 지정한다. MX는 Mail eXchanger의 약자다(RFC 1035).

```
example.com.      3600  IN  MX     10 mx1.example.com.
example.com.      3600  IN  MX     20 mx2.example.com.
```

- 앞의 숫자는 우선순위(preference)다. 작을수록 먼저 시도하고, 실패하면 다음 순위로 넘어간다. 같은 숫자끼리는 분산된다.
- 값은 도메인 이름이어야 한다. IP 주소를 직접 넣을 수 없고, CNAME인 이름을 가리켜도 안 된다.
- MX가 없으면 발신 서버는 그 도메인의 A/AAAA 주소로 메일 전달을 시도한다(RFC 5321 5.1절). 메일을 받지 않는 도메인이라면 `0 .` 값의 null MX(RFC 7505)로 수신하지 않음을 명시한다.
- Google Workspace, Microsoft 365 같은 메일 서비스를 쓰면 해당 업체가 안내하는 값을 그대로 넣는다.
- MX는 수신에만 관여한다. 발신 메일의 신뢰도는 TXT 레코드(SPF, DKIM, DMARC)로 설정한다.


## TXT 레코드

이름에 임의의 텍스트를 붙인다(RFC 1035). 원래는 사람이 읽는 설명용이었지만, 지금은 주로 기계가 읽는 설정값을 담는 데 쓴다.

```
example.com.                       3600 IN TXT "google-site-verification=abc123..."
example.com.                       3600 IN TXT "v=spf1 include:_spf.google.com ~all"
selector1._domainkey.example.com.  3600 IN TXT "v=DKIM1; k=rsa; p=MIIBIjANBgkq..."
_dmarc.example.com.                3600 IN TXT "v=DMARC1; p=quarantine; rua=mailto:dmarc@example.com"
```

주요 용도는 다음과 같다.

- 도메인 소유권 확인: Google Search Console, 각종 SaaS가 지정한 값을 넣어 도메인 소유자임을 증명한다. Let's Encrypt의 DNS-01 챌린지도 `_acme-challenge` 이름의 TXT 레코드를 쓴다.
- SPF(RFC 7208): 이 도메인 이름으로 메일을 보낼 수 있는 서버 목록이다. 한 이름에 SPF 레코드가 둘 이상 있으면 검증이 실패하므로 하나로 합쳐야 한다.
- DKIM(RFC 6376): 발신 메일의 전자 서명을 검증할 공개키다. `셀렉터._domainkey` 이름에 둔다.
- DMARC(RFC 9989, 2026년 5월에 RFC 7489를 대체): SPF, DKIM 검증에 실패한 메일을 수신 측이 어떻게 처리할지(none, quarantine, reject) 정하는 정책이다. `_dmarc` 이름에 둔다.

그 밖의 특징은 다음과 같다.

- 한 이름에 TXT 레코드를 여러 개 둘 수 있다.
- 문자열 하나는 최대 255바이트다. DKIM 키처럼 긴 값은 여러 문자열로 나눠 넣으며, 수신 측은 이를 이어 붙여 해석한다. 대부분의 DNS 관리 화면은 자동으로 나눠 준다.
- 2024년 2월부터 Gmail과 Yahoo는 대량 발신자에게 SPF, DKIM, DMARC 설정을 요구한다. 서비스 메일을 보낸다면 세 가지를 모두 설정하는 것이 사실상 필수다.


## NS 레코드

이 영역(zone)의 레코드를 어느 네임서버가 관리하는지 지정한다. NS는 Name Server의 약자다.

```
example.com.      86400 IN  NS     ns1.dnsprovider.com.
example.com.      86400 IN  NS     ns2.dnsprovider.com.
```

- 같은 NS 정보가 두 곳에 있다. 하나는 상위 TLD 레지스트리 쪽에 등록된 위임(delegation) 정보이고, 다른 하나는 자기 영역 안의 NS 레코드다.
- 등록업체 화면에서 "네임서버 변경"을 하면 레지스트리 쪽 위임 정보가 바뀐다. DNS 관리를 Cloudflare 같은 다른 업체로 옮길 때 하는 작업이 이것이다.
- 위임 정보는 TTL이 길어(.com은 2일) 변경 후 전 세계에 반영되기까지 시간이 걸린다.
- 서브도메인을 다른 네임서버에 위임할 때도 쓴다(예: `dev.example.com`만 별도 DNS 업체에서 관리).
- 안정성을 위해 보통 2개 이상 지정한다.


## SOA 레코드

영역의 기본 관리 정보를 담는다. SOA는 Start of Authority의 약자이며, 영역마다 루트에 정확히 하나 존재한다(RFC 1035).

```
example.com.  3600  IN  SOA  ns1.dnsprovider.com. hostmaster.example.com. (
  2026100201  ; serial
  7200        ; refresh
  3600        ; retry
  1209600     ; expire
  3600 )      ; minimum
```

- 주 네임서버: 영역의 원본을 가진 네임서버다.
- 관리자 메일: `@`를 `.`으로 바꿔 쓴다(`hostmaster.example.com.`은 `hostmaster@example.com`).
- serial: 영역의 버전 번호다. 바뀌면 보조 네임서버가 영역을 다시 받아 간다. 날짜와 순번을 붙인 `yyyyMMddnn` 형식을 관례로 쓴다.
- refresh, retry, expire: 보조 네임서버가 원본과 동기화하는 주기, 실패 시 재시도 간격, 동기화 실패가 계속될 때 데이터를 폐기하는 시간이다.
- minimum: 존재하지 않는 이름(NXDOMAIN)에 대한 응답을 캐시하는 시간이다(RFC 2308).
- Cloudflare, Route 53 같은 관리형 DNS에서는 업체가 자동으로 관리하므로 직접 수정할 일이 거의 없다.


## CAA 레코드

이 도메인에 SSL/TLS 인증서를 발급할 수 있는 인증기관(CA)을 제한한다. CAA는 Certification Authority Authorization의 약자다(RFC 8659).

```
example.com.      3600  IN  CAA    0 issue "letsencrypt.org"
example.com.      3600  IN  CAA    0 issuewild ";"
example.com.      3600  IN  CAA    0 iodef "mailto:security@example.com"
```

- `issue`는 일반 인증서, `issuewild`는 와일드카드 인증서를 발급할 수 있는 CA를 지정한다. `";"`는 어떤 CA도 허용하지 않는다는 뜻이다.
- `iodef`는 정책에 어긋난 발급 요청이 있을 때 보고받을 주소다.
- 2017년 9월부터 CA는 인증서 발급 전에 CAA 레코드를 확인해야 한다(CA/Browser Forum 규정).
- CAA 레코드가 없으면 모든 CA가 발급할 수 있다. 서브도메인에 CAA가 없으면 상위 도메인의 CAA를 적용한다.
- CAA를 설정한 뒤 실제로 쓰는 CA를 빠뜨리면 인증서 발급과 갱신이 실패한다. CDN이나 호스팅 업체가 대신 인증서를 발급한다면 그 업체가 쓰는 CA도 포함해야 한다.


## SRV 레코드

특정 서비스를 제공하는 호스트와 포트를 지정한다(RFC 2782). 이름은 `_서비스._프로토콜.도메인` 형식으로 쓴다.

```
_sip._tcp.example.com.        3600 IN SRV 10 60 5060 sip.example.com.
_minecraft._tcp.example.com.  3600 IN SRV 0 5 25565 mc.example.com.
```

- 값은 우선순위, 가중치, 포트, 대상 호스트 순서다. 우선순위가 작은 것부터 시도하고, 같은 우선순위 안에서는 가중치 비율로 분산한다.
- SIP, XMPP, LDAP, Kerberos(Active Directory), Microsoft 365, 마인크래프트 서버 등이 쓴다.
- 브라우저는 HTTP 연결에 SRV를 쓰지 않는다. 따라서 SRV로 웹사이트의 포트를 지정할 수는 없다.


## PTR 레코드

IP 주소에서 도메인 이름을 찾는 역방향 조회(reverse DNS)에 쓴다. PTR은 Pointer의 약자다.

```
34.216.184.93.in-addr.arpa.  3600  IN  PTR  mail.example.com.
```

- IPv4는 주소를 거꾸로 뒤집어 `in-addr.arpa` 아래에, IPv6는 `ip6.arpa` 아래에 둔다.
- 도메인의 DNS 업체가 아니라 IP 주소를 할당한 쪽(호스팅, 클라우드, ISP)에서 설정한다. VPS나 클라우드 관리 화면의 "Reverse DNS" 항목이 이것이다.
- 메일 서버를 직접 운영할 때 중요하다. 수신 측은 발신 IP의 PTR 이름과 그 이름의 A/AAAA 주소가 서로 일치하는지 확인하는 경우가 많고, 일치하지 않으면 스팸으로 분류하기도 한다.


## HTTPS 레코드

HTTPS 연결에 필요한 정보를 연결 전에 미리 알려 준다. 범용 형식인 SVCB 레코드를 HTTPS 용도로 특화한 것이다(RFC 9460, 2023년).

```
example.com.      3600  IN  HTTPS  1 . alpn="h3,h2" ipv4hint=93.184.216.34
하는지 확인하는 경우가 많고, 일치하지 않으면 스팸으로 분류하기도 한다.


## HTTPS 레코드

HTTPS 연결에 필요한 정보를 연결 전에 미리 알려 준다. 범용 형식인 SVCB 레코드를 HTTPS 용도로 특화한 것이다(RFC 9460, 2023년).

```
example.com.      3600  IN  HTTPS  1 . alpn="h3,h2" ipv4hint=93.184.216.34
```

- 지원 프로토콜(`alpn`, 예: HTTP/3), IP 힌트, ECH(Encrypted Client Hello) 키 등을 담는다. 브라우저는 이를 보고 첫 연결부터 HTTP/3로 접속하거나 HTTPS로 바로 연결할 수 있다.
- 우선순위가 0이면 별칭 모드로, 루트 도메인에서도 다른 이름을 가리킬 수 있다. 표준 방식의 루트 도메인 별칭이지만 클라이언트 지원이 고르지 않아 아직 ALIAS 같은 업체 기능을 대신하지는 못한다.
- Cloudflare는 프록시를 켠 도메인에 HTTPS 레코드를 자동으로 게시한다.


끗.
