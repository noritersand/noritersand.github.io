---
layout: post
date: 2015-06-08 18:47:33 +0900
title: '[etc] 🌰🐿️ 각종 개발 도구, 유틸리티 사이트, 언어, 라이브러리 모음'
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

자주 사용하거나 중요한 것은 ⭐표시함.


## 1. 문서, 메뉴얼, 참고 자료

### 일반

- [MDN Web Docs](https://developer.mozilla.org/): Mozilla Developer Network의 개발자를 위한 기술 문서.
- [⭐DevDocs](https://devdocs.io): 개발자용 API 문서 모음 사이트. [깃허브 링크](https://github.com/freeCodeCamp/devdocs)
- [WikiDocs](https://wikidocs.net): 온라인 책 제작 공유, 프로그래밍 언어별 튜토리얼이 있음.
- [The Concise TypeScript Book](https://github.com/gibbok/typescript-book): TypeScript의 모든 것을 정리한 문서.
- [ECMA International: ecma-262](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/): 비영리 국제 표준화 기구인 ECMA Internation의 사이트. 이 문서에서는 ECMAScript의 버전별 명세와 현재 유효한 표준을 볼 수 있다.
- [You might not need jQuery](https://youmightnotneedjquery.com/): jQuery API 대신 쓸 수 있는 JavaScript + CSS 코드를 알려주는 사이트. 예를 들어 `$(el).show()`를 찾으면, `el.style.display = ''`를 알려주는 식이다.
- [standard-readme](https://github.com/RichardLitt/standard-readme/blob/main/spec.md#specification): README 작성 표준
- [컨퍼런스 비디오](https://conference-view.vercel.app/): 개발 컨퍼런스 발표 영상 모음. 🧪 누군가의 토이 프로젝트임

### JavaScript

- [⭐Can I use](https://caniuse.com/): 웹 API, HTML, CSS 등을 어떤 브라우저에서 지원하는지를 알려주는 사이트.
- [0.30000000000000004.com](https://0.30000000000000004.com/): 부동소수점에 대해 설명하는 문서
- [link anatomy](http://bl.ocks.org/abernier/3070589): `location` 해부학(?)
- [JS Is Weird](https://jsisweird.com/): JavaScript의 혼돈의 카오스 같은 여러 현상들을 퀴즈 형식으로 설명하는 사이트

### HTTP 표준

- [rfc9110 \| HTTP Semantics)](https://www.rfc-editor.org/rfc/rfc9110.html)
- [rfc6648 \| Deprecating the "X-" Prefix and Similar Constructs in Application Protocols](https://datatracker.ietf.org/doc/html/rfc6648)
- [The W3C Markup Validation Service](https://validator.w3.org/): W3C에서 운영하는 걸로 보이는 마크업 검사기. 소스 입력 방법으로 URL, 파일, 직접 입력 세 가지를 제공한다. 그런데 이 검사기는 순수 HTML을 대상으로 작성된 검사기라서 React 같은 프론트엔드용 프레임워크 소스를 검사하면 죄 틀렸다고 나온다. 자매품으로 [CSS Validator](https://jigsaw.w3.org/css-validator/)도 있다.

### 보안

- [MITRE \| CVE](https://cve.mitre.org): 공통 취약점 아카이빙 사이트
- [CVE.report](https://cve.report)
- [DevHub Advisory](https://devhub.checkmarx.com/advisories/): mitre 보다 읽기 좋은 형식으로 설명해주는 사이트인데, 이런식으로 [https://devhub.checkmarx.com/cve-details/CVE-2016-1000027](https://devhub.checkmarx.com/cve-details/CVE-2016-1000027) 맨 뒤 path만 바꿔서 조회하면 편하다.


## 2. 유닉스/리눅스 Unix/Linux

- [Crontab.guru](https://crontab.guru/): Cron(리눅스/유닉스의 스케줄러) 표현식을 테스트하거나 랜덤으로 만들어주는 사이트. Cron Job의 모니터링 소프트웨어를 파는 [Cronitor](https://cronitor.io/cron-job-monitoring?utm_source=crontabguru&utm_campaign=cronitor_button)에서 운영한다.


## 3. 개발 도구

### 코드 에디터, IDE

- [⭐Sublime Text](https://www.sublimetext.com/blog/articles/sublime-text-4): 윈도우 기본 노트패드를 완벽히 대체 가능한 앱. 빠르고 가볍고 멀티 캐럿을 지원한다. 무료 버전은 가끔 저장할 때 팝업이 뜬다.
- [⭐Notepad++](https://notepad-plus-plus.org): 좀 오래됐지만 가볍게 쓰기 좋음. 서브라임으로 열었을 때 앱이 멈춰버릴 정도로 큰 파일도 노트패드++ 에선 잘 열린다.
- [⭐Visual Studio Code](https://code.visualstudio.com)
- [Obsidian](https://obsidian.md/): 신개념 텍스트 에디터. 마크다운과 다이어그램을 기본으로 지원하고, 노트끼리 링크로 연결할 수 있다. 작성된 노트 기반으로 자동 생성되는 마인드맵 기능이 있다. 💰작성한 파일의 클라우드 동기화 기능(Obsidian Sync)은 유료다.
- [Cursor](https://www.cursor.com/): AI 기반 코드 에디터. 일렉트론 + 모나코 에디터 기반이라 VSCode와 인터페이스가 매우 유사하다.
- [Zed](http://zedapp.org): 윈도우는 아직(2025-02-05) 지원 예정이라 기대만 하고 있는 텍스트 에디터. 특징은 고성능, 경량, 실시간 협업 기능이다.
- [IntelliJ](http://www.jetbrains.com/idea/download/index.html): Java 한정 최강. 단점은 💰유료
- [WebStorm](http://www.jetbrains.com/webstorm/download/index.html): JS/TS 웹 개발에 최적화된 IDE. 원래는 유료였다가 2025년에 무료로 풀림

### 온라인 코드 에디터, 스니펫

- [CodePen](https://codepen.io): 온라인 코드 편집기 #1. 웹으로 코드를 작성하고 테스트하거나 남들과 공유할 수 있는 서비스.
- [JSFiddle](https://jsfiddle.net): 온라인 코드 편집기 #2
- [CodeSandbox](https://codesandbox.io): 온라인 코드 편집기 #3. 웹으로 직접 작성 말고도 [CLI 업로드](https://www.npmjs.com/package/codesandbox)를 지원한다.
- [StackBlitz](https://stackblitz.com): 온라인 코드 편집기 #4. 프로트엔드에 특화되어 있다. 깃허브 저장소와 연동할 수 있고, Node.js 같은 런타임 환경을 제공한다. public 프로젝트에 한해 무료로 사용 가능.
- [GitHub Gist](https://gist.github.com/): 코드 스니펫 공유용 앱 #1. 사용하려면 깃허브 아이디 필요
- [Pastebin](https://pastebin.com/): 코드 스니펫 공유용 앱 #2. 로그인 필요함
- [Hastebin | Toptal](https://www.toptal.com/developers/hastebin): 코드 스니펫 공유용 앱 #3. 로그인 필요 없음
- [Snippet.host](https://snippet.host/): 코드 스니펫 공유용 앱 #4. 로그인 필요 없음
- [Carbon](https://carbon.now.sh/): 코드 스니펫 공유용 앱 #5. 이쪽은 코드를 이미지로 만들어주는 사이트다.

### 정규식 Regular Expressions

- [regular expressions 101](https://regex101.com/): 정규식 테스트 겸 코드 공유 사이트. (근데 왜 101일까)
- [RegExr](https://regexr.com/): 정규식 테스트 사이트
- [REGEXPER](https://regexper.com/): 정규식을 (나름)이쁘게 설명해줌. 요딴식으로 `https://regexper.com/#^M[^iI]*%3F[iI][^iI]*%3F%24` URL에 정규식을 때려넣는게 특징
- [^grex$](https://pemistahl.github.io/grex-js/): 테스트 케이스를 작성하면 정규식을 만들어줌

### 의존성 관리(라이브러리 설치/조회/삭제)

- [Chocolatey](https://chocolatey.org/): Windows OS용 패키지 관리 도구. NuGet 기반으로 만들어졌다 한다. 비슷한 것으로 MS 공식 도구인 [Winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/)이 있다.
- [Scoop](https://scoop.sh/): Chocolatey보다 규모와 사용자는 적지만, 개발자 커뮤니티가 매우 활성화되어 있다고 함.
- [uv](https://github.com/astral-sh/uv): `pip`, `virtualenv`, `pip-tools`를 대체하는 Python의 패키지 매니저. 의존성 관리와 가상 환경 관리 등을 지원한다.

### SDK Manager

- [Node Version Manager](https://github.com/nvm-sh/nvm): Node.js의 설치/버전 관리 도구. 여러 버전의 Node.js를 설치하고 지금 사용하려는 버전으로 손쉽게 전환할 수 있게 해준다. 통칭 nvm
- [SDKMAN](https://sdkman.io/): nvm처럼 여러 버전의 SDK를 설치하고 특정 버전으로 전환하는 기능을 제공하는 SDK 관리 도구다. 지원하는 SDK로 Java, Scala, Groovy, Kotlin, Ceylon, Gradle, Maven 등이 있다. 단점으로, SDKMAN을 실행하려면 bash 환경이 필요하기 때문에 WSL이 아니면 **윈도우에서는 쓸 수 없음**.

### JSON 도구

- [⭐JSON Placeholder](https://jsonplaceholder.typicode.com/): JSON 응답을 받아야하는데 백엔드를 만들기 귀찮으면 쓰는 Free Fake JSON API 서버
- [jsoneditoronline.org](https://www.jsoneditoronline.org): JSON 편집 및 뷰어
- [jsonviewer.codeplex.com](https://jsonviewer.codeplex.com): JSON 뷰어

### 빌드 도구

- [Bazel](https://github.com/bazelbuild/bazel/): 멀티 언어 빌드 도구. Maven, Gradle, Webpack 등과 달리 다양한 언어(Java, C++, Python 등)를 하나의 시스템에서 통합 빌드할 수 있다는 점이 특징이다.


## 4. 프레임워크, 라이브러리

### 프론트엔드 프레임워크

- [Vue](https://vuejs.org/): 프론트엔드 라이브러리 #1.
- [⭐React](https://react.dev/): 프론트엔드 라이브러리 #2.
- [React Native](https://reactnative.dev/): React 기반의 크로스 플랫폼 개발 프레임워크. JavaScript 코드 하나로 안드로이드와 iOS에서 작동하는 앱을 빌드할 수 있다.
- [Svelte](https://svelte.dev/): 프론트엔드 프레임워크. 가볍고 단순한 게 특징. 일반적인 프레임워크와 다르게 런타임 대신 컴파일러로 작동한다. 이 말은 빌드 후에 별도로 불러오는 라이브러리가 없어 성능과 로딩 속도에서 이점이 있다는 뜻이다.
- [Flutter](https://flutter.dev/): 구글이 만든 UI 툴킷(SDK) 겸 크로스 플랫폼 개발 프레임워크. 지원되는 플랫폼은 Windows, macOS, 웹이다. 사용언어는 Dart
- [Vite](https://vitejs.dev/): 모던 프론트엔드 프로젝트를 위한 빌드 도구. React, Vue, Svelte와 순수 JavaScript를 모두 지원한다. 빠른 콜드 스타트, 핫 모듈 교체, 빌드 최적화, 플러그인 시스템 등이 특징이다.
- [Gatsby](https://www.gatsbyjs.com/): React 기반의 정적 사이트 생성(SSG, Static Site Generation) 프레임워크. 오픈 소스다.
- [⭐Next.js](https://nextjs.org/): React 기반 풀스택 프레임워크. SSR, SSG, CSR을 모두 지원한다. 주로 프론트엔드 개발에 사용된다.
- [React Router](https://reactrouter.com/): React 애플리케이션에서 클라이언트 사이드 라우팅을 구현하기 위해 사용되는 라이브러리. 7.x 버전부터는 Remix 3와 통합된 프레임워크 모드를 지원한다.
- [⭐Remix](https://remix.run/): React 기반 풀스택 웹 프레임워크. 가볍고 배우기 쉽다. Remix는 표준 웹 API를 활용해 만들어졌기 때문에 브라우저, Node.js, 서버리스 플랫폼, 클라우드 엣지 등의 다양한 환경에 동일한 코드 베이스로 배포할 수 있다(반면 어떤 프레임워크들은 Node.js 환경에서만 작동한다). 개발 측면에서 Next.js와 비교해보면, 서버 사이드 렌더링에서 더 세밀한 제어가 필요한 상황이나 복잡한 중첩 라우팅 구조에서는 Remix가 더 유리하지만, 서드파티 라이브러리나 고급 API 지원, 커뮤니티 등이 부족한 단점이 있다. 최신 버전인 Remix 3은 [React Router 7의 프레임워크 모드로 통합](https://remix.run/blog/incremental-path-to-react-19)되었으니, 도움말이 필요하면 [React Router의 문서](https://reactrouter.com/start/framework/installation)를 볼 것.
- [Quasar Framework](https://quasar.dev/): Vue 전용인듯?
- [Vuetify](https://vuetifyjs.com/en/): Vue용 컴포넌트 프레임워크

### 백엔드 프레임워크

- [⭐Netty](https://netty.io/): Java 네트워크 앱 개발용 NIO(비동기 입출력) 클라이언트-서버 프레임워크
- [Spring](https://spring.io/): Java 백엔드의 대명사 격인 Java 서버 애플리케이션 개발 프레임워크
- [Spring Modulith](https://github.com/spring-projects/spring-modulith): 하나의 앱 안에 여러 개의 모듈을 구성하고, 모듈 간 명확한 경계를 유지하도록 도와주는 프레임워크. 잘 구조화된 모놀리틱 시스템을 만들고 싶거나, 나중에 MSA로 전환할 계획일 경우 사용하면 된다.
- [NestJS](https://nestjs.com/): TypeScript 기반의 백엔드 애플리케이션(= API 서버) 구축을 위한 Node.js 프레임워크. 의존성 관리, 모듈화, 서버 사이드 렌더링, 웹소켓 등을 지원한다.
- [Tiles](https://tiles.apache.org/): Java에서 사용하는 템플릿 프레임워크. 지금은 retired 상태라서 업데이트는 없다.
- [SiteMesh](https://struts.apache.org/plugins/sitemesh/): 타일즈와 같은 JSP 템플릿 프레임워크. 오래되긴 마찬가지긴 하지만 가장 최근에 썼었...던가?

### CSS 프레임워크, UI 컴포넌트 라이브러리

- [Bootstrap](https://getbootstrap.com/): 범용 프레임워크로 분류됨
- [Bulma](https://bulma.io/): Flexbox 기반
- [Foundation Framework](https://get.foundation/)
- [⭐Tailwind CSS](https://tailwindcss.com/): 미리 작성된 스타일링 클래스를 제공하는 방식. 유틸리티 기반 프레임워크로 분류됨
- [daisyUI](https://daisyui.com/): Tailwind CSS의 플러그인으로, 자주 쓰는 클래스를 조합해 만든 컴포넌트 세트를 제공한다. 테일윈드조차 귀찮게 느껴질 때 유용하지만, 복잡한 커스터마이징이 필요한 경우엔 다소 불편할 수 있다.
- [StyleX](https://stylexjs.com/): 메타(페북)에서 만듦. 테일윈드 경쟁자라 함. JavaScript 객체 기반으로 사용한다. 조건부 스타일 설정, 컴파일, 타입 안정성(type safe)이 특징이다. React 없이도 쓸 수 있는 것으로 보임.
- [Open Props](https://open-props.style/)
- [Basscss](https://basscss.com/)
- [Water.css](https://watercss.kognise.dev/)
- [MVP.css](https://andybrewer.github.io/mvp/)
- [Materialize CSS](https://materializecss.com/)
- [System.css](https://sakofchit.github.io/system.css/): 레트로 갬성. 애플 시스템 OS와 똑같다고 한다.
- [NES.css](https://nostalgic-css.github.io/NES.css/): 닌텐도 스타일
- [PSone.css](https://micah5.github.io/PSone.css/): 플스 1 스타일
- [98.css](https://jdan.github.io/98.css/): 이건 윈도우 98
- [XP.css](https://nostalgic-css.github.io/NES.css/): 윈도우 XP
- [7.css](https://khang-nd.github.io/7.css/): 이건 윈도우 7
- [Bojier](https://bojler.slicejack.com/): 메일용
- [Chota](https://jenil.github.io/chota/)
- [Pico.css](https://picocss.com/)
- [ChakraUI](https://chakra-ui.com/): React 애플리케이션을 위한 간결한 모듈식
- [Mantine](https://mantine.dev/): React용 UI 컴포넌트 라이브러리
- [Ant Design](https://ant.design/): React용 UI 컴포넌트 라이브러리
- [NextUI](https://nextui.org/): React용 UI 컴포넌트 라이브러리
- [Reach UI](https://reach.tech/): React와 Vue용 UI 컴포넌트
- [Headless UI](https://headlessui.dev/): React와 Vue용 UI 컴포넌트
- [⭐shadcn/ui](https://ui.shadcn.com/): React용 UI 컴포넌트 컬렉션. 소스 코드를 복사해 커스터마이징하는 방식으로, 전통적인 라이브러리와 차이가 있다. Tailwindd CSS와 [Radix UI](https://www.radix-ui.com/) 기반으로 만들어졌다. 사실 CSS 프레임워크라기보단 UI 컴포넌트 라이브러리에 가깝다.
- [Storybook](https://storybook.js.org/): 프론트엔드 워크샵이라고 한다(그게뭐야). UI 컴포넌트나 페이지를 만들 때 쓴다는데 아직 몲. 근데 가급적 빨리 써보는 게 좋을 것 같은 느낌적인 느낌
- [component.gallery](https://component.gallery/): CSS 프레임워크와 디자인 시스템 같은 것을 모아놓은 사이트
- [Material UI](https://mui.com/material-ui/): React용 UI 컴포넌트 모음
- [@mdi/svg](https://www.npmjs.com/package/@mdi/svg): MDI(Material Design Icons) 프로젝트의 SVG 아이콘을 제공하는 라이브러리. 단독 기능은 필요한 아이콘을 svg로 만들어주는 게 있다. (아마도?)
- [Vitebook](https://vitebook.dev/)

### JavaScript 라이브러리

- [Moment.js](https://momentjs.com/): 날짜와 시간을 다루는 라이브러리 #1. 포매터가 필요하면 이걸 쓰자
- [⭐Day.js](https://day.js.org/): 날짜와 시간을 다루는 라이브러리 #2. 포매터 지원함. MUI의 Date picker에 적용된 라이브러리다.
- [Lodash](https://lodash.com/): 모듈 시스템을 지원하는 유틸리티 라이브러리. 성능 향상을 위해 쓰인다. 대표적으로 [lodash.debounce](https://lodash.com/docs/4.17.15#debounce)가 있음.
- [babel](https://github.com/babel/babel): 일종의 컴파일러(?). 작성한 JavaScript 코드의 최신 문법과 기능을 구버전의 브라우저나 환경에서 실행될 수 있도록 변환해 준다.
- [⭐Immer](https://immerjs.github.io/immer/): 불변성을 유지하면서 객체를 쉽게 수정할 수 있게 해주는 라이브러리. React에서 상태값이 객체일 때 활용하기 좋다. 제공된 draft를 직접 변경하는 방식으로 사용하는데, 변경된 부분만 복제하도록 최적화되어 있기 때문에 객체 전체의 깊은 복제용으로는 적합하지 않다.
- [sharp](https://github.com/lovell/sharp): 고성능의 이미지 처리 Node.js 패키지. 이미지 최적화에 주로 사용된다. 제공되는 기능은 이미지 크기 조정, 포맷 변환, 자르기, 회전, 반전, 필터 적용 등.
- [Execa](https://github.com/sindresorhus/execa): Node.js 환경에서 외부 명령어를 실행할 수 있게 도와주는 프로세스 실행 패키지. `child_process` 모듈의 개선된 대안으로, 더 나은 API를 제공하며 명령어 실행 결과를 쉽게 다루고, 비동기 작업과 오류 처리 등을 효율적으로 처리할 수 있다.
- [Puppeteer](https://github.com/puppeteer/puppeteer): Google에서 개발한 헤드리스 브라우저 제어 Node.js 패키지, Chrome 또는 Chromium 브라우저를 프로그래밍 방식으로 제어할 수 있게 해준다. 브라우저를 자동화하거나 웹 스크래핑, UI 테스트, PDF 생성, 스크린샷 찍기, 성능 측정 등의 작업을 수행할 수 있다. 기본적으로 헤드리스 모드(브라우저 UI 없이 실행)로 작동하지만, 헤드 모드로 실제 브라우저 창을 띄워 작업을 실행할 수도 있다.
- [fullPage.js](https://github.com/alvarotrigo/fullPage.js): 어떤 게임의 이벤트 페이지에서 줏어온 거. 전체 화면 스크롤 웹 사이트(단일 페이지 웹 사이트 또는 단일 페이지 사이트라고도 함)를 만들고 사이트 섹션 내에 가로 방향 슬라이더를 추가하는 간단하고 사용하기 쉽다...는데 사실 뭔지 잘 몲.
- [Dexie.js](https://dexie.org/): 브라우저의 IndexedDB를 다루는 서드 파티 중 가장 인기 있는 라이브러리. Promise 기반 API를 제공한다.
- [pug](https://pugjs.org/api/getting-started.html): Node.js 기반 템플릿 엔진. HTML을 서버 사이드 렌더링할 때 사용한다. 예전 이름은 Jade
- [json-server](https://github.com/typicode/json-server): JSON 파일만으로 API 서버를 제공하고 싶을 때 사용하는 fake API server 라이브러리.
- [⭐live-server](https://github.com/tapio/live-server): 로컬 전용 웹 서버가 필요할 때 설치하는 패키지. 페이지 새로고침을 자동으로 해주는 live reload 기능을 제공하며 index 파일이 필요 없어서 편하다.
- [⭐serve](https://github.com/vercel/serve): 빠르고 가벼운 정적 파일 제공용 웹 서버. Live reload 기능은 없으며, 프로덕션 환경을 미리 보고 싶을 때 사용한다.

### React 리액트 전용 라이브러리

- [Million](https://million.dev/): React를 빠르게 만들어준다고 함. (무려 70%)
- [Redux](https://redux.js.org/): 리덕스. 상태(reactive state) 관리 라이브러리 #1. 모든 상태 변화가 중앙에서 관리되기 때문에 예측과 디버깅 등 유지보수에 도움이 된다고 한다. React에서는 복잡한 계층 구조를 가진 컴포넌트들 사이에서 state의 변화를 전달할 때 *props drilling*이라 불리는 번거로운 작업이 필요한데, Redux는 이러한 작업을 간소화할 수 있는 라이브러리다. 그런데 문법이 너무 복잡한 탓인지 인기가 점점 시들해진다고...
- [⭐zustand](https://github.com/pmndrs/zustand): 상태 관리 라이브러리 #2. 리덕스와 마찬가지로 props drilling 문제를 방지하고 상태를 전역으로 관리할 수 있게 해준다. 리덕스보다 코드 작성이 간결하다.
- [recoiljs](https://recoiljs.org/): 상태 관리 라이브러리 #3
- [Jotai](https://jotai.org/):  상태 관리 라이브러리 #4
- [React Hook Form](https://react-hook-form.com/): 입력 폼을 쉽게 다루게 해주는 라이브러리. 쓰기 좀 복잡하긴 한데 챗피티가 자꾸 좋다고 들이댐.
- [useHooks](https://github.com/uidotdev/usehooks): 훅 모음 #1. React 개발에서 자주 반복되는 기본적인 로직(클립보드 복사, 디바운스, 로컬 스토리지 관리 등)을 커스텀 훅 형태로 모아놓은 라이브러리다. 아래의 react-use에 비해 비교적 간단하고 필수적인 훅 위주로 모아져있다. [usehooks.com](https://usehooks.com/)는 원래 React의 커스텀 훅을 직접 만들어보며 학습할 수 있도록 예시와 개념을 설명하는 튜토리얼 사이트였음.
- [react-use](https://github.com/streamich/react-use): 훅 모음 #2. 이 패키지도 React 개발에서 자주 반복되는 로직(브라우저 API 연동, 상태 관리, UI 상호작용 등)을 훅 형태로 모아놓은 라이브러리다. useHooks 보다 방대하고 폭 넓은 훅들을 제공한다.
- [TanStack Query](https://tanstack.com/query/latest/docs/framework/react/overview): 데이터 페칭, 캐싱, 상태 업데이트를 간단하고 효율적으로 관리할 수 있게 해주는 라이브러리다. 네트워크 요청과 그 결과를 체계적으로 관리해 주는 '데이터 상태 관리' 솔루션이라 설명하기도 함. 예전 이름은 React Query였음.
- [⭐SWR](https://swr.vercel.app/): 데이터 페칭과 캐싱을 간편히 처리해주는 라이브러리. Vercel에 만듦.
- [Legend-State](https://github.com/LegendApp/legend-state): Fine-grained reactivity가 특징인 프록시 기반의 React 상태 관리 라이브러리. 상태의 특정 부분이 변경될 때 해당 부분을 사용하는 컴포넌트만 리렌더링해서 성능이 좋다. Fine-grained reactivity에 대해선 [이 글](https://yozm.wishket.com/magazine/detail/3294/?data=8YxZrHl21HwdZpRI0O8P3A00cy25KUkklgTYMHWWUWg%3D) 참고.

### Vue 뷰 전용 라이브러리

- [Pinia](https://pinia.vuejs.org/): Vue에서 공식으로 지원하는 전역 상태 관리 라이브러리다.

### 번들러

- [webpack](https://webpack.js.org/)
- [Browserify](https://browserify.org/)
- [rollup.js]()https://rollupjs.org/
- [Parcel](https://parceljs.org/)

### UI 컴포넌트 라이브러리

- [Air Datepicker](https://air-datepicker.com/): 프론트엔드용 달력 컴포넌트. 순수 JavaScript 기반이며, 언어 기본값이 러시아어인 걸 보니 러시아산인 모양
- [Tom Select](https://tom-select.js.org/): 셀렉트박스. 순수 JavaScript 기반.
- [AG Grid](https://www.ag-grid.com): 주변 사람이 추천한 💰유료 그리드
- [jqxgrid](https://www.jqwidgets.com/jquery-widgets-demo/demos/jqxgrid/index.htm)
- [Toast UI Grid](https://ui.toast.com/tui-grid): 줄여서 TUI Grid. NHN에서 만들었고 MIT 라이선스의 오픈 소스 그리드. 가볍게 쓰기 좋지만 깃허브 대응이 좀 많이 느리다.
- [Fontello](https://fontello.com/): 아이콘을 폰트로 구현할 때 씀. 사이트에서 선택한 아이콘만 다운로드할 수 있음.
- [Reveal.js](https://revealjs.com/): HTML로 만드는 PPT

### Java 라이브러리

- [⭐JSpecify](https://github.com/jspecify/jspecify): IDE나 프레임워크마다 제각각 작동하던 `null` 관련 어노테이션들을 표준화한 명세이자 라이브러리. 메서드 파라미터나 반환값의 `null` 허용 여부를 `@Nullable`, `@NullMarked`, `@NullUnmarked`, `@NonNull` 어노테이션으로 명시해서, 정적 분석 도구가 NPE 위험을 사전에 감지할 수 있도록 한다(런타임에는 아무런 일도 하지 않음). 메이븐 등으로 별도 설치 가능하며 Spring Framework 7에는 아예 내장되어 있음. 나중에는 Java에 통합될 가능성도 있다 함.


## 5. 인프라, 배포

### PaaS (Platform as a Service)

PaaS 중에 유명한 것들은 대체로 웹 앱 소스를 올리면 대신 빌드와 배포를 해주는 클라우드 서비스들이다.

- [Heroku](https://www.heroku.com/): 애플리케이션 개발과 배포를 위한 PaaS 서비스. 초반에 있었다는 일부 무료 정책(앱 5개까지 무료)은 없어진 모양이다. 다음 목록은 추천 받은 대체제들: [Koyeb](https://www.koyeb.com/), [Fly.io](https://fly.io/), [Northflank](https://northflank.com/)
- [Kubernetes](https://kubernetes.io/): 통칭 K8s, 컨테이너화된 애플리케이션을 관리하고 배포하기 위한 오픈 소스 플랫폼이다. 기존 PaaS 범주와 비슷하지만 다르다고 한다. [공식 문서 링크](https://kubernetes.io/docs/concepts/overviw/). "컨테이너화된 애플리케이션을 배포, 관리, 확장할 때 수반되는 다수의 수동 프로세스를 자동화하는 오픈 소스 컨테이너 오케스트레이션 플랫폼"이라 소개된다.
- [cloudtype](https://cloudtype.io/): 최스님이 알려줌. 안써봄
- [smolsite](https://smolsite.zip): 스몰사이트, ZIP으로 압축해서 업로드하면 무료로 호스팅 해줌
- [Vercel](https://vercel.com/): 프론트엔드용 클라우드 플랫폼. Next.js의 개발사이기도 하다. 정적 사이트와 Jamstack 이키텍처에 최적화되어 있다고 한다.
- [⭐Netlify](https://www.netlify.com/): 정적 웹사이트 및 프론트엔드 애플리케이션을 빠르고 쉽게 배포할 수 있는 클라우드 기반 플랫폼. 웹 앱 배포 및 관리를 위한 다양한 기능을 제공한다. 주요 특징으로 자동 빌드 및 배포, 서버리스, 글로벌 CDN, 커스텀 도메인, 무료 SSL 인증서 등이 있다. 뭔지 모르겠지만 JAMstack(?) 아키텍처와 잘 맞는다고 함. [무료 플랜](https://www.netlify.com/pricing/)에선 월 100GB의 트래픽 제한이 있다.

### IaaS (Infrastructure as a Service)

인프라만 제공하는 클라우드 서비스.

- [AWS](https://aws.amazon.com/): 아마존의 the AWS. 제한적인 무료 티어를 제공함
- [Google Cloud Platform (GCP)](https://cloud.google.com/?hl=ko)
- [Azure](https://azure.microsoft.com/)
- [Oracle Cloud Infrastructure (OCI)](https://www.oracle.com/kr/cloud/): 오라클 OCI. 얘네도 무료 티어가 있음
- [Pulumi](https://www.pulumi.com/}): JavaScript 코드로 클라우드 인프라를 관리하는 IaC 서비스. 개인 용도는 무료(2023-09-13)

ℹ️ IaC(Infrastructure as Code): 코드를 사용하여 수동 프로세스가 아닌 자동으로 인프라를 정의하고 관리하는 방법론의 일종

### BaaS (Backend as a Service)

BaaS란 백엔드의 전반적인 기능을 제공하는 서비스를 의미함.

- [Firebase](https://firebase.google.com/): 구글의 BaaS 플랫폼.
- [Supabase](https://supabase.com/): PostgreSQL 기반의 백엔드 기능을 제공하는 BaaS 플랫폼. 인증, 데이터베이스, 파일 저장소, 서버리스 기능 등을 제공한다.
- [PocketBase](https://pocketbase.io/): 오픈 소스 백엔드 서비스 #2. 실시간 데이터베이스, 인증, 파일 저장소, 어드민 대시보드를 파일 하나로 만들어준다. 다른 서비스와 다르게 클라우드가 아니라 서버를 직접 구축하는 방식이다.

### Web Server/WAS

- [Apache HTTP Server](https://www.apachelounge.com/download): the 웹 서버. 검색으론 다운로드 링크 찾기 힘듬.
- [nginx](https://nginx.org/): the 웹 서버 #2
- [copyparty](https://github.com/9001/copyparty): Python만 있으면 실행 가능한 파일 서버. 의존하는 라이브러리는 하나도 없으면서, HTTP, 웹 브라우저 접속, WebDAV, FTP, TFTP, SMB/CIFS, zeroconf, 미디어 인덱싱, 검색, 썸네일 생성, 압축 다운로드까지 뭐 안되는 게 없다. 업/다운로드는 조각별 해시와 병렬 연결 기반으로 작동한다.

### CDN

- [cdnjs](https://cdnjs.com): Cloudflare에 의해 호스팅되는 무료 오픈 소스 CDN
- [JSDELIVR](https://www.jsdelivr.com): 역시 무료 CDN. npm과 GitHub에 최적화되어 있다고 한다.
- [UNPKG](https://unpkg.com/): npm으로 배포된 패키지를 불러올 수 있는 CDN. `unpkg.com/react@16.7.0/umd/react.production.min.js` 요런식으로 씀

### 호스팅, 터널링

- [ngrok](https://ngrok.com/): 로컬 구동 서버(localhost)를 외부에서 접속할 수 있게 해주는 로컬 터널링 도구. HTTPS 프로토콜도 지원한다. 윈도우에선 chocolatey로 설치하고, 별도로 발급 받은 토큰을 `ngrok config add-authtoken MY_TOKEN` 명령으로 등록한 뒤 `ngrok http http://localhost:8080` 명령으로 로컬 서버와 연결하는 식으로 구동한다. expo는 터널링 옵션을 위해 ngrok을 내장하고 있기도 하다.

### 프로세스 매니저

- [PM2](https://pm2.io/): Node.js 앱을 운영하는데 자주 쓰이는 프로세스 매니저. 자동 재시작, 로드 밸런싱, 모니터링, 로그 관리, 무중다 배포(Zero downtime deployment) 등의 기능을 제공한다.

### 메시지 브로커(메시지 큐 시스템)

- [Apache Kafka](https://kafka.apache.org/): 카프카. 아파치 소프트웨어 재단이 스칼라로 개발한 오픈 소스 분산 스트리밍 플랫폼. '메시지 브로커 프로젝트' 혹은 '분산 환경에서 사용되는 데이터 스트리밍 플랫폼'으로 소개된다. 여러 데이터베이스의 동기화 작업이나, 비동기로 메시지를 주고 받을 때 사용한다(데이터 입출력도 여기에 해당함). 에이전트를 설치하는 방식이라 카더라.
- [RabbitMQ](https://www.rabbitmq.com/): MQTT, AMQP를 구현한 오픈 소스 메시지 브로커. Erlang으로 만들어졌다(얼랭?). 사용자 요청의 비동기 처리, 앱 간 메시지 교환, 분산 시스템에서의 메시지 브로커링 등에 활용된다.


## 6. 데이터베이스

### DBMS

- [⭐SQLite](https://www.sqlite.org/index.html): 오픈 소스 RDBMS #1. 매우 가벼워서 보통 서버가 아닌 소프트웨어에 내장시키는 임베디드용으로 쓰인다. ACID(Atomicity, Consistency, Isolation, Durability) 특성을 준수하는 기능과 트랜잭션을 지원한다. 시스템이 매우 안정적이라 프로시저 같은 추가 기능이 필요한 게 아니라면 서버용으로 써도 된다고 하는 사람도 있다. 에스큐엘라이트 혹은 시퀄라이트라고 읽는다. 경량 DB라서 SQL 표준 기능이 일부 빠져있거나 제한적으로 제공한다. 예를 들어 외래키는 기본적으로 비활성화되어 있고 사용 제한이 있다. 그리고 프로시저는 미지원, 트리거와 윈도우 함수 등은 부분적으로 지원한다. 동시성 제어와 격리 수준도 다른 DBMS만큼 세밀하지 않으며 여러 사용자의 동시 쓰기 시 충돌 가능성이 있다. 따라서 SQLite를 서버에서 사용한다면 읽기 전용으로만 설계하고, 쓰기는 다른 수단을 마련할 것. 그리고 기본적으로 체크섬을 수행하지 않기 때문에, 디스크 손상으로 인한 데이터 무결성을 보장하지 않는다는 점은 주의해야 한다.
- [⭐PostgreSQL](https://www.postgresql.org/): 오픈 소스 RDBMS #2. SQL 표준을 높은 수준으로 준수하며 ACID와 트랜잭션을 지원한다. JSON이나 XML 같은 데이터 타입을 그대로 저장할 수 있고, 플러그인과 확장 모듈을 통해 사용자 정의 함수나, 데이터 타입, 인덱스 방식 등을 추가할 수 있다는 점이 기존 RDBMS들과 다르다. 예를 들어 [PGMQ](https://github.com/tembo-io/pgmq) 확장을 설치하면 데이터베이스와 메시지 큐를 복잡한 구성 없이 단일 트랜잭션으로 묶을 수 있다.
- [⭐Redis](https://redis.io/): 인메모리 데이터베이스 중 가장 인지도 높음. 메모리에 데이터를 저장해서 속도가 빠른게 특징이며 디스크 백업 기능도 제공함.
- [SwayDB](https://swaydb.io/?language=java): 레디스같은 인메모리 데이터베이스. 레디스처럼 서버용은 아니고 임베디드로 쓰이는 모양?

### DBMS Tool

- [DBeaver](https://dbeaver.io): 벤더 안가리는 도구. 이클립스와 같은 프레임워크로 추정.
- [QueryBox](http://www.querybox.com): 벤더 가리지 않고 접속할 수 있는 국산 앱. 💰기업용은 유료.
- [Flyway](https://flywaydb.org/): 데이터베이스의 버전 관리 도구. 마구잡이로 헤집어놓은 데이터도 원하는 시점으로 되돌리는 기능 등을 제공한다. 개발과 테스트를 위한 데이터베이스 구축에 매우 쓸만한 오픈 소스 도구.
- [⭐DataGrip](https://www.jetbrains.com/datagrip/): IntelliJ IDEA Ultimate 패키지에 포함된 제품. 성능은 돈 값 한다.

### ERD

- [dbdiagram.io](https://dbdiagram.io/): 데이터베이스 ERD 전용. 웹 버전만 있긴 하지만 쓸만함. 별도의 스키마 문법을 사용하는 건 안좋음.
- [⭐erd-editor](https://github.com/dineug/erd-editor): VSCode에서 사용하기 좋은 무료 ERD 에디터. IntelliJ와 웹 버전도 지원한다.


## 7. 테스트, 품질 관리

### 유닛 테스트 프레임워크

- [JUnit](https://junit.org): Java 테스팅 프레임워크. JUnit Platform + JUnit Jupiter + JUnit Vintage 세 개를 합친 JUnit 5 버전이 나왔음. (2023-02-15)
- [AssertJ Core](https://assertj.github.io/doc/): 테스트용 Java 라이브러리. Spring boot starter 라이브러리에 포함돼 있다. (사실 JUnit도 같이 있음) 들리는 말로는 요게 더 좋다고 함.
- [Mocha](https://mochajs.org/): 오래된 JavaScript 테스팅 프레임워크 #1. 이하 모카
- [Chai](https://www.chaijs.com/): JavaScript용 Assertion 라이브러리. 가독성 좋고 유연한 문법을 제공한다. 보통 모카와 같이 쓴다.
- [Jest](https://jestjs.io/): React 프로젝트에서 많이 사용하는 테스팅 프레임워크. Facebook에서 개발했다. 지금은 OpenJS 재단에서 관리하는 모양.
- [⭐Vitest](https://vitest.dev/): Vite 생태계의 테스팅 프레임워크. Vite와의 쉬운 통합, 빠른 실행 속도가 특징. ESM을 지원한다.

### 테스트 도구

- [postman](https://www.getpostman.com): HTTP Request/Response 테스트
- [Selenium](https://www.selenium.dev/): 동적 웹 앱(페이지 로딩 후 JavaScript 등에 의해 동적으로 구성이 변경되는 사이트) 테스트 혹은 크롤링에 사용되는 도구다.


## 8. 인프라 모니터링, 성능 분석

### APM(Application Performance Management), 프로파일링

통칭 모니터링 도구 모음. 얘네들은 웬만하면 상용이다.

- [Datadog](https://www.datadoghq.com): 데이터독. 인프라 모니터링. APM 기능도 있지만 시스템 성능 지표 분석 기능이 주력이다. 설치형이 아니라 데이터는 저쪽에서 관리하며, 비싸다.
- [제니퍼](https://jennifersoft.com/ko/product/java): Java 앱 모니터링
- [VisualVM](https://visualvm.github.io): 프로파일링 도구#1. Java 앱용. VM의 환경, CPU와 메모리의 사용량, 클래스와 스레드의 점유율, CPU/메모리/JDBC 프로파일링 등의 기능을 제공한다. 오픈 소스
- [Eclipse Memory Analyzer](https://www.eclipse.org/mat/downloads.php): 프로파일링 도구 #2. Java 앱용. VisualVM보다 기능이 조금 더 많고 도움말이 잘 되어 있어서 쓰기 편함. UI는 이클립스 기반임. 오픈 소스
- [XRebel](https://www.jrebel.com/products/xrebel): 프로파일링 도구 #3. Java 웹 앱 전용이다. 상용
- [JProfiler](https://www.ej-technologies.com/products/jprofiler/overview.html): 프로파일링 도구 #4. 10일 무료. IDE와 연동할 수 있음. 상용
- [YourKit Java Profiler](https://www.yourkit.com/java/profiler/features): 프로파일링 도구#5. 이것도 IDE 연동 쌉가능. 단, Java 1.7 미만의 환경은 지원하지 않으며 상용이다.
- [Jennifer](https://jennifersoft.com/ko/product/java): APM #2. Java/PHP/닷넷 앱 모니터링. 상용
- [WhaTap](https://www.whatap.io/ko): APM #3. 앱/서버/DB/URL/컨테이너/인프라 모니터링. 상용이며 한국기업이라 한국어판을 제공한다.
- [⭐Pinpoint](https://pinpoint-apm.gitbook.io/pinpoint/): 오픈 소스 APM. 네이버에서 만들었다 함
- [Scouter](https://github.com/scouter-project/scouter): 오픈 소스 APM. LG CNS랑 관련이 있나 봄. 이거 만든 사람들이 WhaTap 만들었다고 하던디...?

### 데이터 시각화

- [Grafana](https://grafana.com/): 메트릭/로그 시각화 도구. 오픈 소스다. 메트릭이란 주기적으로 발생하는 타임스탬프를 포함한 수치 데이터라고 한다.
- [Kibana](https://www.elastic.co/kr/kibana/): Elastic Stack의 일부인 데이터 시각화 및 분석 도구. 모니터링과 APM 기능도 제공됨.


## 9. 보안, 인증

- [Let's Encrypt](https://letsencrypt.org/): 비영리 단체인 [ISRG, Internet Security Research Group](https://www.abetterinternet.org/)에서 제공하는 TLS(SSL) 인증서 무료 발급 사이트. 
- [certbot](https://certbot.eff.org/): *Let's Encrypt*에서 발급하는 TLS 인증서를 쉽게 관리하게 도와주는 오픈 소스 소프트웨어. [EFF(Electronic Frontier Foundation)](https://www.eff.org/)에서 만들었고 인증서 자동 갱신 같은 작업을 지원한다.
- [HashiCorp Vault](https://www.vaultproject.io/): 비밀번호 관리, 암호화 키 관리, 접근 제어 등을 통합 제공하는 오픈 소스. 암호화 키를 관리하는 용도로 쓴다. 클라우드, 온프레미스 모두 지원한다. 엔터프라이즈 버전은 전용 서버가 제공되지만 무료 버전은 직접 설치해야 한다.


## 10. 프로그래밍 언어 Programming Language

- [⭐Rust](https://www.rust-lang.org/): 가비지 컬렉션(GC)을 사용하지 않으며 메모리 안전성(memory safe)을 추구하는 언어 #1. 마찬가지로 GC가 없지만 memory unsafe한 C와 C++의 대체제로 꼽힌다. [Mozilla Research](https://research.mozilla.org/)에서 개발했다. 성능이 중요한 네이티브 앱, 웹 서버, CLI 도구, 임베디드 시스템이나 OS 수준의 코드를 개발할 때 적합하다.
- [Elixir](https://elixir-lang.org/): BEAM(Erlang의 가상머신) 위에서 실행되는 함수형 프로그래밍 언어. 동시성과 분산 처리에 강하며, 프로세스 간의 격리(한 프로세스의 실패가 시스템 전체에 영향을 주지 않음)를 통한 안정성이 특징이다. Erlang이 Java라면 Elixir는 Kotlin에 비견된다. 디스코드는 실시간 메시징 처리를 Elixir로 구현했다고 함. 비결은 Erlang VM이 관리하는 Erlang 프로세스(OS의 프로세스나 스레드와 다른 개념)와 concurrency 지원 기능 덕분이라나...
- [Erlang](https://www.erlang.org/): 1980년대에 통신 시스템 구축을 위해 만들어진 언어(그래서 Erlang/OTP, Open Telecom Platform이라 함). 짧은 지연시간, 견고함, 내결함성, 분산 시스템 지원 등이 특징이다. Java와 유사하게 다른 언어가 컴파일하는 가상머신(Erlang의 VM, BEAM이라 부른다)에서 작동한다. 
- [Ada](https://ada-lang.io/): GC를 사용하지 않으며 메모리 안전성을 추구하는 언어 #2. 항공, 방위, 우주 산업처럼 높은 수준의 안전성과 정확성이 요구되는 시스템에서 쓰인다고 한다.
- [Zig](https://ziglang.org/): GC를 사용하지 않으며 메모리 안전성을 추구하는 언어 #3. 셋 중 가장 최신 언어다(Ada의 표준화는 1983년, Rust의 최초 릴리즈가 2010년, Zig는 2015년에 개발 시작).
- [Scala](https://www.scala-lang.org/): 객체 지향 프로그래밍과 함수형 프로그래밍을 결합한 개발 언어. 왜인지 모르겠지만 개발자 설문조사 언어별 평균연봉 항목에서 늘 상위권을 차지한다. Java 기반이며 JVM에서 실행된다.
- [Kotlin](https://kotlinlang.org/): JVM에서 실행되는 Java와 상호 운용 가능한 언어. 안드로이드 개발에 주로 쓰인다. 네이티브, JavaScript(?)까지 지원한다고 하며 기존의 Java 코드를 호환해줘서 그대로 사용할 수 있다고 한다.
- [Apache Groovy](https://groovy-lang.org/): JVM에서 작동하는 동적 타입 프로그래밍 언어(? 그게 뭔데). Java, Python, Ruby 등의 언어에서 영향을 받았다 한다. 
- [⭐Go](https://golang.org): 한 때 세계에서 가장 돈을 많이 버는 프로그래밍 언어로 집계되기도 했으며 개발 속도와 실행 속도 둘 다 빠른 획기적인 언어라고 함. 언어 자체가 경량이라 늘 쓰던것만 쓴다는 소소한 단점이 있지만, 그만큼 빠르고 쉽게 익힐 수 있다. 간단한 API 서버나 백엔드 애플리케이션 개발에 적합하다.
- [Dart](https://dart.dev/): 구글이 멀티 플랫폼 작동을 목적으로 만든 언어. 문법은 C와 비슷. Java처럼 DVM(Dart VM) 상에서 작동하거나 네이티브 컴파일을 따로 한다.
- [⭐TypeScript](https://www.typescriptlang.org/): JavaScript의 슈퍼셋(superset)으로, 이름처럼 정적 데이터 타입이 추가되어 컴파일 에러 검출이 가능한 게 대표적인 특징이다. 컴파일 타임이 존재하며 JavaScript 코드로 변환된다.
- [Mojo](https://www.modular.com/mojo): Python의 슈퍼셋. Python의 느린 속도를 개선했으며 저수준의 제어가 가능한 것이 특징. Python 3.x를 완벽하게 호환한다.
- [Jsonnet](https://jsonnet.org/): 환경 설정용 언어(A configuration language). 오직 JSON 데이터를 만들기 위한 언어로, 기존 JSON에선 불가능하던 변수 참조, 조건 분기, 함수, import 등의 기능을 사용할 수 있다.

### 언어 변환 및 분석

- [Figstack](https://www.figstack.com/): 코드를 다른 언어로 번역, 영어로 해설, documentation comments 만들기, 시간 복잡도 계산, 작성한 코드 기반 자연어로 질문까지. 아직은 쪼끔 느린게 흠.
- [OneLang.io](https://ide.onelang.io/): 개발 언어 병렬 번역기


## 11. 런타임 환경 Runtime Environments

- [Node.js](https://nodejs.org/): 서버 사이드 JavaScript 런타임
- [Deno](https://deno.land/): Node.js 개발자가 만든 JavaScript와 Rust기반의 JavaScript/TypeScript 용 런타임. Node.js 개선 버전이라고 보면 됨.
- [Bun](https://bun.sh/): Node.js보다 최적화된 성능, 더 나은 메모리 관리 등이 목표인 서버 사이드 JavaScript 런타임. 다른 런타임보다 빠르다고 주장한다.


## 12. 크로스 플랫폼 개발

- [Electron](https://electronjs.org/): 크로스 플랫폼 데스크탑 앱 개발 프레임워크. 오픈 소스고 JavaScript 기반이다. VSCode, Atom, Notion desktop 등이 이걸로 만들어짐. 
- [Tauri](https://tauri.app/): 크로스 플랫폼 앱 개발 프레임워크(안드로이드나 iOS도 되는 모양). 일렉트론의 경쟁 모델이다. OS의 네이티브 웹 런타임을 활용하며 백엔드는 Rust, 프론트엔드는 JavaScript 기반이다. React나 Vue 등의 대부분의 프론트엔드용 프레임워크를 지원한다. 일렉트론보다 가볍고 빠르며, 더 나은 보안을 장점으로 내세운다.


## 13. 디자인, 다이어그램

### 다이어그램, UML

- [⭐eraser](https://www.eraser.io/pricing): 마크다운 노트와 드로잉 앱을 합체시킨 신박한 물건. 엔지니어링 팀을 위한 화이트보드라 소개된다. 키보드로 플로우 차트 그리기 수월하다. Diagram as Code, 코멘트 기능 지원. 무료 플랜 제공
- [Excalidraw](https://excalidraw.com/): 웹 전용 드로잉 앱. 무료 사용 가능. eraser에 비해 가볍게 쓰기 좋다.
- [Balsamiq](https://balsamiq.com/wireframes/): UML, 와이어프레임 등을 위한 드로잉 앱. 좋지만 무료 플랜이 없는 게 단점
- [StarUML](http://staruml.sourceforge.net/ko)
- [Draw.io](http://www.draw.io)
- [Gliffy](http://www.gliffy.com)
- [⭐Mermaid](https://mermaid.js.org/): 간단한 텍스트 구문을 이용해 다이어그램을 생성해주는 JavaScript 기반 라이브러리. 이런걸 Diagram as Code라고 한다. 플로우 차트, 간트 차트, 클래스 다이어그램, 깃 그래프, 시퀀스 다이어그램, 클래스 다이어그램, ERD 등을 지원한다. 더 자세한 내용은 [여기](https://mermaid.js.org/intro/)서 확인.
- [chart.xkcd](https://github.com/timqian/chart.xkcd): JavaScript로 만드는 차트. 결과물은 svg로 나옴. 발로 그린 것 같은 모양새가 특징
- [⭐tree.nathanfriend.com](https://tree.nathanfriend.com/): 트리 구조의 텍스트 기반 다이어그램을 생성해주는 사이트. 입력한 값은 URL의 쿼리스트링에 포함되기 때문에 공유가 쉽다.

### 마인드맵

- [XMind](http://www.xmind.net)
- [FreeMind](http://freemind.sourceforge.net/wiki/index.php/Main_Page)

### 와이어프레임, 기획

- [MockFlow](https://www.mockflow.com/)
- [Moqups](https://moqups.com/)
- [Cacoo](https://nulab.com/cacoo/)
- [Wireframe.cc](https://wireframe.cc/)
- [⭐Axure](https://www.axure.com/): 기획안/와이어프레임 작성에 사용하는 앱. 무료 플랜은 없다. 가격은 월 25달러(2024-07-11)

### 디자인 리소스

- [Framer](https://www.framer.com/): 노코드 웹사이트 빌더. 코드 없이 웹사이트 만드는 게 주요 기능인데, 최소 기능 제품(MVP)이나 개념 증명(POC)에 활용하기 좋다. 제품의 마케팅이나 소개 페이지, 간단한 포트폴리오 등을 빠르게 만드는 데도 좋다.
- [Figma](https://www.figma.com): 요즘(2021-05-03) 뜬다는 UI 디자인 도구. 기본은 무료고, 💰비공개 프로젝트를 여러 명이 사용할 땐 유료
- [ThemeForest](https://themeforest.net/): 언어, 엔진, 프레임워크 별 테마(HTML과 CSS 묶음. 필요하면 JS까지) 파는 사이트
- [loading.io](https://loading.io/): 로딩 이미지, 패턴, 텍스트 등을 받을 수 있는 사이트. 무료버전인 경우 색 정도밖에 못바꿈.
- [미리캔버스](https://www.miricanvas.com/): 자칭 디자인 플랫폼. 템플릿 활용할 수 있는 간편한 디자인 도구를 제공하며 맞춤 디자인 의뢰도 가능함.
- [Fluent Icons](https://fluenticons.co/): 마소의 오픈 소스 아이콘 저장소. 마소가 만든건 아님. SVG 혹은 PNG로 받을 수 있다.
- [Awesome list of free CSS Generator - DEV Community](https://dev.to/jon_snow789/awesome-list-of-free-css-generator-293k): CSS 코드 생성기 모음


## 14. 네트워크, 보안

### SSH/텔넷/FTP 클라이언트

사실 윈도우 10부터는 Windows Terminal이 있어서 이런 거 필요 없지만... 🤭

- [WinSCP](https://winscp.net/eng/download.php)
- [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty)
- [Xshell](http://www.netsarang.co.kr/download/main.html): 가장 좋으나 💰유료 라이선스.
- [bitvise](https://www.bitvise.com/ssh-client-download): 서버 딱 하나에 붙는 용도로는 아주 좋다. 여러 서버에 붙으려면 매번 프로필들을 불러와야 해서 불편.
- [MobaXterm](https://mobaxterm.mobatek.net/download.html): 무료 앱 중에서 여러 서버 동시 접속 기능은 그나마...

### VPN

- [OpenVPN GUI](https://github.com/OpenVPN/openvpn-gui): OpenVPN 커뮤니티 버전의 클라이언트 앱. 공식 앱의 이름은 OpenVPN Connect이며 OpenVPN GUI와는 다르다. VPN 설정과 같은 네트웤에서도 앱에 따라 차이가 있을 수 있다. 예를 들어 OpenVPN Connect로 특정 VPN에 붙었더니 인터넷 먹통 현상이 발생했는데, OpenVPN GUI에선 같은 문제가 발생하지 않음.
- [Surfshark](https://surfshark.com): 접속 국가 변조용 💰유료 VPN

### 트래픽 캡처

- [Fiddler](http://www.telerik.com/fiddler): 네트워크 디버깅 도구. 파폭은 SSL 인증서 때문에 빡칠 수 있다. 💰유료
- [Wirehsark](https://www.wireshark.org/download.html): 패킷 추적 도구.

### IP 확인

- [https://icanhazip.com](https://icanhazip.com): 내 공인 IP만 텍스트로 응답하는 사이트.
- [https://whatismyipaddress.com](https://whatismyipaddress.com): 접속한 PC의 IP 관련 정보를 보여주는 사이트 #1. 예전에 bot이 있었는데 사라짐.
- [https://ipinfo.io](https://ipinfo.io): 접속한 PC의 IP 관련 정보를 보여주는 사이트 #2


## 15. 버전 관리

- [⭐Sublime Merge](https://www.sublimemerge.com): Git GUI 클라이언트 #1. 기본적으로 무료 앱이며 다크 모드를 쓰고 싶을 때만 결제하면 된다. 속도가 CLI 수준으로 빠르고 키보드 단축키 지원이 훌륭한 편.
- [Fork](https://fork.dev): Git GUI 클라이언트 #2. 가볍고 그래프가 보기 좋은게 특징. 무료지만 후원 방식으로 라이선스 구입이 가능하다.
- [GitKraken](https://www.gitkraken.com): Git GUI 클라이언트 #3. 속도는 느리지만 편의성은 탑. 💰사설 서버 혹은 비공개 저장소는 유료버전이 아니면 사용할 수 없다.
- [gitui](https://github.com/extrawurst/gitui): Mdir(?) 스타일의 GUI 앱. 옛날 갬성이 좋으면 쓸만하다. 단점으로 커밋 그래프 기능이 없고, 셸에서 저장소 경로로 이동하여 실행하는 방식이라 여러 저장소를 관리하기 불편하다.
- [GitHub Desktop](https://github.com/apps/desktop): GitHub 공식 GUI 클라이언트. 단점으로 커밋 그래프 기능이 없다.


## 16. 코드 하이라이팅

- [prismjs](https://prismjs.com): 코드 하이라이팅 JS 라이브러리. HTML로 작성된 페이지는 어디든 적용할 수 있음. [깃허브](https://github.com/PrismJS/prism)
- [Code Beautify](https://codebeautify.org/code-highlighter): 인라인 코드 하이라이터 #1. 입력한 텍스트에 CSS를 적용해 HTML로 만들어주는 사이트. 코드 블록을 지원하지 않는 메일이나 게시판에서 사용하기 좋다.
- [Color Scripter](https://colorscripter.com): 인라인 코드 하이라이터 #2.
- [hilite.me](http://hilite.me/): 인라인 코드 하이라이터 #3


## 17. 생산성, 관리

### 일정, TODO

- [⭐Workflowy](https://workflowy.com/): 온라인 할 일 관리 도구
- [⭐ClickUp](https://app.clickup.com/): 온라인 프로젝트 관리 도구. 무료 플랜도 쓸만하고 간트 차트를 제공한다.
- [Calendly](https://calendly.com): 일정 예약 + 스케줄링 자동화 도구. 메일로 일정 맞추는 신박한 기능도 있다.
- [Linear](https://linear.app/): 소프트웨어 개발팀용 이슈 트래커/프로젝트 관리 도구
- [Markwhen: Project planning example](https://markwhen.com/): 코드로 프로젝트 일정 표를 만드는 사이트

### 워크플로 자동화

- [n8n](https://n8n.io/): 오픈 소스 워크플로 자동화 도구. API 호출, 데이터 통합, 스케줄링 등을 통해 애플리케이션 간 워크플로를 자동화. 노코드/로우코드 방식으로 직관적인 워크플로 설계 가능. n8n은 nodemation을 줄인말이고, node-based automation tool이란 뜻이다.
- [Make](https://www.make.com/en/integromat-evolves-to-make): 옛날 이름은 Integromat
- [Zapier](https://zapier.com/)
- [Node-RED](https://nodered.org/)

### 메일 서비스

- [Guerrilla Mail](https://www.guerrillamail.com/): 일회용 메일 서버. 60분 동안만 유효한 메일 주소를 제공.
- [sharklasers.com](https://www.sharklasers.com): 일회용 메일 서버 #2. 이건 꽤 오래감.
- [Firefox Relay](https://relay.firefox.com): 파폭 계정으로 사용한 메일에 별칭을 만들 수 있고, 해당 별칭으로 메일이 오면 파폭이 포워딩 해줌. fㅔ이크 메일을 진짜처럼 쓸 수 있게 해주는 것.
- [Mailgun](https://www.mailgun.com/): 클라우드 기반 메일 인프라 서비스. 웹 API를 통해서 메일을 발송할 수 있다. 하루 100건은 무료
- [stibee](https://www.stibee.com/): 메일 마케팅 서비스. 읽기 쉬운 메일 작성 지원

### 결제

- [stripe](https://stripe.com): 결제 대행 웹 앱. 해외판 PG사라고 생각하면 된다. 노션과 험블번들에서 쓰더라.
- [PortOne](https://portone.io/korea/ko): 30분이면 된다는 통합 결제 서비스. 이 서비스는 PG의 PG에 가깝다(?).
- [i'mport](https://www.iamport.kr): 아임포트. 결제/본인인증 중계 서비스. 예를 들면 쇼핑몰과 PG사(혹은 신용조회회사)의 중간에 위치한다고 보면 됨.


## 18. AI

- [ChatGPT](https://chat.openai.com/): OpenAI 사의 GPT 기반 대형 언어 모델 #1
- [GPTForge](https://gptforge.net/): GPT를 활용한 웹앱, 툴, 앱 등을 모아놓은 사이트. 누가 따로 모으는 게 아니라 만든 사람들이 껴달라고 신청하는 것 같다.
- [FUTUREPEDIA](https://www.futurepedia.io/): AI 관련 도구 모음 사이트
- [Teachable Machine](https://teachablemachine.withgoogle.com/): 구글 티처블 머신. 초등학생도 사용할 수 있는 웹 기반 머신 러닝 도구다. 아직(2023-12-28)은 오디오나 이미지 정도만 지원함.
- [Claude](https://claude.ai/): Anthropic 사의 GPT 기반 대형 언어 모델 #2. 발음은 '클로드'. 스스로 주장하기를 ChatGPT보다 성능이 좋다고 함.
- [Tabnine](https://www.tabnine.com/): 코파일럿 같은 코드 어시스턴트. 특징으로는 내 코드에서 모델을 학습한다는 것. 개인의 코딩 패턴, 팀 전체의 코딩 스타일 등을 학습하고 여기에 맞춰 코드를 추천해 준다고 한다. 로컬 기반 학습이라 데이터 유출 문제에서도 안심등심.
- [Grok](https://grok.com/): xAI에서 개발한 대형 언어 모델 #3
- [MagicPath AI](https://www.magicpath.ai/): 자연어 기반 프롬프트로 실제 작동하는 프로덕션 레벨의 프론트엔드 코드를 생성하는 AI 도구. 완성된 컴포넌트를 브라우저에서 확인하는 건 무료, 💰코드 생성부터는 유료다. 비슷한 서비스: [Stitch](https://stitch.withgoogle.com/), [Readdy](https://readdy.ai/)
- [⭐DeepWiki](https://deepwiki.org/): 깃 저장소를 분석해서 사람이 읽기 위한 개요 문서를 만들어준다. 이 문서는 해당 코드베이스가 어떻게 작동하는지, 구조가 어떤지를 다이어그램과 함께 설명한다. 추가 질문을 답변해주는 AI 채팅도 지원함. 비공개 저장소는 Devin 계정과 연결해야 하는데, 💰유료인지 아닌지는 안해봐서 몰?루
- [Gitingest](https://gitingest.com/): 깃 저장소의 전체 코드베이스를 LLM에 최적화된(prompt-friendly text) 마크다운 파일 하나로 만들어주는 사이트. 깃허브 저장소 URL에서 `hub`만 `ingest`로 바꿔주면 바로 결과를 받아볼 수 있다.
- [GitMCP](https://gitmcp.io/): 깃허브 저장소의 전체 코드베이스를 MCP 서버(AI-accessible documentation hubs)로 만들어준다. Gitingest가 만드는 파일은 용량이 매우 커서 LLM에 그대로 제출하면 토큰을 많이 잡아먹는데, 만약 LLM 토큰을 아끼고 싶다면 이쪽이 좋다. 깃허브 저장소 URL에서 `hub.com`을 `mcp.io`로만 바꿔주면 결과가 나온다.


## 19. 모바일 개발

- [troy](http://troy.labs.daum.net)
- [Adobe Edge Inspect](https://creative.adobe.com/ko/products/inspect)
- [browser-deeplink](https://github.com/hampusohlsson/browser-deeplink): 브라우저에서 앱 실행


## 20. 기타 유틸리티

### 개발 관련

- [⭐mockaroo](https://www.mockaroo.com/): mock 데이터(더미 데이터, 가짜 데이터) 만들어주는 사이트
- [⭐Small Dev tools](https://smalldev.tools/): 인코딩/디코딩, 포매터, 테스트 데이터 생성 등 개발에 필요한 웹 도구 모음.
- [Itty bitty](https://itty.bitty.site): 간단한 서식의 글을 작성하고 URL로 공유하는 사이트. 데이터베이스를 사용하지 않고 URL에 작성한 글 내용이 모두 담겨있는 게 특징. 설명서는 [여기에](https://github.com/alcor/itty-bitty/wiki/).
- [TypeForm](https://www.typeform.com): 설문 조사용 웹 사이트. 여태 봤던것 중 가장 깔끔. 💰유료일듯?
- [Chatbase](https://www.chatbase.co/): 웹 사이트에 위젯처럼 간단히 추가할 수 있는 AI 챗봇.
- [GitBook](https://www.gitbook.com/): 마크다운으로 웹 문서 만드는 사이트. 웹에서 직접 에디트도 가능하지만 도저히 쓸 물건이 아니라서(다국어 입력하다 보면 먹통됨) 마크다운이나 노션으로 작성한 후 복붙해야 됨. 문서 버전 관리보단 완성된 결과물의 출판용으로 적합한 서비스.
- [ON24](https://www.on24.com/): 웨비나(Webinar, 웹 세미나) 서비스 사이트. Why Slack에서 쓰길래 줍줍
- [Firefox Monitor](https://monitor.firefox.com): 다른 사이트 가입할 때 사용한 내 계정 정보가 털렸는지 안털렸는지 알려줌
- [evanw: Source Map Visualization](https://evanw.github.io/source-map-visualization/): JavaScript 소스 맵 시각화 도구 #1
- [sokra: source-map-visualization](https://sokra.github.io/source-map-visualization/): JavaScript 소스 맵 시각화 도구 #2
- [Meta Tags](https://metatags.io/): 메타 태그 만들어주는 사이트.
- [⭐Emmet](https://emmet.io/): 예전 이름은 Zen Coding. 마크업이나 CSS 코드를 짧은 문법을 통해 자동으로 확장해주는 코드 단축 도구. 웬만한 편집기나 IDE에는 기본으로 포함되어 있다.
- [Advent of Code](https://adventofcode.com/): 매년 일정 시간마다 하나씩 공개되는 프로그래밍 퍼즐 사이트. 모든 언어로 풀 수 있음. ~~UI에서해커냄새가난다~~

### 시스템 유틸리티

- [⭐Microsoft PowerToys](https://learn.microsoft.com/en-us/windows/powertoys/install): 윈도우 고급 사용자를 위한 유틸리티 모음. Color Picker, 항상 위, 마우스 찾기, PowerToys Run(윈도우판 Spotlight) 같은 기능을 추가해 준다. 그 중 가장 쩌는건 **PowerRename**(이제 파일명 바꾼다고 코딩 안해도 됨 🥹) 
- [Caffeine](https://www.zhornsoftware.co.uk/caffeine/index.html): ~~월급루팡의 필수품~~ PC가 절전 모드 혹은 화면 보호기 모드로 바뀌지 않게 해주는 앱. 개발사는 Zhorn Software.
- [RunCat](https://github.com/Kyome22/RunCat_for_windows/releases): CPU 사용량이 높을 수록 다리가 빨라지는 고양이. 트레이에 거주함.
- [스텔라리움](https://stellarium.org/ko/): 스텔라륨. 오픈 소스 천체 투영관
- [SoundSwitch](https://soundswitch.aaflalo.me): 오디오 장치가 둘 이상일 때 출력 선택을 단축키로 변경할 수 있음
- [Meld](https://meldmerge.org/): 윈도우 용 GUI diff 앱. 파일 비교 후 머지까지 할 수 있고 3-way merge도 가능. macOS는 아직 지원 안함.
- [FFmpeg](https://ffmpeg.org/): CLI 방식의 동영상 변환/편집 도구. [니콜라스 유튜브 \| FFmpeg 소개 영상](https://www.youtube.com/watch?v=z2iodiQW0fg)
- [CPU-Z](https://www.cpuid.com/softwares/cpu-z.html): 설치된 컴퓨터의 하드웨어 스펙 조회 유틸리티. 칩셋, 캐시, 메인보드, 메모리, 그래픽카드의 모델명과 스펙이 표시됨.
- [HWMonitor](https://www.cpuid.com/softwares/hwmonitor.html): 하드웨어 모니터링 유틸리티. 쿨링 팬 속도, 사용 전압, 온도 등을 실시간으로 보여 줌.
- [OCCT](https://www.ocbase.com/download): 오버클럭 테스트용으로 사용하는 과부하 앱인데, 밴치마킹, 모니터링, 안정성 테스트, 시스템 정보 확인 등의 기능도 제공한다.
- [한국표준과학연구원: 표준시각 맞추기](https://www.kriss.re.kr/menu.es?mid=a10305020000): 한국표준과학연구원에서 제공하는 현재 시간 및 세계 시간 확인/동기화 유틸리티. 설치형이고 앱 이름은 UTCk, 현재(2023-08-09) 버전은 3.1
- [Revo Uninstaller](https://www.revouninstaller.com/): 앱을 삭제할 때 레지스트리 같은 일종의 찌꺼기(?)도 완전히 삭제해 준다는 언인스톨러. 상용인 Pro 버전이 따로 있음.
- [구라제거기](https://teus.me): 악성 코드 제거 도구

### 매크로, 키 매핑

- [autohotkey](https://www.autohotkey.com): 스크립트로 작성하는 매크로 프로그램
- [joytokey](https://joytokey.net/en): 게임패드-키보드(와 마우스) 매핑 앱
- [REWASD](https://www.rewasd.com/map-xbox-elite): 게임패드-키보드(와 마우스) 매핑 앱. 💰매크로와 터보 기능은 유료다.

### 미디어 플레이어

- [VLC media player](https://www.videolan.org/vlc/): 기본 기능에 충실한 미디어 플레이어

### 미디어 편집

- [⭐paint.net](https://www.getpaint.net): 이미지 편집기. 좋음
- [Audacity](https://www.audacityteam.org/download/): 간단한 음원 편집기
- [PDF2JPG](https://pdf2jpg.net): PDF를 JPG로 변환
- [Segment Anything](https://segment-anything.com/): AI로 만든 자동 누끼(?) 앱이라는데 아직 안 써봄. 일단 깃허브 설명을 보면 Python으로 실행하는 모양
- [OpenCut](https://github.com/OpenCut-app/OpenCut): 오픈 소스 영상 편집기. React 기반의 웹 애플리케이션이며 소스를 받아 로컬 서버를 띄워 사용하는 방식이다.

### 원격 연결

- [Parsec](https://parsec.app/):
- [RustDesk](https://rustdesk.com/):
- [AnyDesk](https://anydesk.com/):
- [Splashtop](https://www.splashtop.com/):
- [Jump Desktop](https://jumpdesktop.com/):


## 21. 브라우저, 플러그인

### 브라우저

- [Firefox](https://www.mozilla.org/en-US/firefox/new/): 불여시 브라우저. 개발자용 불여시는 [여기로](https://www.mozilla.org/en-US/firefox/developer/). `about:config`에서 `browser.uidensity`를 1로 해볼 것
- [DuckDuckGo browser](https://duckduckgo.com/app)
- [Brave](https://brave.com/): 프라이버시와 성능에 중점을 둔 크로미움 기반 브라우저 #1
- [Ladybird](https://ladybird.org/): 다른 브라우저의 코드를 사용하지 않는 독립적인 웹 브라우저 겸 엔진. 2026년 여름 정식 출시 예정
- [Nyxt](https://nyxt.atlas.engineer/): 해커용 브라우저(?). 고급 사용자를 위한 브라우저로, 키보드 탐색, 점프 헤딩, 쉬운 탭 관리, 명령 퍼지 검색, 내장 REPL, 스마트 북마크 검색, 사용자 정의 가능한 자동 완성, 클립보드 기록, 트리 기반의 방문 페이지 기록 등의 기능을 제공한다고 함. 2024-12-09 기준 윈도우는 아직 미지원.
- [Chromium](https://www.chromium.org/): 구글의 오픈 소스 웹 브라우저 프로젝트. 크롬의 기반 코드이며 요즘(2023-09-13) 점유율 높은 브라우저들은 대부분 Chromiun 코드베이스를 사용한다.

### Firefox 플러그인

- Mate Translate
- PocketTube
- Save webP as PNG or JPEG
- uBlacklist
- uBlock Origin

### Chrome 플러그인

- Go Back With Backspace
- Mate Translate
- Chrome Remote Desktop

여기에 개발용 크롬이면:

- CSSViewer
- JSON Formatter
- Vue.js devtools
- Wappalyzer
- Authenticator(인증 도구)


## 22. 기타 서비스

- [stream](https://getstream.io/): 채팅 관련 오픈 소스 같은데 뭔지 잘 몲
- [Sanity](https://www.sanity.io/): CMS(Content Management System)라는데 이게 뭘까
- [Apache Tika](https://tika.apache.org/): 파일 콘텐츠를 분석해주는 Java 라이브러리
- [⭐KeystoneJS](https://keystonejs.com/): 어드민 패널 라이브러리. 애플리케이션에 필요한 관리자 화면을 만들어주는 라이브러리다. JavaScript 혹은 TypeScript로 사용할 수 있음. [니콜라스 유튜브 \| KeystoneJS 소개 영상](https://www.youtube.com/watch?v=DlyoFFOcPCg)
- [Chosic](https://www.chosic.com/): 비슷한 노래 찾기 등 노래 관련 탐색 서비스 제공하는 사이트
- [Everynews](https://every.news/): 뉴스에서 관련성 높은 정보를 적절하게 필터링해 전달하는 모니터링 플랫폼. 뉴스레터 서비스의 일종이긴 한데 AI를 끼얹었다. 원하는 주제를 구독하는 방식이다. 해커뉴스 번역판이 가장 인기 많음
- [프리랜서를 위한 메세지 템플릿 서비스](https://fressa.vercel.app/)


## 23. 통계, 분석

- [Worldometer](https://www.worldometers.info): 전 세계의 각종 정보 실시간 카운터. 인구 증감, 남은 석유량 등
- [statcounter](https://gs.statcounter.com): 전 세계 대상 브라우저, OS 점유율 등의 통계자료 조회 사이트
- [A/B Testing Significance Calculator](https://neilpatel.com/ab-testing-calculator): A/B 테스트 통계적 유의성 계산기. 신뢰도를 계산해주는 사이트인가보다. [통계의 신뢰구간과 신뢰도](https://m.blog.naver.com/PostView.nhn?blogId=baboedition&logNo=220916281966&proxyReferer=https%3A%2F%2Fwww.google.com%2F)
- [built with: trends](https://trends.builtwith.com/): 웹/인터넷 기술 사용 트랜드와 통계 보기. 여기는 원래 특정 사이트가 뭘로 만들어진건지를 분석해주는 사이트임.


## 24. 문서 작성 도구

- [WPS Office](https://www.wps.com): 무료 플랜은 기본 기능과 1GB 클라우드 사용 가능.
- [LibreOffice](http://ko.libreoffice.org/download)

### Presentation

- [Prezi(web)](http://prezi.com)
- [Slideshare(web)](http://www.slideshare.net)
