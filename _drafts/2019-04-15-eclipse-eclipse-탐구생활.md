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

#### 참고한 문서

- [somewhere](somewhere)

#### 용어 참고

- `Source`: 파일 탐색기의 각 프로젝트를 우클릭 후 `Properties > Java Build Path > Source`
- `Projects`: 파일 탐색기의 각 프로젝트를 우클릭 후 `Properties > Java Build Path > Projects`
- `Libraries`: 파일 탐색기의 각 프로젝트를 우클릭 후 `Properties > Java Build Path > Libraries`
- `Order and Export`: 파일 탐색기의 각 프로젝트를 우클릭 후 `Properties > Java Build Path > Order and Export`
- `Deployment Assembly`: 파일 탐색기의 각 프로젝트를 우클릭 후 `Properties > Deployment Assembly`


## hot code replace

- [http://www.mkyong.com/eclipse/how-to-configure-hot-deploy-in-eclipse/](http://www.mkyong.com/eclipse/how-to-configure-hot-deploy-in-eclipse/)

이클립스에선 디버그 모드로 로컬 서버 기동 후 클래스 파일의 변경이 감지되면 재시작 없이 변경된 클래스파일을 교체하는 Hot code replace 기능을 제공한다. 관련 기술로 Hot deploy, Hot swap이 있는데 JPDA에서 지원하는 건 Hot swap, WAS에서 지원하는 건 Hot deploy라고 구분하는 모양이다.

그리고 디버그 모드라고 해도 직접 만든 '무한루프로 대기하면서 종료되지 않는 코드'는 핫스왑이 안되는 걸 보면 이 기능이 IDE에서 지원하는건지 아니면 톰캣(혹은 특정 스펙을 준수하는 WAS)에서 지원하는 건지는 확실하지 않음.

### 설정

여기선 톰캣 플러그인을 예로 든다. 우선 톰캣 서버 설정(서버 이름을 더블클릭 해서 진입한다. 서버 목록은 이클립스에서 Servers view를 활성화해야 볼 수 있다.)의 Overview 탭에서 'Automatically publish when resources change' 항목에 체크하고 (만약 'Serve modules without publishing'을 활성화했다면 이 과정은 생략해도 된다):

![](/images/hot-code-replace-1.png)

Modules 탭에서 보이는 Edit 버튼을 눌러 Auto reloading enabled의 체크를 해제한다.
이후 WAS를 디버그 모드로 구동한 뒤 소스를 수정하면 된다.

### 주의사항

hot code replace의 적용 범위는 메서드 본문 한정이다. 다음에 해당하는 코드를 추가하거나 변경할 땐 replace가 불가능하며 반드시 JVM을 재시작해야 한다:

- 클래스와 메서드의 선언부
- 클래스 변수 혹은 인스턴스 변수
- 스태틱 블록

replace가 불가능한 변경이 감지되면 아래와 같은 대화창이 나타나고 변경한 소스는 다음 재시작 때까지 반영되지 않는다.

![](/images/hot-code-replace-2.png)

replace에 실패했으니 무시하던지, JVM을 종료하던지, 아니면 재시작을 하라는 대화창이다. 무시할 경우 마지막으로 replace에 성공한 시점을 유지하지만 경우에 따라서 ClassNotFoundException 같은 예외가 발생할 수 있다.

대화창이 나타나는게 귀찮다면 옵션에서 아예 꺼버릴 수도 있다. 이클립스 mars 기준으로 `Preferences > Java > Debug > Hot Code Replace > show error when hot code replace fails.` 체크 박스를 해제하면 된다.

그리고 간혹 익명 클래스나 중첩 클래스의 내용을 변경할 때 hot code replace가 이상하게 작동하는 때가 있다. 이 때는 그냥 재시작 하면 된다. 하여간 안되면 재실행이 답.

플러그인 중 JRebel이 있는데, 재시작 없이 교체 가능한 범위의 제한이 없다. 하지만 유료.


## 원격 서버 디버깅

- [https://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Ftasks%2Ftask-remotejava_launch_config.htm](https://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Ftasks%2Ftask-remotejava_launch_config.htm)
- [https://dzone.com/articles/how-debug-remote-java-applicat](https://dzone.com/articles/how-debug-remote-java-applicat)
- [http://wiki.javajigi.net/pages/viewpage.action?pageId=743](http://wiki.javajigi.net/pages/viewpage.action?pageId=743)
- [https://stackoverflow.com/questions/3835612/remote-debugging-tomcat-with-eclipse](https://stackoverflow.com/questions/3835612/remote-debugging-tomcat-with-eclipse)
- [https://codebeamer.com/cb/wiki/794800](https://codebeamer.com/cb/wiki/794800)

그냥은 안되고 원격지에 설정을 따로 해줘야 한다.

일단 아래처럼 원격 서버의 VM 옵션을 추가하고:

```bash
-Xdebug -Xnoagent -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8000
```

이클립스 메뉴에서 `Run > Debug configurations > Remote Java Application`이 있는데:

![](/images/remote-java-app-config-1.png)

프로젝트는 원격지와 소스가 동일한 프로젝트로 선택해주고, 아이피는 원격지의 아이피, 포트는 VM 옵션의 포트로(이 예시의 경우 8000번) 설정한다.

요로코롬 하면 원격지에서 돌아가는 자바 애플리케이션을 디버깅할 수 있다.
끄는 방법은 Debug view에서 VM을 선택하고 끄면 됨.

JPDA(Java Platform Debugger Architecture) 관련 기술인것 같다. [오라클 문서 링크](https://docs.oracle.com/javase/8/docs/technotes/guides/jpda/index.html)


## Customize Perspective

### Menu Visibility

![](/images/eclipse-menu-visibility.png)  
요것이 메뉴.

### Toolbar Visibility

![](/images/eclipse-toolbar-visibility.png)  
요것이 툴바

- Action Set Availability
- ShortCuts
- 단축키는 뭐랑 관계 있어?


## Preferences

### General

- `Keep next/previous editor, view and perspectives dialog open`: 체크를 헤재하면 에디터, 뷰, perspective 전환 단축키를 때자마자 바로 전환한다.


## Project Properties

### Project References

- TODO: 이거 뭐... 다른 프로젝트를 컴파일 타임과 WST 서버 띄울때만 참조하는 거였던가...

### Web Project Settings

- Context root: 웹 프로젝트의 컨텍스트 루트 경로를 의미함. WST로 서버를 추가할 때 Context Path가 이 값으로 설정된다. 만약 메이븐 프로젝트라면 **pom.xml의 build > finalName 속성이 우선**권을 가지므로 프로젝트 속성에서 직접 수정하지 않도록 한다.


## 빌드

### Source

이클립스 빌더에게 소스 파일이 어느 경로에 있는지`Source folers on build path`와 컴파일한 파일을 어디로 내보낼지`Default output folder`를 알려주는 설정이다. 만약 소스 경로에 컴파일 할 수 없는(예: xml) 파일이 있으면 별도 작업 없이 그대로 내보내는걸로 추정된다.

### Projects

빌드에 필요한 클래스패스를 지정한다. 이클립스 프로젝트 단위로 추가할 수 있다. 추가된 프로젝트는 빌드 시 프로젝트명.jar로 묶인다.

### Libraries

빌드에 필요한 클래스패스를 지정한다. Jar, class 폴더를 설정한다.

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

이후 primary 프로젝트에서 includeMe 프로젝트의 jar를 import 할 수 있게된다. 단, 이것은 이클립스 내에서만 잘 돌아가는 설정이다. 만약 WAR로 빌드 후 별도의 WAS에서 구동하면 `Deployment Assembly`에서 includeMe를 추가해도 정상작동 하지 않는다.


## 빌드 - JST Server Adapter

보통은 아파치-톰캣으로 사용하는 서버 확장기능과 관련된 설정이다. 이 설정을 어떻게 하느냐에 따라 WAS를 위해 빌드하는 방식이 다르며, 메이븐/그래들 같은 빌드 툴을 사용하지 않은 환경에서는 아주 중요한 설정이다. 상당히 번거롭고 개발툴에 빌드 설정을 의존하기 때문에 가능하면 안쓰는게 좋다.

**WAS의 publish**: WAS의 publish란 WAS가 working directory(우리가 직접 보고 수정하는 작업 파일들)를 직접 읽지 않고 별도의 공간에 WAS 자신이 읽을 수 있는 모양의 형태로 배포하는것을 말한다. `eclipse-workspace\.metadata\.plugins\org.eclipse.wst.server.core\tmp0\wtpwebapps` 보통은 요딴 경로에 있으니 WAR로 빌드하기 전에 어떤 모양이 나오는지 미리 확인하거나 디버깅(특정 파일이 있는지 없는지) 하는 식으로 활용할 수 있다.

**배포**: WAR나 풀려있는 WAR로 내보내는 행위

### Projects, Libraries

여기에 추가된 항목은 빌드할 때 사용하긴 하지만 배포할 때도 그런건 아니다. 배포에 포함하고 싶다면 `Deployment Assembly` 설정에서 추가한다.

**...라고 정리해놨으나, 현장에서 `Deployment Assembly` 설정 없이 돌아가는 배포 환경을 목격함. 실제 배포 대상에 없는 파일을 JST 서버에서 읽는다.**

```xml
<Context docBase="backoffice" path="/" reloadable="false" source="org.eclipse.jst.jee.server:backoffice">
    <Resource auth="Container" driverClassName="oracle.jdbc.OracleDriver" ... />
    <Loader className="org.apache.catalina.loader.VirtualWebappLoader" virtualClasspath="C:/somewhere" />
</Context>
```

톰캣7 설정 중 `Loader-virtualClasspath` 속성으로 라이브러리 폴더의 전체 경로를 지정하는 방식이었다. 클래스패스를 WAS 설정에 하드코딩 하다니, 정말 듣도보도 못한 충격적인 발상이다. 톰캣 버전이 올라가면서 이 속성은 없어진것 같다.

### Deployment Assembly

Deployment Assembly는 JST 확장기능이 웹앱을 배포할 때에 필요한 소스 위치와 배포 위치를 지정하는 설정이다.

![](/images/eclipse-탐구생활-1.png)

`Source`는 말 그대로 소스 파일의 원래 경로를 의미하고 `Deploy Path`는 소스 파일이 어느 경로로 배포될 지를 의미한다.

예를 들어 `Projects` 탭에서 AAA라는 프로젝트를 추가하면 WAS는 이 프로젝트의 작업 파일을 직접 참조한다. 이렇게만 하면 일단 컴파일 자체는 문제가 없지만, WAR 빌드 시에 AAA 프로젝트를 포함하지 않기 때문에 (JST가 아닌) 이클립스와 분리된 WAS에서 구동 시 `ClassNotFoundException`이 발생한다. 따라서 Deployment Assembly에서 AAA 프로젝트를 추가해 빌드 시 같이 내보내도록 설정해야 한다.

다른 빌드 패스 설정도 마찬가지지만 Deployment Assembly 또한 빌드 툴(메이븐/그래들)에 의해서 자동으로 설정될 수 있다. 메이븐의 경우 dependency로 다른 프로젝트를 참조하도록 설정하면 `Deployment Assembly`와 `Libraries > Maven Dependencies`에 같이 추가된다.

### WAS의 Source

JST 서버별 `Overview > Edit Configuration > Source`는 직접 설정하진 않지만 디버깅용으로 확인하는 정도... 이 설정에 있는 경로나 jar가 정확히 어떤 용도인지는 아직 몲.

`Projects`와 `Libraries` 탭에서 추가한 항목은 자동으로 이 설정에도 추가된다.


## 리팩터 Refactor

### pull up

피확장 클래스(=부모)로 멤버 이동. abstract 메서드는 이동이 아니라 메서드 선언만 복사하며 overridden 메서드가 된다.

### pull down

확장 클래스(=자식)로 멤버 이동


## 파일 속성

### derived resource

파일이나 폴더 속성에서 `Derived` 속성에 체크하면 해당 파일은 '소스에서 파생된 파일'로 취급한다. 파생된 파일은 탐색이나 검색 등에서 자동으로 제외하는 옵션이 있다. 보통은 빌드 아웃풋 폴더(target, bin, build, ...)를 derived resource로 지정해서 컴파일된 클래스 파일이 검색 결과에 나타나지 않도록 설정한다.


## 이클립스의 환경 변수

### ${project_loc:some-project-name}

TODO


## 빌드툴 관련

### TODO
