---
layout: post
date: 2023-01-17 17:10:43 +0900
title: '[Spring] 스프링 부트'
categories:
  - spring
tags:
  - java
  - spring
  - spring-boot
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [http://projects.spring.io/spring-boot/](http://projects.spring.io/spring-boot/)
- [http://start.goodtime.co.kr/2014/10/이제는-spring-boot를-써야할-때다/](http://start.goodtime.co.kr/2014/10/이제는-spring-boot를-써야할-때다/)

#### 버전 정보

- 이 글 작성 시점은 2.7.3


## 개요

어쩌구


## 스태틱 파일

스프링 부트는 요 경로들에 있는 정적 웹 리소스(js, css, ...)를 자동으로 추가한다고 한다:

- /META-INF/resources/
- /resources/
- /static/
- /public/

❓ 아마 WEB-INF/classes/static으로 추가하는 것 같은데, 여기 접근은 어떻게 되는 걸까.
❓ 게다가 실제 자원 접근 시 URL에 WEB-INF/classes/static 없이도 가능함.
❓ WEB-INF/classes 아래에 static이나 public 등의 이름을 가진 디렉터리가 있으면 부트가 자동으로 연결해주는 게 아닐까?

### WEB-INF 아래 리소스에 직접 접근이 가능하다?

K뿅뿅 프로젝트 때 발견한 것

- spring-boot + maven 환경이었는데 어떻게 한건지는 모름.
- `src/main/resources/static` 경로에 있는 스크립트 파일들이 `WEB-INF/classes/static`으로 배포가 되며(WAR 기준. 메이븐 output 폴더 기준으로는 `target/classes/static`), 이 스크립트를 URL로 직접 접근이 가능했다.
- 동시에 다른 프로젝트의 resources를 복사해오는 메이븐 플러그인도 적용함.
- 곁다리: 듣자하니 resources 복사하는 메이븐 플러그인이 빌드 시에만 작동하는게 아니라 실시간으로 반영되게 할 수도 있다 함.


## 로컬 환경에서 서버 재시작 없이 JSP 리로드

외부 WAS를 사용할 때 그냥 되는 게 스프링 부트에선 안되는데, Spring Developer tools를 설치해야 한다:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <version>x.x.x</version>
</dependency>
```

debug/run 모드인지는 상관 없음.

구글링 하면 나오는 Live Reload는 별도의 훅과 브라우저의 확장 기능을 이용한 자동 새로고침 기능이다.
