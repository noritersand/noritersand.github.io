---
layout: post
date: 2015-06-08 18:47:33 +0900
title: '[etc] 도토리 목록: 각종 개발 툴, 유틸리티 사이트, 언어, 라이브러리 모음'
categories:
  - etc
tags:
  - note
  - devtool
  - link
  - acorns
---

* Kramdown table of contents
{:toc .toc}


## 개요

쓰니는 다람쥐같은 습성이 있어서 일단 모으는 것을 좋아한다고 한다.

자주 쓰거나 중요한 것은 ⭐ 표시함.


## 분류 없음

- [stream](https://getstream.io/): 채팅 관련 오픈 소스 같은데 뭔지 잘 몲
- [Sanity](https://www.sanity.io/): CMS(Content Management System)라는데 이게 뭘까


## 각종 문서(메뉴얼, API Doc, 튜토리얼 등)

- [MDN](https://developer.mozilla.org/): 그 MDN
- [⭐ DevDocs](https://devdocs.io): 개발자용 API 문서 모음 사이트. [깃허브 링크](https://github.com/freeCodeCamp/devdocs)
- [⭐ Can I use](https://caniuse.com/): 웹 API, HTML, CSS 등을 어떤 브라우저에서 지원하는지를 알려주는 사이트.
- [WikiDocs](https://wikidocs.net): 온라인 책 제작 공유, 프로그래밍 언어별 튜토리얼이 있음.
- [0.30000000000000004.com](https://0.30000000000000004.com/): 부동 소수점에 대해 설명하는 문서
- [link anatomy](http://bl.ocks.org/abernier/3070589): `location` 해부학(?)
- [JS Is Weird](https://jsisweird.com/): 자바스크립트의 혼돈의 카오스 같은 여러 현상들을 퀴즈 형식으로 설명하는 사이트
- [The Concise TypeScript Book](https://github.com/gibbok/typescript-book): TypeScript의 모든 것을 정리한 문서.
- [ECMA International: ecma-262](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/): 비영리 국제 표준화 기구인 ECMA Internation의 사이트. 이 문서에서는 ECMAScript의 버전별 명세와 현재 유효한 표준을 볼 수 있다.
- [You might not need jQuery](https://youmightnotneedjquery.com/): jQuery API 대신 쓸 수 있는 자바스크립트 + CSS 코드를 알려주는 사이트. 예를 들어 `$(el).show()`를 찾으면, `el.style.display = ''`를 알려주는 식이다.
- [standard-readme](https://github.com/RichardLitt/standard-readme/blob/main/spec.md#specification): 누군가 만들어놓은 README 작성 표준

### HTTP 표준

- [rfc9110 \| HTTP Semantics)](https://www.rfc-editor.org/rfc/rfc9110.html)
- [rfc6648 \| Deprecating the "X-" Prefix and Similar Constructs in Application Protocols](https://datatracker.ietf.org/doc/html/rfc6648)
- [The W3C Markup Validation Service](https://validator.w3.org/): W3C에서 운영하는 걸로 보이는 마크업 검사기. 소스 입력 방법으로 URL, 파일, 직접 입력 세 가지를 제공한다. 그런데 이 검사기는 순수 HTML을 대상으로 작성된 검사기라서 리엑트 같은 프론트엔드용 프레임워크 소스를 검사하면 죄 틀렸다고 나온다. 자매품으로 [CSS Validator](https://jigsaw.w3.org/css-validator/)도 있다.


## 정규식 Regular Expression

- [regular expressions 101](https://regex101.com/): 정규식 테스트 겸 코드 공유 사이트. (근데 왜 101일까)
- [RegExr](https://regexr.com/): 정규식 테스트 사이트
- [REGEXPER](https://regexper.com/): 정규식을 (나름)이쁘게 설명해줌. 요딴식으로 `https://regexper.com/#^M[^iI]*%3F[iI][^iI]*%3F%24` URL에 정규식을 때려넣는게 특징
- [^grex$](https://pemistahl.github.io/grex-js/): 테스트 케이스를 작성하면 정규식을 만들어줌


## 유닉스/리눅스 Unix/Linux

- [Crontab.guru](https://crontab.guru/): Cron(리눅스/유닉스의 스케줄러) 표현식을 테스트하거나 랜덤으로 만들어주는 사이트. Cron Job의 모니터링 소프트웨어를 파는 [Cronitor](https://cronitor.io/cron-job-monitoring?utm_source=crontabguru&utm_campaign=cronitor_button)에서 운영한다.


## 개발환경 Development Environment

### 호스팅

- [ngrok](https://ngrok.com/): 로컬 구동 서버(localhost)를 외부에서 접속할 수 있게 해주는 (심자어 https로!) 서비스. 윈도우에선 chocolatey로 설치하고, 별도로 발급 받은 토큰을 `ngrok config add-authtoken MY_TOKEN` 명령으로 등록한 뒤 `ngrok http http://localhost:8080` 명령으로 로컬 서버와 연결하는 식으로 구동한다.

### 의존성 관리(라이브러리 설치/조회/삭제)

- [Chocolatey](https://chocolatey.org/): Windows OS용 패키지 관리 도구. NuGet 기반으로 만들어졌다 한다. 비슷한 것으로 MS 공식 툴인 [Winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/)이 있다.
- [Scoop](https://scoop.sh/): Chocolatey에 비해 규모와 사용자는 적지만, 개발자 커뮤니티가 매우 활성화되어 있다고 함.


## 프로그래밍 언어 Programming Language

- [Elixir](https://elixir-lang.org/): BEAM(Erlang의 가상머신) 위에서 실행되는 함수형 프로그래밍 언어. 동시성과 분산 처리에 강하며, 프로세스 간의 격리(한 프로세스의 실패가 시스템 전체에 영향을 주지 않음)를 통한 안정성이 특징이다. Erlang이 자바라면 Elixir는 코틀린에 비견된다. 디스코드는 실시간 메시징 처리를 Elixir로 구현했다고 함. 비결은 Erlang VM이 관리하는 Erlang 프로세스(OS의 프로세스나 쓰레드와 다른 개념)와 concurrency 지원 기능 덕분이라나...
- [Erlang](https://www.erlang.org/): 1980년대에 통신 시스템 구축을 위해 만들어진 언어(그래서 Erlang/OTP, Open Telecom Platform이라 함). 짧은 지연시간, 견고함, 내결함성, 분산 시스템 지원 등이 특징이다. 자바와 유사하게 다른 언어가 컴파일하는 가상머신(Erlang의 VM, BEAM이라 부른다)에서 작동한다. 
- [Rust](https://www.rust-lang.org/): 가비지 컬렉션(GC)을 사용하지 않으며 메모리 안전성(memory safe)을 추구하는 언어 #1. 마찬가지로 GC가 없지만 memory unsafe한 C와 C++의 대체제로 꼽힌다. 시스템 프로그래밍, 웹 어셈블리, 임베디드 시스템 등 다양한 분야에서 사용되는 범용 언어다. [Mozilla Research](https://research.mozilla.org/)에서 개발했다.
- [Ada](https://ada-lang.io/): GC를 사용하지 않으며 메모리 안전성을 추구하는 언어 #2. 항공, 방위, 우주 산업처럼 높은 수준의 안전성과 정확성이 요구되는 시스템에서 쓰인다고 한다.
- [Zig](https://ziglang.org/): GC를 사용하지 않으며 메모리 안전성을 추구하는 언어 #3. 셋 중 가장 최신 언어다(Ada의 표준화는 1983년, Rust의 최초 릴리즈가 2010년, Zig는 2015년에 개발 시작).
- [Scala](https://www.scala-lang.org/): 객체 지향 프로그래밍과 함수형 프로그래밍을 결합한 개발 언어. 왜인지 모르겠지만 개발자 설문조사 언어별 평균연봉 항목에서 늘 상위권을 차지한다. 자바(Java) 기반이며 JVM에서 실행된다.
- [Kotlin](https://kotlinlang.org/): JVM에서 실행되는 자바와 상호 운용 가능한 언어. 안드로이드 개발에 주로 쓰인다. 네이티브, 자바스크립트(?)까지 지원한다고 하며 기존의 자바 코드를 호환해줘서 그대로 사용할 수 있다고 한다.
- [Apache Groovy](https://groovy-lang.org/): JVM에서 작동하는 동적 타입 프로그래밍 언어(? 그게 뭔데). 자바, 파이썬(Python), 루비(Ruby) 등의 언어에서 영향을 받았다 한다. 
- [Go](https://golang.org): 한 때 세계에서 가장 돈을 많이 버는 프로그래밍 언어로 집계되기도 했으며 개발 속도와 실행 속도 둘 다 빠른 획기적인 언어라고 함. 언어 자체가 경량이라 늘 쓰던것만 쓴다는 소소한 단점이 있지만, 그만큼 빠르고 쉽게 익힐 수 있다고...
- [Dart](https://dart.dev/): 구글이 멀티 플랫폼 작동을 목적으로 만든 언어. 문법은 C와 비슷하다고 함. 자바처럼 DVM(Dart VM) 상에서 작동하거나 네이티브 컴파일을 따로 한다.
- [TypeScript](https://www.typescriptlang.org/): 타입스크립트. 자바스크립트의 슈퍼셋(superset)으로, 이름처럼 정적 데이터 타입이 추가되어 컴파일 에러 검출이 가능한 게 대표적인 특징이다. 컴파일 타임이 존재하며 자바스크립트 코드로 변환된다.
- [Mojo](https://www.modular.com/mojo): 파이썬의 슈퍼셋. 파이썬의 느린 속도를 개선했으며 저수준의 제어가 가능한 것이 특징. Python 3.x를 완벽하게 호환한다. 처음 소개 영상을 봤을 시점엔 waitlist 등록 후 기다려야 한다고 했는데 지금(2023-11-28)은 어떨지 몰?루
- [Jsonnet](https://jsonnet.org/): 환경 설정용 언어(A configuration language). 오직 JSON 데이터를 만들기 위한 언어로, 기존 JSON에선 불가능하던 변수 참조, 조건 분기, 함수, import 등의 기능을 사용할 수 있다.
- [Figstack](https://www.figstack.com/): 코드를 다른 언어로 번역, 영어로 해설, documentation comments 만들기, 시간 복잡도 계산, 작성한 코드 기반 자연어로 질문까지. 아직은 쪼끔 느린게 흠.
- [OneLang.io](https://ide.onelang.io/): 개발 언어 병렬 번역기


## 써드파티 라이브러리 Third-party Libraries

### 미분류

- [Apache Tika](https://tika.apache.org/): 파일 콘텐츠를 분석해주는 자바 라이브러리
- [Electron](https://electronjs.org/): 크로스 플랫폼 데스크탑 앱 개발 프레임워크. 오픈 소스고 자바스크립트 기반이다. VSCode, Atom, Notion desktop 등이 이걸로 만들어짐. 
- [Chromium](https://www.chromium.org/): 구글의 오픈 소스 웹 브라우저 프로젝트. 크롬의 기반 코드이며 요즘(2023-09-13) 점유율 높은 브라우저들은 대부분 Chromiun 코드베이스를 사용한다.
- [⭐ Netty](https://netty.io/): 자바 네트워크 앱 개발용 NIO(비동기 입출력) 클라이언트-서브 프레임워크
- [🌟 KeystoneJS](https://keystonejs.com/): 어드민 패널 라이브러리. 애플리케이션에 필요한 관리자 화면을 만들어주는 라이브러리다. 자바스크립트 혹은 타입스크립트로 사용할 수 있음. [니콜라스 유튜브 \| KeystoneJS 소개 영상](https://www.youtube.com/watch?v=DlyoFFOcPCg)
- [Mermaid](https://mermaid.js.org/): 간단한 텍스트 구문을 이용해 다이어그램을 생성해주는 자바스크립트 기반 라이브러리. 이런걸 Diagram as Code라고 한다. 플로우 차트, 간트 차트, 클래스 다이어그램, 깃 그래프, 시퀀스 다이어그램, 클래스 다이어그램, ERD 등을 지원한다. 더 자세한 내용은 [여기](https://mermaid.js.org/intro/)서 확인.

### 런타임

- [Node.js](https://nodejs.org/): 서버 사이드 자바스크립트 런타임
- [Deno](https://deno.land/): Node.js 개발자가 만든 자바스크립트와 Rust기반의 자바스크립트/타입스크립트 용 런타임. Node.js 개선 버전이라고 보면 됨.
- [Bun](https://bun.sh/): 최적화된 성능, 더 나은 메모리 관리 등이 목표인 서버 사이드 자바스크립트 런타임. Node.js, Deno보다 빠르다고 주장한다. 실제로 그런 것 같고, Node.js 대체제로 떠오르는 중(2023-09-11)...

### 유닛 테스트

- [JUnit](https://junit.org): 자바 테스팅 프레임워크. JUnit Platform + JUnit Jupiter + JUnit Vintage 세 개를 합친 JUnit 5 버전이 나왔음. (2023-02-15)
- [AssertJ Core](https://assertj.github.io/doc/): 테스트용 자바 라이브러리. Spring boot starter 라이브러리에 포함돼 있다. (사실 JUnit도 같이 있음) 들리는 말로는 요게 더 좋다고 함.
- [Mocha](https://mochajs.org/): 오래된 자바스크립트 테스팅 프레임워크 #1. 이하 모카
- [Chai](https://www.chaijs.com/): 자바스크립트용 Assertion 라이브러리. 가독성 좋고 유연한 문법을 제공한다. 보통 모카와 같이 쓴다.
- [Jest](https://jestjs.io/): 자바스크립트 테스팅 프레임워크 #2. Facebook에서 개발했다고 한다.(지금은 OpenJS 재단에서 관리하는 모양인데). 리액트 프로젝트에서 많이 쓰인다고 한다.

### 프레임워크

- [Tiles](https://tiles.apache.org/): 자바에서 사용하는 템플릿 프레임웍. 지금은 retired 상태라서 업데이트는 없다.
- [SiteMesh](https://struts.apache.org/plugins/sitemesh/): 타일즈와 같은 JSP 템플릿 프레임워크. 오래되긴 마찬가지긴 하지만 가장 최근에 썼었...던가?
- [Spring](https://spring.io/): 자바 백엔드의 대명사 격인 자바 서버 애플리케이션 개발 프레임워크
- [Vue](https://vuejs.org/): 뷰. 설명이 필요없는 프론트엔드 라이브러리 #1.
- [React](https://react.dev/): 리액트. 설명이 필요없는 프론트엔드 라이브러리 #2.
- [React Native](https://reactnative.dev/): 리액트 기반의 크로스 플랫폼 개발 프레임워크. 자바스크립트 코드 하나로 안드로이드와 iOS에서 작동하는 앱을 빌드할 수 있다.
- [Svelte](https://svelte.dev/): 리액트, 뷰를 잇는 프론트엔드 프레임워크. 비교적 가볍고 단순한 게 특징이다.
- [Flutter](https://flutter.dev/): 구글이 만든 UI 툴킷(SDK) 겸 크로스 플랫폼 개발 프레임워크. 지원되는 플랫폼은 Windows, macOS, 웹이다. 사용언어는 Dart
- [Next.js](https://nextjs.org/): 리액트 기반의 풀스택 프레임워크. SSR, SSG, CSR을 모두 지원한다. 주로 프론트엔드 개발에 사용된다.
- [Vite](https://vitejs.dev/): 모던 프론트엔드 프로젝트를 위한 빌드 툴. 리액트, 스벨트와 순수 자바스크립트를 모두 지원한다. 빠른 콜드 스타트, 핫 모듈 교체, 빌드 최적화, 플러그인 시스템 등이 특징이다.
- [NestJS](https://nestjs.com/): 타입스크립트 기반의 백엔드 애플리케이션(=API 서버) 구축을 위한 Node.js 프레임워크. 의존성 관리, 모듈화, 서버 사이드 렌더링, 웹소켓 등을 지원한다.
- [Gatsby](https://www.gatsbyjs.com/): 리액트 기반의 정적 사이트 생성(SSG, Static Site Generation) 프레임워크. 오픈 소스다.
- [Remix](https://remix.run/): 리액트 기반의 SSR/CSR 프레임워크. CSR보단 SSR로 주로 쓰이는 듯 하며, 백엔드에서 리액트를 실행하고 결과를 클라이언트에 전송하는 방식이다.

#### 리액트용 라이브러리

- [Million](https://million.dev/): 리액트를 빠르게 만들어준다고 함. (무려 70%)
- [Redux](https://redux.js.org/): 리덕스. 리액트용 상태(reactive state) 관리 라이브러리 #1. 모든 상태 변화가 중앙에서 관리되기 때문에 예측과 디버깅 등 유지보수에 도움이 된다고 한다. 리액트에서는 복잡한 계층 구조를 가진 컴포넌트들 사이에서 state의 변화를 전달할 때 *props drilling*이라 불리는 번거로운 작업이 필요한데, 리덕스는 이러한 작업을 간소화할 수 있는 라이브러리다.
- [zustand](https://github.com/pmndrs/zustand): 리액트용 상태 관리 라이브러리 #2. 리덕스와 마찬가지로 props drilling 문제를 방지하고 상태를 전역으로 관리할 수 있게 해준다. 리덕스보다 코드 작성이 간결하다는 평. 

### CSS 프레임워크

[2023 베스트 CSS 프레임워크 소개 \| 니콜라스 유튜브](https://www.youtube.com/watch?v=FRSUP2sbgTY)

- [Storybook](https://storybook.js.org/): 프론트엔드 워크샵이라고 한다(그게뭐야). UI 컴포넌트나 페이지를 만들 때 쓴다는데 아직 몲. 근데 가급적 빨리 써보는 게 좋을 것 같은 느낌적인 느낌
- [component.gallery](https://component.gallery/): CSS 프레임웍과 디자인 시스템 같은 것을 모아놓은 사이트
- [Bootstrap](https://getbootstrap.com/): 가장 유명하고 오래된 그 부트스트랩. 범용 프레임워크로 분류됨
- [⭐ Bulma](https://bulma.io/): Flexbox 기반
- [Foundation Framework](https://get.foundation/): **TODO**
- [Tailwind CSS](https://tailwindcss.com/): 미리 작성된 스타일링 클래스를 제공하는 방식. 유틸리티 기반 프레임워크로 분류됨
- [⭐ StyleX](https://stylexjs.com/): 메타(페북)에서 만듦. 테일윈드 경쟁자라 함. 자바스크립트 객체 기반으로 사용한다. 조건부 스타일 설정, 컴파일, 타입 안정성(type safe)이 특징이다. 리액트 없이도 쓸 수 있는 것으로 보임.
- [Open Props](https://open-props.style/): **TODO**
- [Basscss](https://basscss.com/): **TODO**
- [Water.css](https://watercss.kognise.dev/): **TODO**
- [MVP.css](https://andybrewer.github.io/mvp/): **TODO**
- [Materialize CSS](https://materializecss.com/): **TODO**
- [⭐ System.css](https://sakofchit.github.io/system.css/): 레트로 갬성. 애플 시스템 OS와 똑같다...고 한다.
- [NES.css](https://nostalgic-css.github.io/NES.css/): 닌텐도 스타일
- [PSone.css](https://micah5.github.io/PSone.css/): 플스 1 스타일
- [98.css](https://jdan.github.io/98.css/): 이건 윈도우 98
- [XP.css](https://nostalgic-css.github.io/NES.css/): 윈도우 XP
- [7.css](https://khang-nd.github.io/7.css/): 이건 윈도우 7
- [Bojier](https://bojler.slicejack.com/): 이메일용
- [Chota](https://jenil.github.io/chota/): **TODO**
- [Pico.css](https://picocss.com/): **TODO**
- [⭐ ChakraUI](https://chakra-ui.com/): React 애플리케이션을 위한 간결한 모듈식
- [Mantine](https://mantine.dev/): **TODO**
- [Ant Design](https://ant.design/): **TODO**
- [Material UI](https://mui.com/material-ui/): 리액트용 UI 컴포넌트 모음
- [NextUI](https://nextui.org/): **TODO**
- [리액트 UI](https://reach.tech/): **TODO**
- [Headless UI](https://headlessui.dev/): 리액트 혹은 뷰에 적용할 수 있음
- [@mdi/svg](https://www.npmjs.com/package/@mdi/svg): React나 Vue에서 갖다 쓰는 패키지로, 필요한 아이콘을 svg로 만들어줌. (아마도?)
- [Vitebook](https://vitebook.dev/): **TODO**
- [Quasar Framework](https://quasar.dev/): 뷰 전용인듯?
- [Vuetify](https://vuetifyjs.com/en/): 뷰용 컴포넌트 프레임워크

### UI 컴포넌트 라이브러리

그리드나 달력 등 UI 관련 라이브러리 중 단독으로 사용할 수 있는 것들 모음.

- [⭐ Air Datepicker](https://air-datepicker.com/): 프론트엔드용 달력 일명 데이트피커. 순수 자바스크립트 기반. 언어 기본값이 러시아어인 걸 보니 러시아산인 모양
- [Tom Select](https://tom-select.js.org/): 셀렉트박스. 순수 자바스크립트 기반이다.
- [AG Grid](https://www.ag-grid.com): 주변 사람이 추천한 유료 그리드
- [jqxgrid](https://www.jqwidgets.com/jquery-widgets-demo/demos/jqxgrid/index.htm)
- [Toast UI Grid](https://ui.toast.com/tui-grid): 줄여서 TUI Grid. NHN에서 만들었고 MIT 라이선스의 오픈 소스 그리드. 가볍게 쓰기 좋지만 깃허브 대응이 좀 많이 느리다. 4.21.9 버전 기준, 스크롤 기능에 버그가 있고, 제공되는 소팅 옵션이 너무 적어 서버 사이드 소팅 구현이 이상하다.
- [Fontello](https://fontello.com/): 아이콘을 폰트로 구현할 때 씀. 사이트에서 선택한 아이콘만 다운로드할 수 있음.
- [Reveal.js](https://revealjs.com/): HTML로 만드는 PPT

### 자바스크립트 라이브러리

- [Moment.js](https://momentjs.com/): 날짜와 시간을 다루는 자바스크립트 라이브러리 #1. 포매터가 필요하면 이걸 쓰자
- [Day.js](https://day.js.org/): 날짜와 시간을 다루는 자바스크립트 라이브러리 #2. 포매터 지원함. MUI의 Date picker에 적용된 라이브러리다.
- [Lodash](https://lodash.com/): 모듈 시스템을 지원하는 모던 자바스크립트 유틸리티 라이브러리. 성능 향상을 위해 쓰인다. 대표적으로 [lodash.debounce](https://lodash.com/docs/4.17.15#debounce)가 있음.


## 인프라

### PaaS

PaaS 중에 유명한 것들은 대체로 소스만 올리면 앱이 돌아가는 클라우드 서비스들이다.

- [Heroku](https://www.heroku.com/): 애플리케이션 개발과 배포를 위한 PaaS 서비스. 초반에 있었다는 일부 무료 정책(앱 5개까지 무료)은 없어진 모양이다. 다음 목록은 추천 받은 대체제들:
  - [Koyeb](https://www.koyeb.com/)
  - [Fly.io](https://fly.io/)
  - [Northflank](https://northflank.com/)
- [Kubernetes](https://kubernetes.io/): 통칭 K8s이라 쓰는 쿠버네티스. 컨테이너화된 애플리케이션을 관리하고 배포하기 위한 오픈소스 플랫폼이다. 기존 PaaS 범주와 비슷하지만 다르다고 한다. [공식 문서 링크](https://kubernetes.io/docs/concepts/overviw/). "컨테이너화된 애플리케이션을 배포, 관리, 확장할 때 수반되는 다수의 수동 프로세스를 자동화하는 오픈소스 컨테이너 오케스트레이션 플랫폼"이라 소개된다.
- [cloudtype](https://cloudtype.io/): 최스님이 알려줌. 안써봄
- [smolsite](https://smolsite.zip): 스몰사이트, ZIP으로 압축해서 업로드하면 무료로 호스팅 해줌
- [Vercel](https://vercel.com/): 프론트엔드용 클라우드 플랫폼. Next.js의 개발사이기도 하다. 정적 사이트와 Jamstack 이키텍처에 최적화되어 있다고 한다.

### IaaS

인프라만 제공하는 클라우드 서비스.

- [AWS](https://aws.amazon.com/): 아마존의 the AWS. 제한적인 무료 티어를 제공함
- [Google Cloud Platform (GCP)](https://cloud.google.com/?hl=ko)
- [Azure](https://azure.microsoft.com/)
- [Oracle Cloud Infrastructure (OCI)](https://www.oracle.com/kr/cloud/): 오라클 OCI. 얘네도 무료 티어가 있음
- [Pulumi](https://www.pulumi.com/}): 자바스크립트 코드로 클라우드 인프라를 관리하는 IaC\* 서비스. 개인 용도는 무료(2023-09-13)

\* IaC(Infrastructure as Code): 코드를 사용하여 수동 프로세스가 아닌 자동으로 인프라를 정의하고 관리하는 방법론의 일종

### BaaS, Backend as a Service

BaaS란 백엔드의 전반적인 기능을 제공하는 서비스를 의미함.

- [Firebase](https://firebase.google.com/): 구글의 그 파이어베이스
- [Supabase](https://supabase.com/): 오픈소스 백엔드 서비스. 인증, 데이터베이스, 파일 저장소, 서버리스 기능 등을 제공함. 파이어베이스에는 없는 RDBMS(PostgreSQL)을 제공한다고 함.
- [PocketBase](https://pocketbase.io/): 오픈소스 백엔드 서비스 #2. 실시간 데이터베이스, 인증, 파일 저장소, 어드민 대시보드를 파일 하나로 만들어준다. 다른 서비스와 다르게 클라우드가 아니라 서버를 직접 구축하는 방식이다.

### VPN

- [OpenVPN GUI](https://github.com/OpenVPN/openvpn-gui): OpenVPN 커뮤니티 버전의 클라이언트 앱. 공식 앱의 이름은 OpenVPN Connect이며 OpenVPN GUI와는 다르다. VPN 설정과 같은 네트웤에서도 앱에 따라 차이가 있을 수 있다. 예를 들어 OpenVPN Connect로 특정 VPN에 붙었더니 인터넷 먹통 현상이 발생했는데, OpenVPN GUI에선 같은 문제가 발생하지 않음.
- [Surfshark](https://surfshark.com): 접속 국가 변조용 유료 VPN

### CDN

- [cdnjs](https://cdnjs.com): Cloudflare에 의해 호스팅되는 무료 오픈 소스 CDN
- [JSDELIVR](https://www.jsdelivr.com): 역시 무료 CDN. npm과 GitHub에 최적화되어 있다고 한다.
- [UNPKG](https://unpkg.com/): npm으로 배포된 패키지를 불러올 수 있는 CDN. `unpkg.com/react@16.7.0/umd/react.production.min.js` 요런식으로 씀


## 플랫폼, 브로커

플랫폼과 브로커의 의미는 잘 모르겠지만... 🤭

- [Apache Kafka](https://kafka.apache.org/): 카프카. 아파치 소프트웨어 재단이 스칼라로 개발한 오픈소스 분산 스트리밍 플랫폼. '메시지 브로커 프로젝트' 혹은 '분산 환경에서 사용되는 데이터 스트리밍 플랫폼'으로 소개된다. 여러 데이터베이스의 동기화 작업이나, 비동기로 메시지를 주고 받을 때 사용한다(데이터 입출력도 여기에 해당함). 에이전트를 설치하는 방식이라 카더라.
- [RabbitMQ](https://www.rabbitmq.com/): 엠큐티티. AMQP를 구현한 오픈 소스 메시지 브로커. Erlang으로 만들어졌다(얼랭?). 사용자 요청의 비동기 처리, 앱 간 메시지 교환, 분산 시스템에서의 메시지 브로커링 등에 활용된다.


## 데이터베이스

### DBMS

- [SQLite](https://www.sqlite.org/index.html): RDBMS. 매우 가벼워서 보통 서버가 아닌 소프트웨어에 내장시키는 임베디드용으로 쓰인다. 근데 어떤 사람의 주장으로는 매우 안정적이라 프로시저 같은 추가 기능이 필요한 게 아니라면 서버용으로 써도 된다고. 시퀄라이트라고도 읽는다.
- [Redis](https://redis.io/): 인메모리 데이터베이스 중 가장 인지도 높은 그 레디스. 메모리에 데이터를 저장해서 속도가 빠른게 특징이며 디스크 백업 기능도 제공함.
- [SwayDB](https://swaydb.io/?language=java): 레디스같은 인메모리 데이터베이스. 레디스처럼 서버용은 아니고 임베디드로 쓰이는 모양?

### DBMS Tool

- [DBeaver](https://dbeaver.io): 벤더 안가리는 툴. 이클립스와 같은 프레임웍으로 추정.
- [QueryBox](http://www.querybox.com): 벤더 가리지 않고 접속할 수 있는 국산 툴. 기업용은 유료.
- [Flyway](https://flywaydb.org/): 데이터베이스의 버전 관리 툴. 마구잡이로 헤집어놓은 데이터도 원하는 시점으로 되돌리는 기능 등을 제공한다. 개발과 테스트를 위한 데이터베이스 구축에 매우 쓸만한 오픈 소스 도구.


## UML/MDA/다이어그램/마인드 맵/드로잉 툴

- [⭐ eraser](https://www.eraser.io/pricing): (엔지니어링 팀을 위한 화이트보드라 주장하는) 마크다운 노트와 드로잉 툴을 합체시킨 신박한 물건. 플로우 차트 그리기 굉장히 편하다. Diagram as Code(이걸로 ERD 그리기 가능)와 코멘트 기능도 있다. 무료 플랜 제공
- [Excalidraw](https://excalidraw.com/): 웹 전용 드로잉 툴. 무료 사용 가능. eraser에 비해 가볍게 쓰기 좋다.
- [Balsamiq](https://balsamiq.com/wireframes/): UML, 드로잉 툴. 이제는 아마 유료임.
- [StarUML](http://staruml.sourceforge.net/ko)
- [Draw.io (web)](http://www.draw.io)
- [Gliffy (web)](http://www.gliffy.com)
- [Fluent Icons](https://fluenticons.co/): 마소의 오픈 소스 아이콘 저장소. 마소가 만든건 아님. SVG 혹은 PNG로 받을 수 있다.
- [chart.xkcd](https://github.com/timqian/chart.xkcd): 자바스크립트로 만드는 차트. 결과물은 svg로 나옴. 발로 그린 것 같은 모양새가 특징(근데 그게 매력 터짐)
- [XMind](http://www.xmind.net): 마인드 맵
- [FreeMind](http://freemind.sourceforge.net/wiki/index.php/Main_Page): 마인드 맵 #2

### ERD

- [dbdiagram.io](https://dbdiagram.io/): 데이터베이스 ERD 전용. 웹 버전만 있긴 하지만 좋음.


## 온라인 코드 편집기(에디터) 겸 테스트 툴

- [CodePen](https://codepen.io): 온라인 코드 편집기. 웹으로 코드를 작성하고 테스트하거나 남들과 공유할 수 있는 서비스.
- [JSFiddle](https://jsfiddle.net): 온라인 코드 편집기 #2
- [CodeSandbox](https://codesandbox.io): 온라인 코드 편집기 (3). 웹으로 직접 작성 말고도 [CLI 업로드](https://www.npmjs.com/package/codesandbox)를 지원한다.
- [StackBlitz](https://stackblitz.com): 프론트엔드 개발용 온라인 코드 편집기 (4). 깃허브 저장소와 연동할 수 있고, Node.js 등 빌드 툴 실행이 가능한 환경을 제공한다. public 프로젝트에 한해 무료로 사용 가능.


## CVE 아카이빙 사이트

- [MITRE \| CVE](https://cve.mitre.org)
- [CVE.report](https://cve.report)
- [DevHub Advisory](https://devhub.checkmarx.com/advisories/): mitre 보다 읽기 좋은 형식으로 설명해주는 사이트인데, 이런식으로 [https://devhub.checkmarx.com/cve-details/CVE-2016-1000027](https://devhub.checkmarx.com/cve-details/CVE-2016-1000027) 맨 뒤 path만 바꿔서 조회하면 편하다.


## 일정/TODO 관리

- [⭐ workflowy](https://workflowy.com): 온라인 TODO 툴
- [Calendly](https://calendly.com): 이메일로 일정 맞추기
- [Linear](https://linear.app/)
- [Markwhen: Project planning example](https://markwhen.com/): 코드로 프로젝트 일정 표를 만드는 사이트


## 통계

- [Worldometer](https://www.worldometers.info): 전 세계의 각종 정보 실시간 카운터. 인구 증감, 남은 석유량 등
- [statcounter](https://gs.statcounter.com): 전 세계 대상 브라우저, OS 점유율 등의 통계자료 조회 사이트
- [A/B Testing Significance Calculator](https://neilpatel.com/ab-testing-calculator): A/B 테스트 통계적 유의성 계산기. 신뢰도를 계산해주는 사이트인가보다. [통계의 신뢰구간과 신뢰도](https://m.blog.naver.com/PostView.nhn?blogId=baboedition&logNo=220916281966&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
- [built with: trends](https://trends.builtwith.com/): 웹/인터넷 기술 사용 트랜드와 통계 보기. 여기는 원래 특정 사이트가 뭘로 만들어진건지를 분석해주는 사이트임.


## 디자인

- [Figma](https://www.figma.com): 요즘(2021-05-03) 뜬다는 UI 디자인 툴. 기본은 무료고, 대-충 비공개 프로젝트를 여러명이 사용할 땐 유료인듯
- [ThemeForest](https://themeforest.net/): 언어, 엔진, 프레임워크 별 테마(HTML과 CSS 묶음. 필요하면 JS까지) 파는 사이트
- [loading.io](https://loading.io/): 로딩 이미지, 패턴, 텍스트 등을 받을 수 있는 사이트. 무료버전인 경우 색 정도밖에 못바꿈.
- [미리캔버스](https://www.miricanvas.com/): 자칭 디자인 플랫폼. 템플릿 활용할 수 있는 간편한 디자인 툴을 제공하며 맞춤 디자인 의뢰도 가능함.
- [https://dev.to/jon_snow789/awesome-list-of-free-css-generator-293k](https://dev.to/jon_snow789/awesome-list-of-free-css-generator-293k): CSS 코드 생성기 모음


## 공인 IP 확인

- [https://icanhazip.com](https://icanhazip.com): 내 공인 IP만 텍스트로 응답하는 사이트.
- [https://whatismyipaddress.com](https://whatismyipaddress.com): 접속한 PC의 IP 관련 정보를 보여주는 사이트 #1. 예전에 bot이 있었는데 사라짐.
- [https://ipinfo.io](https://ipinfo.io): 접속한 PC의 IP 관련 정보를 보여주는 사이트 #2


## 버전 관리

- [Fork](https://fork.dev): Git GUI #1. 가볍고 그래프가 보기 좋은게 특징인 툴. 무료지만 후원 방식으로 라이선스 구입이 가능하다. 구입하면 보상은 하트 ❤
- [GitKraken](https://www.gitkraken.com): Git GUI #2. 속도는 느리지만 편의성은 탑. 그런데 사설 서버 혹은 비공개 저장소는 유료버전이 아니면 사용할 수 없다. 😩
- [⭐ Sublime Merge](https://www.sublimemerge.com): Git GUI (3). 지원하는 기능은 Fork나 GitKraken에 비해서 딸리지만 속도가 CLI 수준으로 빠르다.
- [gitui](https://github.com/extrawurst/gitui): Git GUI (4). Mdir(?) 스타일의 GUI 툴. 옛날 갬성이 좋으면 쓸만하지만...


## 트래픽 캡쳐

- [Fiddler](http://www.telerik.com/fiddler): 네트워크 디버깅 툴. 파폭은 SSL 인증서 때문에 빡칠 수 있다. 유료
- [Wirehsark](https://www.wireshark.org/download.html): 패킷 추적 툴.


## 코드 하이라이팅/컬러링

- [prismjs](https://prismjs.com): 코드 하이라이팅 JS 라이브러리. HTML로 작성된 페이지는 어디든 적용할 수 있음. [깃허브](https://github.com/PrismJS/prism)
- [Color Scripter](https://colorscripter.com): 입력한 텍스트에 CSS를 적용해 HTML로 만들어주는 사이트. 코드 블록을 지원하지 않는 메일이나 게시판에서 사용하기 좋다.


## 텍스트 에디터

- [⭐ Sublime Text](https://www.sublimetext.com/blog/articles/sublime-text-4)
- [Notepad++](https://notepad-plus-plus.org)
- [UltraEdit](http://www.ultraedit.com/loc/ko/index_ko.html)
- [Nova](https://nova.app)
- [⭐ Visual Studio Code](https://code.visualstudio.com): 요즘 대세
- [Obsidian](https://obsidian.md/): 신개념 텍스트 에디터. 마크다운과 다이어그램을 기본으로 지원하고, 노트끼리 링크로 연결할 수 있다. 작성된 노트 기반으로 자동 생성되는 마인드맵 기능도 제공한다. 또 사용자 플러그인 설치가 가능한 점, 오프라인 파일이 생성되어 백업이 쉽다는 점이 있다. 동기화 기능(Obsidian Sync)은 유료다.


## SSH/텔넷/FTP 클라이언트

사실 윈도우 10부터는 Windows Terminal이 있어서 이런 거 필요 없지만... 🤭

- [WinSCP](https://winscp.net/eng/download.php)
- [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty): SSH = putty
- [Xshell](http://www.netsarang.co.kr/download/main.html): 가장 좋으나 유료 라이선스.
- [bitvise](https://www.bitvise.com/ssh-client-download): 서버 딱 하나에 붙는 용도로는 아주 좋다. 여러 서버에 붙으려면 매번 프로필들을 불러와야 해서 불편.
- [MobaXterm](https://mobaxterm.mobatek.net/download.html): 무료 툴 중에서 여러 서버 동시 접속 기능은 그나마...


## Mobile development

- [troy (web)](http://troy.labs.daum.net)
- [Adobe Edge Inspect](https://creative.adobe.com/ko/products/inspect)
- [browser-deeplink](https://github.com/hampusohlsson/browser-deeplink): 브라우저에서 앱 실행


## JSON viewer/editor

- [jsoneditoronline.org](https://www.jsoneditoronline.org)
- [jsonviewer.codeplex.com](https://jsonviewer.codeplex.com)


## IDE

- [Eclipse](http://www.eclipse.org/downloads)
- [IntelliJ](http://www.jetbrains.com/idea/download/index.html)
- [jEdit](http://www.jedit.org/index.php?page=download)
- [NetBeans](https://netbeans.org/downloads/index.html)
- [Cloud9 (WEB)](https://c9.io)
- [WebStorm](http://www.jetbrains.com/webstorm/download/index.html)
- [Aptana](http://www.aptana.com/products/studio3/download)
- [ZED](http://zedapp.org)


## 디컴파일러 decompiler

- [JD Project](http://java-decompiler.github.io): 자바 디컴파일러. Java 1.1.8 부터 12까지 지원한다고 한다(2020-01-06 기준).


## Web server/WAS

- [Apache HTTP Server](https://www.apachelounge.com/download): 그 아파치. 검색으론 다운로드 링크 찾기 힘듬.


## 프로젝트 관리 도구 

Project manager 혹은 Issue tracker

- [Trello](https://trello.com)
- [Taiga](https://taiga.io)
- [JIRA](https://ko.atlassian.com/software/jira): 10개의 계정과 1GB까지 무료. 설치형은 없어


## Office

- [WPS Office](https://www.wps.com): 무료 플랜은 기본 기능과 1GB 클라우드 사용 가능.
- [LibreOffice](http://ko.libreoffice.org/download)


## 메일 서비스/메일 서버

- [stibee](https://www.stibee.com/?utm_source=stibee&utm_campaign=sponsorbanner&utm_medium=email&uid=b2ZmaWNpYWxAbmV3bmVlay5jbw): 이메일 마케팅 서비스. 읽기 쉬운 메일 작성 지원. 메일링.
- [https://www.guerrillamail.com/ko](https://www.guerrillamail.com/ko): 일회용 이메일 서버. 60분 동안만 유효한 메일 주소를 제공.
- [https://www.sharklasers.com](https://www.sharklasers.com): 일회용 이메일 서버 #2. 이건 꽤 오래감.
- [Firefox Relay](https://relay.firefox.com): 파폭 계정으로 사용한 이메일에 별칭을 만들 수 있고, 해당 별칭으로 메일이 오면 파폭이 포워딩 해줌. fㅔ이크 이메일을 진짜처럼 쓸 수 있게 해주는 것.


## 테스트 툴

- [postman](https://www.getpostman.com): HTTP Request/Response 테스트
- [Selenium](https://www.selenium.dev/): 동적 웹 앱(페이지 로딩 후 자바스크립트 등에 의해 동적으로 구성이 변경되는 사이트) 테스트 혹은 크롤링에 사용되는 툴이다.


## APM(Application Performance Management)/성능 분석/프로파일링/데이터 시각화

통칭 모니터링 툴 모음. 얘네들은 웬만하면 상용툴이다.

- [Datadog](https://www.datadoghq.com): 데이터독. 인프라 모니터링. APM 기능도 있지만 시스템 성능 지표 분석 기능이 주력이다. 설치형이 아니라 데이터는 저쪽에서 관리하며, 비싸다.
- [제니퍼](https://jennifersoft.com/ko/product/java): 자바앱 모니터링
- [VisualVM](https://visualvm.github.io): 프로파일링 툴#1. 자바 앱용. VM의 환경, CPU와 메모리의 사용량, 클래스와 쓰레드의 점유율, CPU/메모리/JDBC 프로파일링 등의 기능을 제공한다. 오픈 소스
- [Eclipse Memory Analyzer](https://www.eclipse.org/mat/downloads.php): 프로파일링 툴 #2. 자바 앱용. VisualVM보다 기능이 조금 더 많고 도움말이 잘 되어 있어서 쓰기 편함. UI는 이클립스 기반임. 오픈 소스
- [XRebel](https://www.jrebel.com/products/xrebel): 프로파일링 툴 (3). 자바 웹 앱 전용이다. 상용
- [JProfiler](https://www.ej-technologies.com/products/jprofiler/overview.html): 프로파일링 툴 (4). 10일 무료. IDE와 연동할 수 있음. 상용
- [YourKit Java Profiler](https://www.yourkit.com/java/profiler/features): 프로파일링 툴#5. 이것도 IDE 연동 쌉가능. 단, Java 1.7 미만의 환경은 지원하지 않으며 상용이다.
- [Jennifer](https://jennifersoft.com/ko/product/java): APM 툴 #2. 자바/PHP/닷넷 앱 모니터링. 상용
- [WhaTap](https://www.whatap.io/ko): APM 툴 (3). 앱/서버/DB/URL/컨테이너/인프라 모니터링. 상용이며 한국기업이라 한국어판을 제공한다.
- [⭐ Pinpoint](https://pinpoint-apm.gitbook.io/pinpoint/): 오픈 소스 APM. 네이버에서 만들었다 함
- [Scouter](https://github.com/scouter-project/scouter): 오픈 소스 APM. LG CNS랑 관련이 있나 봄. 이거 만든 사람들이 WhaTap 만들었다고 하던디...?
- [Grafana](https://grafana.com/): 메트릭/로그 시각화 툴. 오픈 소스다. 메트릭이란 주기적으로 발생하는 타임스탬프를 포함한 수치 데이터라고 한다.
- [Kibana](https://www.elastic.co/kr/kibana/): Elastic Stack의 일부인 데이터 시각화 및 분석 도구. 모니터링과 APM 기능도 제공됨.


## 네트워크 Network

### 인증서

- [Let's Encrypt](https://letsencrypt.org/): 비영리 단체인 [ISRG, Internet Security Research Group](https://www.abetterinternet.org/)에서 제공하는 TLS(SSL) 인증서 무료 발급 사이트. 
- [certbot](https://certbot.eff.org/): *Let's Encrypt*에서 발급하는 TLS 인증서를 쉽게 관리하게 도와주는 오픈 소스 소프트웨어. [EFF(Electronic Frontier Foundation)](https://www.eff.org/)에서 만들었고 인증서 자동 갱신 같은 작업을 지원한다.


## 브라우저 플러그인

### 파이어폭스

- Mate Translate
- PocketTube
- Save webP as PNG or JPEG
- uBlacklist
- uBlock Origin

### 크롬

- Go Back With Backspace
- ~~Move Tab Hotkeys~~
- Mate Translate
- Chrome Remote Desktop

여기에 개발용 크롬이면:

- CSSViewer
- JSON Formatter
- Vue.js devtools
- Wappalyzer
- Authenticator(인증 도구)


## 결제

- [stripe](https://stripe.com): 결제 대행 웹 앱. 해외판 PG사라고 생각하면 된다. 노션과 험블번들에서 쓰더라.
- [PortOne](https://portone.io/korea/ko): 30분이면 된다는 통합 결제 서비스. 이 서비스는 PG의 PG에 가깝다(?).
- [i'mport](https://www.iamport.kr): 아임포트. 결제/본인인증 중계 서비스. 예를 들면 쇼핑몰과 PG사(혹은 신용조회회사)의 중간에 위치한다고 보면 됨.


## AI

- [ChatGPT](https://chat.openai.com/): OpenAI 사의 GPT 기반 대형 언어 모델 #1
- [GPTForge](https://gptforge.net/): GPT를 활용한 웹앱, 툴, 앱 등을 모아놓은 사이트. 누가 따로 모으는 게 아니라 만든 사람들이 껴달라고 신청하는 것 같다.
- [FUTUREPEDIA](https://www.futurepedia.io/): AI 관련 툴 모음 사이트
- [Teachable Machine](https://teachablemachine.withgoogle.com/): 구글 티처블 머신. 초등학생도 사용할 수 있는 웹 기반 머신 러닝 툴이다. 아직(2023-12-28)은 오디오나 이미지 정도만 지원함.
- [Claude](https://claude.ai/): Anthropic 사의 GPT 기반 대형 언어 모델 #2. 발음은 '클로드'. 스스로 주장하기를 ChatGPT보다 성능이 좋다고 함.


## 기타 웹 서비스

- [⭐ JSON Placeholder](https://jsonplaceholder.typicode.com/): JSON 응답을 받아야하는데 백엔드를 만들기 귀찮으면 쓰는 Free Fake JSON API 서버.
- [⭐ Small Dev tools](https://smalldev.tools/): 인코딩/디코딩, 포매터, 테스트 데이터 생성 등 개발에 필요한 웹 도구 모음.
- [Itty bitty](https://itty.bitty.site): 간단한 서식의 글을 작성하고 URL로 공유하는 사이트. 데이터베이스를 사용하지 않고 URL에 작성한 글 내용이 모두 담겨있는 게 특징. 설명서는 [여기에](https://github.com/alcor/itty-bitty/wiki/).
- [TypeForm](https://www.typeform.com): 설문 조사용 웹 사이트. 여태 봤던것 중 가장 깔끔. 유료일듯?
- [⭐ Chatbase](https://www.chatbase.co/): 웹 사이트에 위젯처럼 간단히 추가할 수 있는 AI 챗봇.
- [GitBook](https://www.gitbook.com/): 마크다운으로 웹 문서 만드는 사이트. 웹에서 직접 에디트도 가능하지만 도저히 쓸 물건이 아니라서(다국어 입력하다 보면 먹통됨) 마크다운이나 노션으로 작성한 후 복붙해야 됨. 문서 버전 관리보단 완성된 결과물의 출판용으로 적합한 서비스.
- [ON24](https://www.on24.com/): 웨비나(Webinar, 웹 세미나) 서비스 사이트. Why Slack에서 쓰길래 줍줍
- [Firefox Monitor](https://monitor.firefox.com): 다른 사이트 가입할 때 사용한 내 계정 정보가 털렸는지 안털렸는지 알려줌
- [evanw: Source Map Visualization](https://evanw.github.io/source-map-visualization/): 자바스크립트 소스 맵 시각화 툴
- [sokra: source-map-visualization](https://sokra.github.io/source-map-visualization/): 자바스크립트 소스 맵 시각화 툴 #2
- [Meta Tags](https://metatags.io/): 메타 태그 만들어주는 사이트.


## non-dev 유틸리티

- [Microsoft PowerToys](https://learn.microsoft.com/en-us/windows/powertoys/install): 윈도우 고급 사용자를 위한 유틸리티 모음. Color Picker, 항상 위, 마우스 찾기, PowerToys Run(윈도우판 Spotlight) 같은 기능을 추가해준다. 그 중 가장 쩌는건 **PowerRename**(이제 파일명 바꾼다고 코딩 안해도 됨 🥹)
- [Caffeine](https://www.zhornsoftware.co.uk/caffeine/index.html): ~~월급루팡의 필수품~~ PC가 절전 모드 혹은 화면 보호기 모드로 바뀌지 않게 해주는 앱. 개발사는 Zhorn Software.
- [RunCat](https://github.com/Kyome22/RunCat_for_windows/releases): CPU 사용량이 높을 수록 다리가 빨라지는 고양이. 트레이에 거주함.
- [스텔라리움](https://stellarium.org/ko/): 스텔라륨. 오픈 소스 천체 투영관
- [SoundSwitch](https://soundswitch.aaflalo.me): 오디오 장치가 둘 이상일 때 출력 선택을 단축키로 변경할 수 있음
- [Meld](https://meldmerge.org/): 윈도우 용 GUI diff 앱. 파일 비교 후 머지까지 할 수 있고 3-way merge도 가능. macOS는 아직 지원 안함.
- [FFmpeg](https://ffmpeg.org/): CLI 방식의 동영상 변환/편집 툴. [니콜라스 유튜브 \| FFmpeg 소개 영상](https://www.youtube.com/watch?v=z2iodiQW0fg)

### 컴퓨터 관리

- [구라제거기](https://teus.me): 악성 코드 제거 툴
- [CPU-Z](https://www.cpuid.com/softwares/cpu-z.html): 설치된 컴퓨터의 하드웨어 스펙 조회 유틸리티. 칩셋, 캐시, 메인보드, 메모리, 그래픽카드의 모델명과 스펙이 표시됨.
- [HWMonitor](https://www.cpuid.com/softwares/hwmonitor.html): 하드웨어 모니터링 유틸리티. 쿨링 팬 속도, 사용 전압, 온도 등을 실시간으로 보여 줌.
- [OCCT](https://www.ocbase.com/download): 오버클럭 테스트용으로 사용하는 과부하 앱인데, 밴치마킹, 모니터링, 안정성 테스트, 시스템 정보 확인 등의 기능도 제공한다.
- [한국표준과학연구원: 표준시각 맞추기](https://www.kriss.re.kr/menu.es?mid=a10305020000): 한국표준과학연구원에서 제공하는 현재 시간 및 세계 시간 확인/동기화 유틸리티. 설치형이고 앱 이름은 UTCk, 현재(2023-08-09) 버전은 3.1
- [Revo Uninstaller](https://www.revouninstaller.com/): 앱을 삭제할 때 레지스트리 같은 일종의 찌꺼기(?)도 완전히 삭제해준다는 언인스톨러. 상용인 Pro 버전이 따로 있음.

### 인풋 매크로, 키 매핑 등

- [autohotkey](https://www.autohotkey.com): 스크립트로 작성하는 매크로
- [joytokey](https://joytokey.net/en): 게임패드-키보드(와 마우스) 매핑 앱
- [REWASD](https://www.rewasd.com/map-xbox-elite): 게임패드-키보드(와 마우스) 매핑 앱. 매크로와 터보 기능은 유료다.

### Presentation

- [Prezi(web)](http://prezi.com)
- [Slideshare(web)](http://www.slideshare.net)

### 이미지, 비디오, 오디오

- [⭐ paint.net](https://www.getpaint.net): 이미지 편집기. 좋음
- [VLC media player](https://www.videolan.org/vlc/): 기본 기능에 충실한 미디어 플레이어
- [Audacity](https://www.audacityteam.org/download/): 간단한 음원 편집기
- [PDF2JPG](https://pdf2jpg.net): PDF를 JPG로 변환
- [Segment Anything](https://segment-anything.com/): AI로 만든 자동 누끼(?) 앱이라는데 아직 안 써봄. 일단 깃허브 설명을 보면 파이썬으로 실행하는 모양
