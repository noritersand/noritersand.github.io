---
layout: post
date: 1970-01-02 00:00:00 +0900
title: '[misc] IT 용어'
categories:
  - misc
tags:
  - misc
  - note
  - term
---

* Kramdown table of contents
{:toc .toc}


## OGNL, Object-Graph Navigation Language

자바 객체를 탐색하는데 사용하는 오픈소스 표현식 언어. 자바로 만들어졌으며 자바 전용이다. 비슷한 표현식 언어는 다른 언어에도 존재하는데, 가령 .NET Framework에는 LINQ(Language Integrated Query)가 있다.

MyBatis에서 OGNL 표현식은 `<if>`, `<when>`, `<bind>` 등에서 사용한다:

```xml
<select id="selectBlog" resultType="Blog">
  SELECT * FROM BLOG
  <if test="title != null and title != ''">
    WHERE title = #{title}
  </if>
</select>
```

여기에서 `test="title != null and title != ''"` 부분이 OGNL이다.


## 파일 경로를 구성하는 특수문자의 이름

`/`는 경로의 구성 요소를 구분하는 데 사용하는 문자로 *디렉터리 구분자*라고 한다.

`..`은 상위 디렉터리, `.`은 현재 디렉터리를 가리키는데, Bing 챗봇의 답변(2023-04-16)에 따르면 디렉터리 참조(directory reference)라고 한다는데, 관련 문서를 찾지 못했음.


## 도메인

> 도메인이란 컴퓨터 프로그래밍으로 문제를 해결하기 위해 만들 소프트웨어 프로그램을 위한 요구사항, 용어, 기능을 정의하는 학문 영역입니다. 간단히 말하면 해결하고자 하는 문제의 영역을 도메인이라고 합니다. 예를 들어 쇼핑몰을 만든다고 했을 때, 게시글, 댓글, 결제, 정산 등을 도메인이라고 할 수 있습니다.
> 
> 데이터베이스의 모델링에서 비슷한 개념이 있는데, 데이터베이스를 설계할 때 개념 모델링에 가깝다고 할 수 있습니다. 분류 가능한 업무 (요구사항)를 분석해서, 주제 영역을 도출하고 핵심 엔터티를 추출하고 그들 간의 관계를 정의하여 데이터의 골격을 생성하는 것 도메인 모델링과 가깝다고 할 수 있습니다.
> 
> 더 쉽게 이해해보면, 사용자 요구사항에 따른 상위 수준의 (크게 잡은?) 개발 범위를 도메인이라고 할 수 있습니다.
>
> Bing과의 대화, 2023-04-13

### 도메인 모델

개발 대상이 되는 업무를 파악하여 적절한 추상화 과정(모델링 과정)을 거쳐 눈에 보일 수 있는 형태(산출물)로 만들어 내는 것이라 할 수 있다. 도메인 모델은 비즈니스 영역에서 사용되는 객체를 판별하고, 객체가 제공해야 하는 기능을 추출하며, 각 객체간의 관계를 정립하는 과정을 거친다.

소프트웨어의 개발에서 모델링의 결과로는 단순한 설계도인 문서 형태로만 나타나게 된다. 프로그램에서 도메인 모델이라 하면 보통 UML의 클래스 다이어그램을 뜻하지만 하이버네이트에선 데이터베이스 테이블과 매핑할 영속 클래스(기존의 자바빈)를 의미한다.

> 도메인 모델은 애플리케이션이 다루는 문제 영역의 개념과 관계를 표현하는 모델입니다. 이 모델은 애플리케이션의 핵심 업무 로직을 구현하는 데 사용되며, 비즈니스 규칙과 제약 조건을 정의합니다.
>
> 도메인 모델은 일반적으로 클래스 다이어그램이나 개체-관계 다이어그램으로 표현됩니다. 이 다이어그램은 도메인의 주요 개념과 그들 간의 관계를 나타냅니다. 예를 들어, 온라인 쇼핑몰 애플리케이션의 도메인 모델은 고객, 상품, 주문 등의 개념과 이들 간의 관계를 나타낼 수 있습니다.
>
> 도메인 모델은 애플리케이션 개발의 초기 단계에서 정의되며, 애플리케이션의 요구사항을 분석하고 이해하는 데 중요한 역할을 합니다. 이 모델은 애플리케이션 개발자와 비즈니스 전문가가 함께 작업하여 정의하며, 애플리케이션 개발 과정에서 지속적으로 검토하고 업데이트됩니다.
>
> Bing과의 대화, 2023-04-12

### 비즈니스 로직 vs 도메인 로직

> 비즈니스 로직은 애플리케이션의 핵심 기능을 구현하는 코드로, 회사의 요구사항과 비즈니스 규칙을 구현합니다. 이는 애플리케이션의 사용자가 수행하려는 작업을 지원하는 코드로, 데이터 검증, 계산, 데이터베이스 작업 등을 포함합니다.
> 
> 도메인 로직은 비즈니스 로직의 하위 집합으로, 애플리케이션의 도메인 모델과 관련된 규칙과 제약 조건을 구현하는 코드입니다. 도메인 모델은 애플리케이션이 다루는 문제 영역의 개념과 관계를 표현하는 모델로, 도메인 로직은 이 모델의 무결성을 유지하는 데 중요한 역할을 합니다.
> 
> 간단히 말하면, 비즈니스 로직은 애플리케이션의 핵심 기능을 구현하는 코드로, 도메인 로직은 이 중에서도 도메인 모델과 관련된 규칙과 제약 조건을 구현하는 코드입니다.
>
> Bing과의 대화, 2023-04-12

그렇다고 한다.


## 데이터 모델

