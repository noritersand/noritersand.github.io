---
layout: post
date: 1970-01-03 18:47:33 +0900
title: '[misc] 도토리 창고: 각종 툴, 유틸리티, 사이트 모음'
categories:
  - misc
tags:
  - misc
  - note
  - devtool
  - link
  - website
---

* Kramdown table of contents
{:toc .toc}

## 개요

쓰니는 다람쥐같은 습성이 있어서 일단 모으는 것을 좋아한다고 한다.

## 분류음슴

- [stripe](https://stripe.com): 결제 대행 웹 앱. 해외판 PG사라고 생각하면 된다. 노션, 험블번들에서 씀.
- [TypeForm](https://www.typeform.com): 설문 조사용 웹 사이트. 여태 봤던것 중 가장 깔끔. 유료일듯?
- [PDF2JPG](https://pdf2jpg.net): PDF를 JPG로 변환
- [GitBook](https://www.gitbook.com/): 마크다운으로 웹 문서 만드는 사이트. 웹에서 직접 에디트도 가능하지만 도저히 쓸 물건이 아니라서(다국어 입력하다 보면 먹통됨) 마크다운이나 노션으로 작성한 후 복붙해야 됨. 대신 퍼블리싱 결과물은 노션 읽기 전용 페이지보다 훌륭하다. 버전 관리보단 완성된 결과물의 출판용으로 적합한 서비스.
- [stream](https://getstream.io/): 채팅 관련 오픈소스 같은데 뭔지 잘 몲
- [ON24](https://www.on24.com/): 웨비나(Webinar, 웹 세미나) 서비스 사이트. Why Slack에서 쓰길래 줍줍

## 메뉴얼/API DOC/튜토리얼

- [MDN](https://developer.mozilla.org/): 그 MDN
- [⭐DevDocs](https://devdocs.io): 개발자용 API 문서 모음 사이트. [깃허브 링크](https://github.com/freeCodeCamp/devdocs)
- [⭐Can I use](https://caniuse.com/): 웹 API, HTML, CSS 등을 어떤 브라우저에서 지원하는지를 알려주는 사이트.
- [WikiDocs](https://wikidocs.net): 온라인 책 제작 공유, 프로그래밍 언어별 튜토리얼이 있음.

## 개발 지원 툴

- [link anatomy](http://bl.ocks.org/abernier/3070589): `location` 해부학(?)
- [JSON Placeholder](https://jsonplaceholder.typicode.com/): JSON 응답을 받아야하는데 백엔드를 만들기 귀찮으면 쓰는 Free Fake JSON API 서버.
- [⭐Small Dev tools](https://smalldev.tools/): 인코딩/디코딩, 포매터, 테스트 데이터 생성 등 개발에 필요한 유틸리티 모음.
- [Meta Tags](https://metatags.io/): 메타 태그 만들어주는 사이트.
- [OneLang.io](https://ide.onelang.io/): 개발 언어 병렬 번역기
- [Figstack](https://www.figstack.com/): 코드를 다른 언어로 번역, 영어로 해설, documentation comments 만들기, 시간 복잡도 계산, 작성한 코드 기반 자연어로 질문까지! 아직은 쪼끔 느린게 흠.
- [Crontab.guru](https://crontab.guru/): Cron(리눅스/유닉스의 스케줄러) 표현식을 테스트하거나 랜덤으로 만들어주는 사이트. Cron Job의 모니터링 소프트웨어를 파는 [Cronitor](https://cronitor.io/cron-job-monitoring?utm_source=crontabguru&utm_campaign=cronitor_button)에서 운영한다.

## 온라인 코드 에디터 겸 테스트 툴

- [CodePen](https://codepen.io)
- [JSFiddle](https://jsfiddle.net)
- [StackBlitz](https://stackblitz.com)
- [regular expressions 101](https://regex101.com/): 정규식 테스트 겸 코드 공유 사이트. (근데 왜 101일까)
- [RegExr](https://regexr.com/): 정규식 테스트 사이트
- [REGEXPER](https://regexper.com/): 정규식을 (나름)이쁘게 설명해줌. 요딴식으로 `https://regexper.com/#^M[^iI]*%3F[iI][^iI]*%3F%24` URL에 정규식을 때려넣는게 특징

## 개발용 컴포넌트, 써드파티 라이브러리

- [Air Datepicker](https://air-datepicker.com/): 프론트엔드용 달력 UI 라이브러리(일명 데이트피커). 퓨어 자바스크립트 기반. 언어 기본값이 러시아어인 걸 보니 러시아산인 모양
- [Tom Select](https://tom-select.js.org/): 셀렉트박스 UI 라이브러리. 퓨어 자바스크립트 기반
- [Toast UI Grid](https://ui.toast.com/tui-grid): MIT 라이선스의 그리드 UI 라이브러리. 가볍게 쓰기 매우 좋다. NHN에서 만듦
- [chart.xkcd](https://github.com/timqian/chart.xkcd): 자바스크립트로 만드는 차트. 결과물은 svg로 나옴. 발로 그린 것 같은 모양새가 특징(근데 그게 매력 터짐)

### UI 컴포넌트

부트스트랩같은 걸 생각하면 됨

- [Storybook](https://storybook.js.org/)
- [component.gallery](https://component.gallery/): UI 컴포넌트, 디자인 시스템을 모아놓은 사이트
- [리액트 UI](https://reach.tech/)
- [Headless UI](https://headlessui.dev/): 리액트 혹은 뷰에 적용할 수 있는 UI 컴포넌트
- [Vitebook](https://vitebook.dev/introduction/what-is-vitebook.html#playgrounds)
- [Quasar Framework](https://quasar.dev/): 뷰 전용인듯?

## 일정/TODO 관리

- [workflowy](https://workflowy.com): 온라인 TODO 툴
- [Calendly](https://calendly.com): 이메일로 일정 맞추기
- [Linear](https://linear.app/)
- [Markwhen: Project planning example](https://markwhen.com/): 코드로 프로젝트 일정 표를 만드는 사이트

## 키 리맵핑

- [autohotkey](https://www.autohotkey.com): 스크립트를 작성하고 실행하는 방식의 키보드 매크로
- [joytokey](https://joytokey.net/en): 게임패드-키보드(와 마우스) 키 매핑 앱.

## 스피커, 소리 출력, 사운드

- [SoundSwitch](https://soundswitch.aaflalo.me): 오디오 장치가 둘 이상일 때 출력 선택을 단축키로 변경할 수 있음

## 보안

- [Firefox Monitor](https://monitor.firefox.com): 다른 사이트 가입할 때 사용한 내 계정 정보가 털렸는지 안털렸는지 알려줌

## 통계

- [Worldometer](https://www.worldometers.info): 전 세계의 각종 정보 실시간 카운터. 인구 증감, 남은 석유량 등
- [statcounter](https://gs.statcounter.com): 전 세계 대상 브라우저, OS 점유율 등의 통계자료 조회 사이트
- [A/B Testing Significance Calculator](https://neilpatel.com/ab-testing-calculator): A/B 테스트 통계적 유의성 계산기. 신뢰도를 계산해주는 사이트인가보다. [통계의 신뢰구간과 신뢰도](https://m.blog.naver.com/PostView.nhn?blogId=baboedition&logNo=220916281966&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

## 마인드맵

- [XMind](http://www.xmind.net)
- [FreeMind](http://freemind.sourceforge.net/wiki/index.php/Main_Page)

## UI 디자인

- [Figma](https://www.figma.com): 요즘(2021-05-03) 뜬다는 UI 디자인 툴. 기본은 무료고, 대-충 비공개 프로젝트를 여러명이 사용할 땐 유료인듯
- [ThemeForest](https://themeforest.net/): 언어, 엔진, 프레임워크 별 테마(HTML과 CSS 묶음. 필요하면 JS까지) 파는 사이트

## ERD

- [dbdiagram.io](https://dbdiagram.io/): 웹 버전만 있긴 하지만 좋음.
- [eXERD](http://www.exerd.com): 30일 체험판이라 좀 그럼.

## UML/MDA/드로잉 툴

- [Excalidraw](https://excalidraw.com/): 웹 전용 드로잉 툴. 무료. 가볍게 쓰기 좋음.
- [Balsamiq](https://balsamiq.com/wireframes/): UML, 드로잉 툴. 아마 유료임.
- [StarUML](http://staruml.sourceforge.net/ko)
- [Draw.io (web)](http://www.draw.io)
- [Draw.io Pro (chrome app)](https://chrome.google.com/webstore/detail/drawio-pro/onlkggianjhjenigcpigpjehhpplldkc?utm_source=plus)
- [Gliffy (web)](http://www.gliffy.com)
- [45+ 온라인 드로잉 툴](http://www.smashingapps.com/2011/08/26/45-free-online-tools-to-create-charts-diagrams-and-flowcharts)
- [Fluent Icons](https://fluenticons.co/): 마소의 오픈 소스 아이콘 저장소. 마소가 만든건 아님. SVG 혹은 PNG로 받을 수 있다.

## 공인 IP 확인

- [https://icanhazip.com](https://icanhazip.com): 딱 내 공인 IP만 텍스트로 응답하는 서버. icanhaz 시리즈가 몇 개 더 있으니 [여기1](https://major.io/icanhazip-com-faq)랑 [여기2](https://github.com/major/icanhaz)를 보자.
- [https://whatismyipaddress.com](https://whatismyipaddress.com): 내 공인 IP 정보 확인하는 사이트. 예전에 bot이 있었는데 사라짐.
- [https://ipinfo.io](https://ipinfo.io): 내 공인 IP 정보 확인하는 사이트.#2

## Git GUI 툴

- [Fork](https://fork.dev): 요즘 잘 쓰고 있는 중요한 기능만 있어 가볍고 UI가 매우 보기 편한게 특징인 툴. 무료지만 후원 방식으로 라이선스 구입이 가능하다. 구입하면 보상은 하트 ❤ ~~난 이미 사슴~~
- [GitKraken](https://www.gitkraken.com): GUI 툴 중엔 ~~속도는 느리지만~~ 편의성은 탑. 그런데 사설 서버 혹은 비공개 저장소는 유료버전이 아니면 사용할 수 없다. 😩
- [Sublime Merge](https://www.sublimemerge.com): 지원하는 기능은 Fork나 GitKraken에 비해서 딸리지만 속도가 지이이이인짜 빠르다.
- [gitui](https://github.com/extrawurst/gitui): Mdir(?) 스타일의 GUI 툴.

## 트래픽 캡쳐

- [Fiddler](http://www.telerik.com/fiddler): 네트워크 디버깅 툴. 파폭은 SSL 인증서 때문에 빡칠 수 있다. 유료
- [Wirehsark](https://www.wireshark.org/download.html): 패킷 추적 툴.

## 코드 하이라이팅/컬러링

- [prismjs](https://prismjs.com): 코드 하이라이팅 JS 라이브러리. HTML로 작성된 페이지는 어디든 적용할 수 있음. [깃허브](https://github.com/PrismJS/prism)
- [Color Scripter](https://colorscripter.com): 입력한 텍스트에 CSS를 적용해 HTML로 만들어주는 사이트. 코드 블록을 지원하지 않는 메일이나 게시판에서 사용하기 좋다.

## DBMS tool

- [DBeaver](https://dbeaver.io): 벤더 안가리는 툴. 이클립스와 같은 UI 프레임웍으로 추정.
- [Oracle SQL Developer](http://www.oracle.com/technetwork/developer-tools/sql-developer/overview/index.html): SQL Developer는 오라클 전용 DBMS 툴로 런타임 환경이나 instant client 없이 서버에 연결할 수 있다.
- [Toad](http://www.toadworld.com/m/freeware/default.aspx)
- [SQLGate](http://www.sqlgate.com/kr/download)
- [Orange](http://www.warevalley.com/xml/download/orange_trial)
- [QueryBox](http://www.querybox.com): 벤더 가리지 않고 접속할 수 있는 국산 툴. 기업용은 유료.

## 텍스트 에디터

- [Sublime Text](https://www.sublimetext.com/blog/articles/sublime-text-4)
- [Notepad++](https://notepad-plus-plus.org)
- [UltraEdit](http://www.ultraedit.com/loc/ko/index_ko.html)
- [Atom](https://atom.io): 깃 허브 페이지용으로 딱 좋음
- [Nova](https://nova.app)
- [typora](https://typora.io): 마크다운 전용 에디터
- [Visual Studio Code](https://code.visualstudio.com): 요즘 대세

## SSH/텔넷/FTP 클라이언트

- [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty): SSH = putty
- [Xshell](http://www.netsarang.co.kr/download/main.html): 가장 좋으나 유료 라이선스.
- [WinSCP](https://winscp.net/eng/download.php)
- [bitvise](https://www.bitvise.com/ssh-client-download): 서버 딱 하나에 붙는 용도로는 아주 좋다. 여러 서버에 붙으려면 매번 프로필들을 불러와야 해서 불편.
- [MobaXterm](https://mobaxterm.mobatek.net/download.html): 무료 툴 중에서 여러 서버 동시 접속 기능은 그나마...

## Presentation

- [Prezi(web)](http://prezi.com)
- [Slideshare(web)](http://www.slideshare.net)

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

## Java decompiler

- [JD Project](http://java-decompiler.github.io)

2020-01-06 기준 Java 1.1.8 부터 12까지 지원한다고 한다.

## Web server/WAS

- [Apache HTTP Server](https://www.apachelounge.com/download): 그 아파치. 검색으론 다운로드 링크 찾기 힘듬.

## Project manager / Issue tracker

- [Trello](https://trello.com)
- [Taiga](https://taiga.io)
- [JIRA](https://ko.atlassian.com/software/jira): 유료, 설치형

## Office

- [WPS Office](https://www.wps.com): 무료 플랜은 기본 기능과 1GB 클라우드 사용 가능.
- [LibreOffice](http://ko.libreoffice.org/download)

## 메일 서비스/메일 서버

- [stibee](https://www.stibee.com/?utm_source=stibee&utm_campaign=sponsorbanner&utm_medium=email&uid=b2ZmaWNpYWxAbmV3bmVlay5jbw): 이메일 마케팅 서비스. 읽기 쉬운 메일 작성 지원. 메일링.
- [https://www.guerrillamail.com/ko](https://www.guerrillamail.com/ko): 일회용 이메일 서버. 60분 동안만 유효한 메일 주소를 제공.
- [https://www.sharklasers.com](https://www.sharklasers.com): 일회용 이메일 서버#2. 얘는 꽤 오래감.
- [Firefox Relay](https://relay.firefox.com): 파폭 계정으로 사용한 이메일에 별칭을 만들 수 있고, 해당 별칭으로 메일이 오면 파폭이 포워딩 해줌. fㅔ이크 이메일을 진짜처럼 쓸 수 있게 해주는 것.

## 테스트 툴

- [postman](https://www.getpostman.com): HTTP Request/Response 테스트

## 분석/프로파일링/모니터링 툴

APM(Application Performance Monitoring) 툴 혹은 프로파일링 툴들. 얘네들은 웬만하면 상용툴이다.

- [Datadog](https://www.datadoghq.com): 데이터독. 인프라 모니터링. APM 기능도 있지만 이것보다 시스템 성능 지표 분석 기능이 주력이다. 설치형이 아니라 데이터는 저쪽에서 관리하며, 비싸다.
- [제니퍼](https://jennifersoft.com/ko/product/java): 자바앱 모니터링
- [VisualVM](https://visualvm.github.io): 프로파일링 툴#1. 자바 앱용. VM의 환경, CPU와 메모리의 사용량, 클래스와 쓰레드의 점유율, CPU/메모리/JDBC 프로파일링 등의 기능을 제공한다. 오픈소스
- [Eclipse Memory Analyzer](https://www.eclipse.org/mat/downloads.php): 프로파일링 툴#2. 자바 앱용. VisualVM보다 기능이 조금 더 많고 도움말이 잘 되어 있어서 쓰기 편함. UI는 이클립스 기반임. 오픈소스
- [XRebel](https://www.jrebel.com/products/xrebel): 프로파일링 툴#3. 자바 웹 앱 전용이다. 상용
- [JProfiler](https://www.ej-technologies.com/products/jprofiler/overview.html): 프로파일링 툴#4. 10일 무료. IDE와 연동할 수 있음. 상용
- [YourKit Java Profiler](https://www.yourkit.com/java/profiler/features): 프로파일링 툴#5. 15일 무료. 얘도 IDE 연동 쌉가능. 단, Java 1.7 미만의 환경은 지원하지 않는다. 상용
- [Jennifer](https://jennifersoft.com/ko/product/java): APM 툴#2. 자바/PHP/닷넷 앱 모니터링. 상용
- [WhaTap](https://www.whatap.io/ko): APM 툴#3. 앱/서버/DB/URL/컨테이너/인프라 모니터링. 상용이며 한국기업이라 한국어판을 제공한다.
- [Pinpoint](https://pinpoint-apm.gitbook.io/pinpoint/): 오픈 소스 APM. 네이버에서 만들었다 함
- [Scouter](https://github.com/scouter-project/scouter): 오픈 소스 APM. LG CNS랑 관련이 있나 봄. 이거 만든 사람들이 WhaTap 만들었다고 하던디...?

## 그리드

- [ag-grid](https://www.ag-grid.com)
- [jqgrid](http://www.trirand.com/blog)
- [jqxgrid](https://www.jqwidgets.com/jquery-widgets-demo/demos/jqxgrid/index.htm)

## 이미지 편집기

- [paint.net](https://www.getpaint.net)
