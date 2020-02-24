---
layout: post
date: 2019-04-15 15:37:00 +0900
title: '[Eclipse] eclipse 탐구생활'
categories:
  - eclipse
tags:
  - devtool
  - eclipse
---

* Kramdown table of contents
{:toc .toc}

#### 관련 문서

- [somewhere](somewhere)

## 빌드

#### 용어 참고

- `Source`: 파일 탐색 뷰 > 각 프로젝트의 `Properties` > `Java Build Path` > `Source` 탭
- `Projects`: 파일 탐색 뷰 > 각 프로젝트의 `Properties` > `Java Build Path` > `Projects` 탭
- `Libraries`: 파일 탐색 뷰 > 각 프로젝트의 `Properties` > `Java Build Path` > `Libraries` 탭
- `Order and Export`: 파일 탐색 뷰 > 각 프로젝트의 `Properties` > `Java Build Path` > `Order and Export` 탭
- `Deployment Assembly`: 파일 탐색 뷰 > 각 프로젝트의 `Properties` > `Deployment Assembly` 메뉴


### Source

이클립스 빌더에게 소스 파일이 어느 경로에 있는지`Source folers on build path`와 컴파일한 파일을 어디로 내보낼지`Default output folder`를 알려주는 설정이다. 만약 소스 경로에 컴파일 할 수 없는(e.g. xml) 파일이 있으면 별도 작업 없이 그대로 내보내는걸로 추정된다.

### Projects, Libraries

빌드 시 참조할 이클립스 프로젝트 혹은 Jar, class 폴더를 설정한다.

#### maven dependency

pom.xml의 dependency 설정에 추가한 프로젝트가 들어오는걸로 추정됨.

### Order and Export

#### Order

빌드 시 패키지까지 완벽히 같은 타입이 존재할 때 **어느 Jar를 사용할지 결정하는 우선순위**. 위에서부터 우선순위가 빠르다.

#### Export

프로젝트가 다음과 같다고 가정할 때:

- primary: 주 작업 프로젝트
- includeMe: 참조 대상
- 필요한 jar는 includeMe 프로젝트에 있고 `Libraries` 탭에 추가되어 있는 상황

우선 includeMe 프로젝트의 `Libraries`에서 `Add Jar` 후 `Order and Export`에서 추가된 라이브러리를 export 하도록 체크해야 한다. (이 작업을 하지 않으면 export 대상에서 제외되어 다른 프로젝트에서 라이브러리를 참조할 수 없다.) primary 프로젝트의 `Projects` 탭에서 참조할 프로젝트인 includeMe를 `Add` 하면 끗.

이후 primary 프로젝트에서 includeMe 프로젝트의 jar를 import 할 수 있게된다. 그리고 이건 이클립스 내에서만 잘 돌아가는 설정이다. 만약 WAR로 빌드 후 별도의 WAS에서 구동하면 `Deployment Assembly`에서 includeMe를 추가해도 정상작동 하지 않는다.

## 빌드 - JST Server Adapter

보통은 아파치-톰캣으로 사용하는 서버 확장기능과 관련된 설정이다. 이 설정을 어떻게 하느냐에 따라 WAS를 위해 빌드하는 방식이 다르며, 메이븐/그래들 같은 빌드 툴을 사용하지 않은 환경에서는 아주 중요한 설정이다. 상당히 번거롭고 개발툴에 빌드 설정을 의존하기 때문에 가능하면 안쓰는게 좋다.

**WAS의 publish**: WAS의 publish란 WAS가 working directory(우리가 직접 보고 수정하는 작업 파일들)를 직접 읽지 않고 별도의 공간에 WAS 자신이 읽을 수 있는 모양의 형태로 배포하는것을 말한다. `eclipse-workspace\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps` 보통은 요딴 경로에 있으니 WAR로 빌드하기 전에 어떤 모양이 나오는지 미리 확인하거나 디버깅(특정 파일이 있는지 없는지) 하는 식으로 활용할 수 있다.

**배포**: WAR나 풀려있는 WAR로 내보내는 행위

### Projects, Libraries

여기에 추가된 항목은 빌드할 때 사용하긴 하지만 배포할 떄도 그런건 아니다. 배포에 포함하고 싶다면 `Deployment Assembly` 설정에서 추가한다.

**...라고 정리해놨으나, 현장에서 `Deployment Assembly` 설정 없이 돌아가는 배포 환경을 목격함. 실제 배포 대상에 없는 파일을 JST 서버에서 읽는다.** 이건 톰캣7 설정 중 loader-virtualClasspath 속성으로 특정 프로젝트의 실제 경로를 지정하는 방식이었다. 빌드 아웃풋 경로를 WAS 설정에 하드코딩 하다니, 정말 듣도보도 못한 발상이다. 톰캣 버전이 올라가면서 이 속성은 없어진것 같다.

### Deployment Assembly

Deployment Assembly는 JST 확장기능이 웹앱을 배포할 때에 필요한 소스 위치와 배포 위치를 지정하는 설정이다.

![](/images/eclipse-탐구생활-1.png)

`Source`는 말 그대로 소스 파일의 원래 경로를 의미하고 `Deploy Path`는 소스 파일이 어느 경로로 배포될 지를 의미한다.

예를 들어 `Projects` 탭에서 AAA라는 프로젝트를 추가하면 WAS는 이 프로젝트의 작업 파일을 직접 참조한다. 이렇게만 하면 일단 컴파일 자체는 문제가 없지만, WAR 빌드 시에 AAA 프로젝트를 포함하지 않기 때문에 (JST가 아닌) 이클립스와 분리된 WAS에서 구동 시 `ClassNotFoundException`이 발생한다. 따라서 Deployment Assembly에서 AAA 프로젝트를 추가해 빌드 시 같이 내보내도록 설정해야 한다.

다른 빌드 패스 설정도 마찬가지지만 Deployment Assembly 또한 빌드 툴(메이븐/그래들)에 의해서 자동으로 설정될 수 있다. 메이븐의 경우 dependency로 다른 프로젝트를 참조하도록 설정하면 `Deployment Assembly`와 `Libraries` > `Maven Dependencies`에 같이 추가된다.

### WAS의 Source

JST 서버별 `Overview` > `Edit Configuration` > `Source`는 직접 설정하진 않지만 디버깅용으로 확인하는 정도... 이 설정에 있는 경로나 jar가 정확히 어떤 용도인지는 아직 몲.

`Projects`와 `Libraries` 탭에서 추가한 항목은 자동으로 이 설정에도 추가된다.

## 리펙터 Refactor

### pull up

피확장 클래스(=부모)로 멤버 이동. abstract 메서드는 이동이 아니라 메서드 선언만 복사하며 오버라이디드 메서드가 된다.

### pull down

확장 클래스(=자식)로 멤버 이동

## 파일 속성

### derived resource

파일이나 폴더 속성에서 `Derived` 속성에 체크하면 해당 파일은 '소스에서 파생된 파일'로 취급한다. 파생된 파일은 탐색이나 검색 등에서 자동으로 제외하는 옵션이 있다. 보통은 빌드 아웃풋 폴더(target, bin, build, ...)를 derived resource로 지정해서 컴파일된 클래스 파일이 검색 결과에 나타나지 않도록 설정한다.

## 이클립스의 환경 변수

### `${project_loc:some-project-name}`

TODO

## 빌드툴 관련

### TODO