데이터의 의미, 데이터 사이의 관계, 일관성 제약 조건 등을 기술하기 위한 개념적 표현들의 집합.
데이터의 정의를 산출물로 작성한 것. 예로 ERD는 데이터 모델을 도식화한 것이다.


## 프로세스 모델

프로세스의 데이터 접근, 처리 등을 문서화한 것.


## 객체의 영속화란?

자바에서는 일반적으로 SQL을 이용해 데이터베이스에 데이터를 저장하는 것을 의미한다.


## 소프트웨어 버저닝 software versioning

버전 넘버링이라고도 함. 소프트웨어의 버전을 지정하는 방법인데, 어떤 표준 같은 게 있는 건 아니지만 모두둘 암묵적으로 따르는 규칙이 있음.

```
major.minor[.build[.revision]]

major.minor[.maintenance[.build]]

major.minor[.patch]
```

대부분 `major.minor` 까지는 동일하지만 그 뒤에 오는 세부 버전은 각 단체마다 제멋대로인 편 😏. 좌측에 있는 번호일 수록 수정의 중요도와 이전 버전과의 차이가 커진다.

`build` 번호 자리에는 하이픈과 추가 문자를 붙이기도 한다. 예를 들어:

- `-a1`: 첫 번째 알파 버전
- `-b3`: 세 번째 베타 버전
- `-rc1`: 릴리즈 후보 첫 번째

등이 있다.


## Isomorphic Application

isomorphic은 '같은 모양의, 동형의' 라는 뜻의 형용사다. 웹 개발에서 isomorphic application은 코드가 서버와 클라이언트 모두에서 실행될 수 있는 애플리케이션을 말한다. 보통은 JavaScript로 만들어진다. (클라이언트가 브라우저인 경우가 대부분이니까...)

보통은 완성된 페이지를 서버 측에서 렌더링하기 때문에 초기 로딩 시간이 적고 검색 엔진 최적화에 유리하다고 한다.

React나 Vue.js, Next.js, Flight 등의 프레임워크로 만든 웹 애플리케이션이 이에 해당한다.


## 커널 Kernel

하드웨어와 소프트웨어 간 인터페이스 역할을 하는 OS의 핵심. 시스템 자원 관리, 프로세스와 스레드 컨트롤, 파일 시스템 등의 기능을 제공한다고 함. System call이라는 인터페이스를 제공하는데 사용자 프로그램이 커널 기능에 접근할 때 쓴다.

인터럽트 처리, 메모리 관리, 장치 드라이버 로드 및 관리, 프로세스 관리 등 하드웨어 제어와 관련된 모든 작업은 커널이 수행한다.

하드웨어와 소프트웨어 간 인터페이스 역할이라는 점에서 BIOS와 비슷하지만, BIOS는 컴퓨터 시스템이 시작될 때 하드웨어를 초기화하고, OS가 부팅에 필요한 상태를 만들어주는 역할'만' 한다는 차이가 있다.


## BSD, Berkeley Software Distribution

UNIX 운영체제의 일종. UNIX 코드 기반으로 UC Berkeley에서 개발함. 리눅스와는 일단 커널부터 다르고 시스템 구조, 명령어도 다르다. 

리눅스의 몇몇 명령어는 BSD 스타일의 옵션을 쓸 수 있다 가령 `ps aux`에서 `a`, `u`, `x`가 BSD 스타일의 옵션이다.


## data URI scheme

가끔 보면 브라우저 네트워크 트래픽으로 `data:image/png;base64,iV....` 이런 게 잡힌다. 정체는 "마치 외부 자원인 것처럼 웹 페이지의 데이터 인라인을 포함하는 방법을 제공하는 통합 자원 식별자(URI) 스킴"이다.

관련 문서:

- [https://en.wikipedia.org/wiki/Data_URI_scheme](https://en.wikipedia.org/wiki/Data_URI_scheme)
- [https://stackoverflow.com/questions/19696418/what-does-it-means-dataimage-png-in-the-source-of-an-image](https://stackoverflow.com/questions/19696418/what-does-it-means-dataimage-png-in-the-source-of-an-image)

이미지 같은 파일을 URL로 링크하는 것이 아니라, 직접 HTML 혹은 CSS 코드에 인라인으로 포함시킬 때 사용한다.

가령 이런 코드는:

```html
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAUA
AAAFCAYAAACNbyblAAAAHElEQVQI12P4//8/w38GIAXDIBKE0DHxglj
NBAAO9TXL0Y4OHwAAAABJRU5ErkJggg==" alt="Red dot" />
```

이미지 파일을 base64로 인코딩하여 src 속성에 데이터로 끼워넣은(?) 것이다. 이렇게 하면 외부 자원인 이미지를 웹으로 요청하거나 다운로드하는 과정을 생략하고 바로 볼 수 있게 된다. 물론 이걸 그리는 브라우저의 일이고.


## IaaS, PaaS, SaaS

- SaaS(Software as a Service): 노션, 드랍박스 처럼 인프라에서 애플리케이션까지 기업이 모든 것을 관리하는 서비스를 말한다.
- PaaS(Platform as a Service): SaaS와 IaaS의 중간 단계로 인프라와 런타임 환경, 미들웨어, OS까지 기업에서 관리하는 서비스다. 구동할 애플리케이션의 소스 코드만 사용자가 업로드하는 방식이라고 보면 됨. [Heroku](https://www.heroku.com/)가 대표적인 케이스
- IaaS(Infrastructure as a Service): OS와 인프라 영역(가상화, 서버, 저장장치, 네트워)을 기업에서 관리하는 서비스다. 딱 AWS EC2를 생각하면 됨.

IaaS(인프라) > PaaS(플랫폼) > SaaS(소프트웨어) 순으로 사용자가 관리해야 하는 부분이 많음. 

## KMS, Key Management Service

암호화 키 관리 서비스. 관리란 암호화 키의 생성과 삭제, 백업, 복구 등을 의미한다.

이 서비스를 쓰는 이유는 아마 시드나 키 값을 코드에 넣어두면, 유출 가능성이 높기 때문이지 않을까...

AWS KMS가 유명하다.


## of()

언젠가부터(아마 약 10년 전 부터) 사용되기 시작한 메서드 이름. 보통은 인스턴스를 새로 만들어 반환하는 메서드에 사용된다. 전달인자가 있는 경우 새 인스턴스를 만들 때의 변수로 활용된다.

참고:

- [JavaScript: Array.of()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/of)
- [Java: LocalDate.of()](https://docs.oracle.com/javase/8/docs/api/java/time/LocalDate.html#of-int-int-int-)


## Tenant

세입자, 임차, 소작 정도로 번역되는 말. 클라우드 컴퓨팅에선 상호 독립적인 인스턴스와 비슷한 의미로 쓰임


## MQTT, MQ Telemetry Transport

메시지 큐 서비스를 위한 기기간 통신 프로토콜. 헤더를 제외하면 제어 메시지가 최소 2바이트까지 가능할 정도로 경량이다. OASIS(기술 표준을 정의하는 비영리 단체) IoT 표준이라고 한다.

MQTT 서버는 중계 역할만 해서 브로커라고 불리는데 가장 유명한 것으로 Mosquitto, 그 외에 Mosca, RabbitMQ, EMQ, HiveMQ 등이 있다.

실제 전송을 어떤 식으로 하는지는 아직 모르지만, 전송 값은 아마 16진수 숫자로 이뤄진 시리얼한 모양인 것 같음.


## Zero Fill

비어있는 저장 공간을 0으로 채우는 것. 프로그래밍에서는 실제 값의 길이가 데이터 타입으로 지정한 자릿수보다 짧을 경우, 자릿수만큼 0 혹은 공백으로 채우는 속성이나 패턴을 의미한다.


## Continuation indent

원래는 한 줄로 이어써야 하는 코드를 (가독성 등의 이유로) 줄 바꿈할 때 들여쓰는 것을 말한다.

```js
function fn() {
  let abcdefg = isTrusted
      ? 'a'
      : isExtensible
          ? 'b'
          : 'c'
}
```

일반적으로 깊이(보통을 중괄호로 작성하는 블록)를 표현할 때 사용하는 들여쓰기를 두 번 연속으로 하는 게 관례적이긴 하다. 그러니까 들여쓰기가 2칸이면 continuation indent는 4칸으로 하는 식.


## CVE, Common Vulnerabilities and Exposures

직역하면 공통 취약성 및 노출인데, 소프트웨어나 프로그램, 소스 코드의 취약점 혹은 버그를 의미한다. 

CVE 아카이빙 사이트:

- [https://cve.mitre.org](https://cve.mitre.org)
- [https://cve.report](https://cve.report)

참고로 이 사이트를 운영하는 Mitre는 1958년 쯤 만들어진 IT 회사고, 항공 교통 관제 시스템 개발 프로젝트에 참여하기도 했음.

비슷한 용어로 CWE(Common Weakness Enumeration)가 있다. 차이점은 아직 몰?루


## PWA, Progressive Web Apps

네이티브도 하이브리드도 아닌 새로운 웹 앱의 종류. TODO


## 유닉스 시간 Unix time

Unix time stamp, Epoch time, POSIX time이라 부르기도 함. 1970-01-01 00:00:00 UTC 부터 경과된 시간을 초로 환산하여 나타낸 것. 유닉스 시간은 세슘 원자 시계와 태양시간의 오차를 보정하는 윤초를 표현할 수 없다고 한다.


## Slug

slug는 웹 주소에서 고유 식별이 가능한 부분을 의미한다. URL에서 컨텍스트를 제외한 나머지를 생각하면 된다. 예를 들어 `https://developer.mozilla.org/en-US/docs/Glossary/Slug` 이 주소에서 slug는 `Glossary/Slug`다.


## FIDO, Fast IDentity Online

[FIDO 얼라이언스](https://fidoalliance.org/?lang=ko)에서 제안한 사용자 인증 프레임웤. 비밀번호를 대체하며 생체인증을 지원한다.

사용자 인증을 서버가 아닌 클라이언트에서 수행하고 서버에는 결과만을 전달하는 식으로 작동한다고 한다. 서버에 개인정보를 저장하지 않으므로 해킹 위험이 적다고 말하는데, 이렇게 하면 클라이언트만 조작하면 되는거 아닌가하는 생각이 듦.


## IETF, Internet Engineering Task Force

프로토콜 같은 인터넷 기술에 대한 표준을 정의하는 기구. RFC(Request for Comments) 문서를 통해 표준을 정의/공표 한다.

[\[위키백과\] 국제 인터넷 표준화 기구](https://ko.wikipedia.org/wiki/국제_인터넷_표준화_기구)


## canonical

프로그래밍에서는 '규칙에 따르는 어떠한 것(according to the rules)'이란 의미로 사용된다고 한다. 교회의 정본, 규범, 계율을 의미하는 'canon'에서 파생된 단어로 보임. 근데 '음악 + 캐논'으로 검색해서 나온 어떤 블로그에서는 캐논이 그리스 말로 규칙, 표준이란 뜻이라고 함.


## [NFS, Network File System](https://en.wikipedia.org/wiki/Network_File_System)

원격지의 디렉터리/파일(혹은 드라이브)을 로컬 저장소에 있는것처럼 하는 것. AWS에선 NFS를 EFS(Elastic File System) 서비스로 제공한다.


## [SSHFS, SSH File System](https://en.wikipedia.org/wiki/SSHFS)

SSH로 원격지에 연결하여 디렉터리/파일을 마운트하거나 상호작용하는 파일 시스템 클라이언트. (단순히 NFS에 SSH를 적용한건 아닌것 같다.)


## 서버리스 Serverless

서버리스란 수요에 맞게 성능(혹은 용량)이 자동으로 확장/축소되는, 소비자(= 우리, 나, 개발자)의 관리 없이 고가용성이 유지되는 서버를 말한다.

AWS의 EC2 오토 스케일링도 서버리스의 일종이다. 잘 알려진 서버리스 데이터베이스로는 DynamoDB, Azure Cosmos DB, Fauna DB, Google Could Datastore, Amazon Aurora Serverless가 있다.

반대 개념으로 프로비저닝(provisioning, 즉시 사용 가능한 시스템을 미리 준비해 두는 것)이란 말이 쓰인다.


## APM, Application Performance Management

애플리케이션 성능 관리. 말 그대로 애플리케이션의 성능과 서비스 이용을 모니터링하고 관리하는 또 다른 애플리케이션을 의미한다.

상용 APM으로 Pinpoint, Scouter, 제니퍼, 와탭이 있다. (예시가 다 국산인 건 우연의 일치)


## LDAP, Lightweight Directory Access Protocol

경량 디렉터리 액세스 프로토콜. 인터넷 프로토콜의 일종으로 TCP/IP 위에서 디렉터리 서비스를 조회하고 수정하는 응용 프로토콜이다.


## deprecated

우리말로는 '유지보수가 중단되어 사용이 권장되지 않는', 짧게는 '지원이 중단된' 라고 표현할 수 있다. 이것도 길면 그냥 '버려진'이라고 하자.


## derived

유래된, 유도, 얻어진, 나온, 파생된이란 뜻의 영단어. 프로그래밍에선 소스 파일이 아닌 컴파일된 class 파일 등을 의미한다.


## 드랍 다운 리스트

Drop-down list. 펼침 메뉴. 클릭하면 아래로(상황에 따라 옆이나 위로) 펼쳐지는 메뉴를 말함.


## Compressed Archive

ZIP 파일을 부르는 업계 용어. 전체 용량을 줄인 압축파일을 의미함. 용량 압축 없이 여러 파일을 하나로 묶기만 한 것은 아마도 그냥 archive, 이걸 풀면 exploded archive인 듯.


## GPG

GNU Privacy Guard 혹은 GnuPG. RSA를 이용하는 보안 소프트웨어. 아마도 데이터 전송용 일 듯.


## SSH

쉽게 말해 텔넷에 보안 레이어를 씌운 것. 컴퓨터끼리의 안전한 통신을 위해 사용하는 프로토콜로 데이터 전송, 원격 제어 등에 사용한다.


## EOL

End Of Line 혹은 Line Ending. 한 줄이 끝났음을 뜻하는 문자나 문자열을 말한다. OS에 따라 LF, CRLF가 있다.


## 테스트 케이스/유닛/슈트 test cases/unit/suite

- 테스트 케이스: 테스트를 코드 한 줄을 의미(메서드 호출 하나)
- 테스트 슈트: 테스트 케이스의 집합

즉, `케이스 < 슈트`

일반적으론 개별 케이스와, 케이스의 묶음인 슈트가 끝이지만, 굳이 중간에 하나를 끼워넣자면 테스트 유닛이 있는걸로 추정된다.  
그러니께 이 경우는 `케이스 < 유닛 < 슈트`인데, 테스트 유닛이라는게 유닛 테스트(단위 테스트)란 말에서 파생되어 잘못 쓰여지게 된 말인건지 확실하지 않음. 😒


## pass-by-reference, pass-by-value, ... pass-by-object-reference?

변수가 객체를 어떤식으로 가리키냐의 차이. 원시 타입 데이터는 pass-by-value가 기본이며 참조 타입일 때만 달라짐. call-by든 pass-by든 결국 같은 말이고, 누군가의 정의로 생긴 개념이 아닌지라 말하는 사람마다 어떤게 by-reference인지 by-value인지 주장하는 바가 조금씩 다르다.

많은 사람들이 자바나 자바스크립트, 파이썬 등의 언어를 pass-by-value라고 말하곤 하는데, [엄밀히 따지면 pass-by-object-reference라는 말이 있다](https://robertheaton.com/2014/02/09/pythons-pass-by-object-reference-as-explained-by-philip-k-dick/). 이에 따르면 분류는 둘이 아니라 셋이다.

- 변수에 객체의 주소를 저장한다는 점은 셋 다 같음.

- pass-by-reference: 함수에 전달할 때 변수의 값이 아니라 변수 자체를(정확하게는 변수를 가리키는 참조) 인자로 전달한다. 함수 내에선 별도의 조작없이 인자를 그대로 매개변수로 사용하므로 함수 안팎의 변수는 서로 다르지 않고 늘 같은 객체를 가리킨다. 이것은 함수 매개변수를 재할당해도 마찬가지다. C언어 등의 포인터를 상상하면 된다.

- pass-by-value: 함수에 전달할 때 변수가 가리키는 객체가 함수의 매개변수로 '복제'된다. 같은 모양의 객체가 하나 더 생긴다는 말이다. 함수를 호출하는 시점부터 매개변수가 함수 밖 변수와의 연결은 완전히 분리된다. 대부분의 언어에서 원시형 데이터를 다루는 방법과 동일하다.

- pass-by-object-reference: 메모리 주소를 값으로만 취급한다(Object references are passed by value). 함수에 인자로 전달한 값은 매개변수에 복사하여 사용하는데, 객체를 복제하는게 아니라 주소값만 복사한다는게 요점. 함수 안밖의 변수들은 같은 객체를 가리키고 있지만, 매개변수를 재할당하는 시점부터 둘은 서로 다른 객체를 가리키게 된다.


## 폴리필 Polyfill

>폴리필은 웹 개발에서 기능을 지원하지 않는 웹 브라우저 상의 기능을 구현하는 코드를 뜻한다.
>기능을 지원하지 않는 웹 브라우저에서 원하는 기능을 구현할 수 있으나, 폴리필 플러그인 로드 때문에 시간과 트래픽이 늘어나고, 브라우저별 기능을 추가하는 것 때문에 코드가 매우 길어지고, 성능이 많이 저하된다는 단점이 있다.
>
> [\[위키백과\] 폴리필 (프로그래밍)](https://ko.wikipedia.org/wiki/폴리필_(프로그래밍))

명세서 상에는 정의되어 있으나 실제 구현체(브라우저 등)에서 해당 기능이 구현되어 있지 않은 경우, 코드로 보완하는것을 말함.

예를 들어 자바스크립트의 `String.prototype.startsWith()`는 IE에서 지원하지 않는데, 이 메서드를 사용자(개발자, 우리)가 직접 코드로 구현한 것이 폴리필이다.


## BOM, byte of mark

바이트 순서 표식은 유니코드 계열의 인코딩으로 저장된 파일이 정확히 어떤 인코딩인지를 나타낸다. 파일의 가장 처음에 위치하며 일반적인 편집기에서는 보이지 않고, 바이너리를 직접 읽거나 HEX 모드 등으로 읽어야 보이는 일종의 메타 정보다. BOM이 생략되는 경우도 많은데, 이 경우 편집기는 처음의 몇 바이트를 읽어서 유추한다.

|인코딩|BOM|
|---|---|
|UTF-8|`EF BB BF`|
|UTF-16 big-endian|`FE FF`|
|UTF-16 little-endian|`FF FE`|
|UTF-32 big-endian|`00 00 FE FF`|
|UTF-32 little-endian|`FF FE 00 00`|


## word, dword, qword

> 워드(word)는 하나의 기계어 명령어나 연산을 통해 저장된 장치로부터 레지스터에 옮겨 놓을 수 있는 데이터 단위이다. 메모리에서 레지스터로 데이터를 옮기거나, ALU을 통해 데이터를 조작하거나 할 때, 하나의 명령어로 실행될 수 있는 데이터 처리 단위이다. 흔히 사용하는 32비트 CPU(ARM 등)라면 워드는 32비트가 된다.
>
> [https://ko.wikipedia.org/wiki/워드_(컴퓨팅)](https://ko.wikipedia.org/wiki/%EC%9B%8C%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85))

데이터 처리의 기본 단위라는 말도 있음. 보통 word는 16비트, dword는 32비트, qword는 64비트의 처리 단위라고 보면 된다.


## crash, hang, memory leak

> 충돌(crash), 또는 시스템 크래시는 컴퓨터 프로그램(응용 소프트웨어 또는 운영 체제)이 적절하게 기능하는 것을 멈췄을 때 이다. 종종 이것은 이러한 오류를 맞닥뜨린 후에, 영향을 받는 프로그램을 종료하게 된다. 책임을 갖는 프로그램은 크래시 리포팅 서비스가 크래시와 잠재적으로 관련된 자세한 사항을 보고할 때까지 프리징된다. 만약 프로그램이 운영 체제의 중요한 부분이라면, 컴퓨터 전체가 크래시될 것이고, 종종 커널 패닉 또는 심각한 시스템 오류를 야기한다.
>
> [https://ko.wikipedia.org/wiki/충돌_(컴퓨팅)](https://ko.wikipedia.org/wiki/%EC%B6%A9%EB%8F%8C_(%EC%BB%B4%ED%93%A8%ED%8C%85))

> 프리징(freezing) 또는 응답 없음은 컴퓨터 환경에서 하나의 컴퓨터 프로그램나 시스템 전체가 입력에 대한 응답을 중단할 때 일어난다. 행(hang)이라고도 하지만 이 표현은 한국에서는 잘 쓰이지 않는다. 가장 흔히 마주치는 상황을 하나 예로 들면 그래픽 사용자 인터페이스를 갖춘 워크스테이션이 있다. 여기서 멈춰버린 프로그램에 속한 모든 창이 움직이지 않지만 마우스 커서는 화면에서 움직일 수 있지만 키보드 입력이나 마우스 클릭을 통해서 프로그램의 창에 어떠한 영향도 줄 수 없다.
>
> [https://ko.wikipedia.org/wiki/프리징_(컴퓨팅)](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A6%AC%EC%A7%95_(%EC%BB%B4%ED%93%A8%ED%8C%85))

> 컴퓨터 과학에서 메모리 누수(memory leak) 현상은 컴퓨터 프로그램이 필요하지 않은 메모리를 계속 점유하고 있는 현상이다. 할당된 메모리를 사용한 다음 반환하지 않는 것이 누적되면 메모리가 낭비된다. 일부 서적에서 메모리 손실이라는 용어로 뜻을 옮기기도 하지만 leak라는 표현은 단순히 잃는 것 이상의 개념이므로 누수라는 표현이 더 정확하다.
>
> [https://ko.wikipedia.org/wiki/메모리_누수](https://ko.wikipedia.org/wiki/%EB%A9%94%EB%AA%A8%EB%A6%AC_%EB%88%84%EC%88%98)


## HA, High Availability

> 고가용성이란 서버와 네트워크, 프로그램 등의 정보 시스템이 상당히 오랜 기간 동안 지속적으로 정상 운영이 가능한 성질을 말한다. 고(高)가용성이란 "가용성이 높다"는 뜻으로서, "절대 고장 나지 않음"을 의미한다. 고가용성은 흔히 가용한 시간의 비율을 99%, 99.9% 등과 같은 퍼센티지로 표현하는데, 1년에 계획 된 것 제외 5분 15초 이하의 장애시간을 허용한다는 의미의 파이브 나인스(5 nines) 즉, 99.999%는 매우 높은 수준으로 고품질의 데이터센터에서 목표로 한다고 알려져 있다. 하나의 정보 시스템에 고가용성이 요구된다면, 그 시스템의 모든 부품과 구성 요소들은 미리 잘 설계되어야 하며, 실제로 사용되기 전에 완전하게 시험되어야 한다.
>
> [https://ko.wikipedia.org/wiki/고가용성](https://ko.wikipedia.org/wiki/%EA%B3%A0%EA%B0%80%EC%9A%A9%EC%84%B1)


## DR, Disaster Recovery

재해가 발생해 서비스나 시스템이 중단 됐을 때 이를 복구하는 것을 말한다.


## CSRF(XSRF), Cross-site request forgery

> 크로스 사이트 요청 위조는 웹사이트 취약점 공격의 하나로, 사용자가 자신의 의지와는 무관하게 공격자가 의도한 행위(수정, 삭제, 등록 등)를 특정 웹사이트에 요청하게 하는 공격을 말한다.
>
> XSS를 이용한 공격이 사용자가 특정 웹사이트를 신용하는 점을 노린 것이라면, 사이트간 요청 위조는 특정 웹사이트가 사용자의 웹 브라우저를 신용하는 상태를 노린 것이다. 일단 사용자가 웹사이트에 로그인한 상태에서 사이트간 요청 위조 공격 코드가 삽입된 페이지를 열면, 공격 대상이 되는 웹사이트는 위조된 공격 명령이 믿을 수 있는 사용자로부터 발송된 것으로 판단하게 되어 공격에 노출된다.
>
> [https://ko.wikipedia.org/wiki/사이트_간_요청_위조](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%9A%94%EC%B2%AD_%EC%9C%84%EC%A1%B0)


## XSS, Cross-Site Scripting

> 크로스 사이트 스크립팅은 웹 애플리케이션에서 많이 나타나는 취약점의 하나로 웹사이트 관리자가 아닌 이가 웹 페이지에 악성 스크립트를 삽입할 수 있는 취약점이다. 주로 여러 사용자가 보게 되는 전자 게시판에 악성 스크립트가 담긴 글을 올리는 형태로 이루어진다. 이 취약점은 웹 애플리케이션이 사용자로부터 입력 받은 값을 제대로 검사하지 않고 사용할 경우 나타난다. 이 취약점으로 해커가 사용자의 정보(쿠키, 세션 등)를 탈취하거나, 자동으로 비정상적인 기능을 수행하게 할 수 있다. 주로 다른 웹사이트와 정보를 교환하는 식으로 작동하므로 사이트 간 스크립팅이라고 한다.
>
> [https://ko.wikipedia.org/wiki/사이트_간_스크립팅](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85)


## system exception과 application exception의 차이

자바에서 system exception은 컨테이너 또는 JVM에서 발생하는 예외, application exception은 코드에서 직접 발생하는 예외다. unchecked exception(e.g. `RuntimeException`)은 application exception에 속한다.


## JNI, Java Native Interface

> 자바 네이티브 인터페이스는 자바 가상 머신위에서 실행되고 있는 자바코드가 네이티브 응용 프로그램 그리고 C, C++ 그리고 어샘블리 같은 다른 언어들로 작성된 라이브러리들을 호출하거나 반대로 호출되는 것을 가능하게 하는 프로그래밍 프레임워크이다.
>
> [https://ko.wikipedia.org/wiki/자바_네이티브_인터페이스](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EB%84%A4%EC%9D%B4%ED%8B%B0%EB%B8%8C_%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4)


## 코드 커버리지 Code Coverage

> 소프트웨어의 테스트를 논할 때 얼마나 테스트가 충분한가를 나타내는 지표중 하나다. 말 그대로 코드가 얼마나 커버되었는가이다. 소프트웨어 테스트를 진행했을 때 코드 자체가 얼마나 실행되었냐는 것이다.
>
> 코드의 구조를 이루는 것은 크게 구문(Statement), 조건(Condition), 결정(Decision)이다. 이러한 구조를 얼마나 커버했느냐에 따라 코드커버리지의 측정기준은 나뉘게 된다. 일반적으로 많이 사용되는 커버리지는 구문(Statement)커버리지이며, 실행 코드라인이 한번 이상 실행 되면 충족된다. 조건(Condition)커버리지는 각 내부 조건이 참 혹은 거짓을 가지면 충족된다. 결정(Decision) 커버리지는 각 분기의 내부 조건자체가 아닌 이러한 조건으로 인해 전체 결과가 참 혹은 거짓이면 충족된다. 그리고 조건과 결정을 복합적으로 고려하는 MC/DC 커버리지 또한 있다. 커버리지를 측정하는 법은 사람이 로그를 찍어가거나 디버거를 이용하여 볼수는 있으나 매우 힘든 과정이다. 시중에는 많은 코드커버리지 측정 도구가 나와 있으며, 대표적인 도구로 DT10, LDRA, VectorCAST, CodeScroll Controller Tester, QualityScroll Cover 라는 도구가 있다.
>
> [https://ko.wikipedia.org/wiki/코드_커버리지](https://ko.wikipedia.org/wiki/%EC%BD%94%EB%93%9C_%EC%BB%A4%EB%B2%84%EB%A6%AC%EC%A7%80)


## SVN에서 BASE와 HEAD의 차이

https://stackoverflow.com/questions/15209463/svn-what-is-the-difference-between-base-revision-and-latest-from-repository

- BASE: base revision. 서버에서 마지막으로 내려받은 파일의 수정 없는 리비전.
- HEAD: lastest from repository. 서버의 가장 마지막 리비전.


## 퍼지 서치 fuzzy search

![](/images/fuzzy-search.gif)

[이미지 출처](https://github.com/mattyork/fuzzy/blob/master/README.md)

퍼지 검색 알고리즘, 유사성 검색 알고리즘, Approximate string matching, 근사 문자열 일치 등으로 부르는 검색 기술. 간단한 예를 들어 misc-note-1.md라는 파일을 찾을 때 'mitemd'만 입력해도 되는 방식이다.


## &amp;nbsp;

nbsp는 non-breaking space의 약어다. `U+0020 SPACE( )`와 다르게 워드 랩이 발생하지 않으며 브라우저 등에 의해 자동으로 제거되지 않는 공백이다.


## 간트 차트 Gantt chart

![](/images/gantt-chart.png)

> 간트 차트(Gantt chart)는 프로젝트 일정관리를 위한 바(bar)형태의 도구로서, 각 업무별로 일정의 시작과 끝을 그래픽으로 표시하여 전체 일정을 한눈에 볼 수 있다. 또한 각 업무(activities) 사이의 관계를 보여줄 수도 있다. 최초의 간트 차트는 카롤 아다미키(Karol Adamiecki)가 하모노그램(Harmonogram)이라는 이름으로 고안하였다. 아다미키는 1931년까지 이것을 발표하지 않았기 때문에 결국 헨리 간트(Henry Gantt)의 이름이 붙게 되었다. 헨리 간트는 자신이 고안한 차트를 1919년 엔지니어링 매거진(The Engineering Magazine)에 "Work, Wages and Profit"이라는 제목으로 발표하였다.
>
> [https://ko.wikipedia.org/wiki/간트_차트](https://ko.wikipedia.org/wiki/%EA%B0%84%ED%8A%B8_%EC%B0%A8%ED%8A%B8)


## SHA, Secure Hash Algorithm

> SHA(Secure Hash Algorithm, 안전한 해시 알고리즘) 함수들은 서로 관련된 암호학적 해시 함수들의 모음이다. 이들 함수는 미국 국가안보국(NSA)이 1993년에 처음으로 설계했으며 미국 국가 표준으로 지정되었다.
>
> [https://ko.wikipedia.org/wiki/SHA](https://ko.wikipedia.org/wiki/SHA)


## 해시 함수와 메시지 다이제스트

해시 함수(hash function)는 임의의 길이를 갖는 임의의 데이터에 대해 고정된 길이의 데이터로 매핑하는 함수를 말한다. 이러한 해시 함수를 적용하여 나온 고정된 길이의 값을 해시값(hash value)이라고 한다. 이 값은 또한 hash code, hash sum, checksum 등으로도 불린다. 메시지 다이제스트(digest, 메시지 압축)를 구현할 때 해시 함수를 사용한다.


## 해싱 Hashing

해싱은 해시 함수로 키(key)를 해시값(hash value)으로 매핑하는 과정 자체를 의미한다.


## 해시 테이블 Hash Table

해시 테이블은 키를 연산해 해시값으로 매핑하고, 해시값을 인덱스나 주소로 사용하는 자료구조를 말한다. 어디에 저장되어 있는지 직접 접근하므로 비교에 의한 탐색 방법과 비해 속도가 빠르다.


## CMOS, BIOS, UEFI

- CMOS: Complementary Metal-Oxide-Semiconductor. BIOS 설정이 저장되는 특수 메모리.
- BIOS: Basic Input/Output System. 메인보드에 있는 펌웨어.
- UEFI: Unified Extensible Firmware Interface. BIOS를 대체하는 펌웨어 규격.


## MIME, multipurpose internet mail extentions

전자우편을 위한 인터넷 표준 포맷. content type의 일종으로 http에서 사용됨


## DTD

Document Type Definition


## web.xml

web application deployment descriptor


## parameter, argument

- 매개변수 = parameter
- 인수 = 인자 = 전달인자 = argument


## [OGNL](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=2ahUKEwjX04St1I3hAhWGS7wKHVGLDCkQFjAAegQICRAB&url=https%3A%2F%2Fcommons.apache.org%2Fognl%2F&usg=AOvVaw0gek1QNwFATrO5BF6_LjL2)

Apache Commons Object Graph Navigation Library.


## HLS

HTTP Live Streaming. 원래 Apple에서 iOS 운영 체제의 일부로 구현한 비디오 전송 프로토콜이라고 한다.


## JWT, JSON Web Token

> JSON Web Token (JWT) is an open standard ([RFC 7519](https://tools.ietf.org/html/rfc7519)) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.
>
> [https://jwt.io/introduction/](https://jwt.io/introduction/)

JSON 기반 웹 토큰이란다. 세션(=쿠키) 대용으로 많이 쓴다고 함.


## OAuth

Open Authorization, Open Authentication. API 접근 위임(API Access Delegation)을 위한 [개방형 표준](https://ko.wikipedia.org/wiki/개방형_표준)(open standard) 프로토콜.  

> 애플리케이션(페이스북,구글,트위터)(Service Provider)의 유저의 비밀번호를 Third party앱에 제공 없이 인증,인가를 할 수 있는 오픈 스탠다드 프로토콜이다. OAuth 인증을 통해 애플리케이션 API를 유저대신에 접근할 수 있는 권한을 얻을 수 있다.
>
> [https://minwan1.github.io/2018/02/24/2018-02-24-OAuth/](https://minwan1.github.io/2018/02/24/2018-02-24-OAuth/)

> ... OAuth는 이렇게 제각각인 인증방식을 표준화한 인증방식이다. OAuth를 이용하면 이 인증을 공유하는 애플리케이션끼리는 별도의 인증이 필요없다. 따라서 여러 애플리케이션을 통합하여 사용하는 것이 가능하게 된다.
>
> [https://ko.wikipedia.org/wiki/OAuth](https://ko.wikipedia.org/wiki/OAuth)

쉽게 말해 구글을 통해 로그인, 페이스북으로 로그인, 카카오로 로그인, ... 이런 얘들이 API 접근 위임 방식의 로그인인데, 이걸 표준화한 것이 OAuth임. 😎


## precision과 scale

정밀도(precision)와 스케일(scale)은 대체로 숫자의 표현 범위를 나타내는 용어다. 상세한 특징은 언어마다 차이가 있다.

ANSI SQL에서 정밀도는 표현가능한 자리값을, 스케일은 소수점 이하 자릿수를 의미한다.

자바는 SQL과 비슷하다. 예를 들어 `0.001`의 scale은 3이며, precision은 4이다. `BigDecimal` 타입에서 스케일 변경 API를 제공한다.

자바스크립트에선 스케일이 없고 정밀도`Number.prototype.toPrecision()`만 있는데, 실제 숫자의 자리수보다 정밀도가 작으면 소수점을 반올림한다. 만약 소수점이 없는데 정밀도가 여전히 자리수보다 작다면 정수부분을 지수표기법으로 표현한다. 반대로 정밀도가 더 크면 소수점 이하에 0을 덧붙인다. 특이한 점이 있는데 `(0.001).toPrecision(1)`의 결과는 `0`이 아니라 `0.001`이다. 정수부가 `0`이라서 그러는 모양...


## 지수 표기법 Exponential notation

![](/images/note-1-1.png)

과학적 표기법 혹은 과학적 기수법이라고도 한다(영어로는 scientific notation 혹은 scientific form). 너무 크거나 작은 숫자를 편하게 사용하기 위해 만들어졌다.

자바 double은 12345678을 1.2345678E7로 표현한다. 즉 1.23456789 x (10의7승)


## POJO

Plain Old Java Object, 오래된 방식의 간단한 자바 오브젝트


## serialization

직렬화, 객체의 스냅샷을 바이트 스트림으로 출력할 수 있는 기능. 파일이나 데이터베이스에 저장하거나 복잡한 객체 값에 의한 전달(pass-by value), 분산 환경에서 노드 간 애플리케이션의 상태를 복제하는데 사용된다. 직렬화된 스트림은 역직렬화를 하지 않으면 특정 데이터만 가져오는것이 불가능하다.


## UML

Unified Modeling Language, 통합모델링 언어. 소프트웨어의 산출(명세화, 시각화, 문서화) 표준을 정의한 범용 언어로 시각적 모델을 만들기 위한 도안 표기법을 포함한다.  
[http://www.slideshare.net/ohyecloudy/uml-1735845](http://www.slideshare.net/ohyecloudy/uml-1735845)


## 데이터베이스 스키마

데이터베이스에서 자료의 구조, 자료의 표현 방법, 자료 간의 관계를 정의한 것


## 컴포넌트

컴퓨터 소프트웨어에서 다시 사용할 수 있는 범용성을 위해 개발된 소프트웨어적인 부품을 일컫는다.


## OLE

윈도우의 각종 응용 프로그램 사이에서 서로 데이터를 공유할 수 있는 기능


## 미들웨어

데이터베이스, 스프레드시트, 윈도우OLE 등의 인터페이스를 준수하는 컴포넌트 기반의 소프트웨어. 톰캣도 미들웨어에 속한다.


## Servlet

자바를 사용하여 웹페이지를 동적으로 생성하는 서버 측 프로그램. 서버용 애플릿, 웹서버에서 실행되는 작은 자바 코드. JVM에서 실행되므로 플랫폼의 구애를 받지 않고, 웹서버와 충돌이 없으며 메모리 관리가 철저함. 웹브라우저에서 실행되지 않고 GUI로 구성되지 않는다는 점이 애플릿과 다름. 웹서버에서 실행되는 서블릿 엔진과 서비스 요청 및 이에 대한 반응 형태로 사용.


## 원시 타입과 참조 타입의 차이

**![primitive type](/images/note-1-2.png)**
**![reference type](/images/note-1-3.png)**


## 내 맴대로 정하는 용어

### 쪽 번호 매기기 pagination

- currentPage: 현재 페이지
- rowsPerPage: 한 페이지 당 출력할 게시물. 고정값으로 처리하거나 사용자가 선택한 값을 받는다.
- totalRows: total number of rows, 모든 데이터의 개수. `SELECT COUNT(*)`의 값을 의미한다.
- pageLength: totalPage, maxPage, 총 페이지 수를 의미하며 아래처럼 계산한다.
- indexPerPage: numberPerPage, Number of indexes per page, 한 페이지에서 표시 가능한 최대 인덱스 수.
- startNumber, endNumber: DB검색 시 사용될 한 페이지의 시작 값과 끝 값
