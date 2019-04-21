---
layout: post
date: 2018-04-18 11:02:53 +0900
title: 'eclipse: hot code replace'
categories:
  - devtool
tags:
  - devtool
  - eclipse
  - hot deploy
  - hot code replace
---

#### 참고 문서
- [http://www.mkyong.com/eclipse/how-to-configure-hot-deploy-in-eclipse/](http://www.mkyong.com/eclipse/how-to-configure-hot-deploy-in-eclipse/)

이클립스에선 JVM을 디버그 모드로 구동한 후, 런타임이 종료되지 않은 상태에서(WAS를 띄워놓은 것과 같은 상태) 클래스 파일의 변경이 감지되면 JVM 재시작 없이 변경된 클래스파일을 교체하는 hot code replace 기능을 제공한다. (hot deploy 또는 hot swap 이라고도 한다.) 유료 플러그인인 JRebel과 비교하면 초라하지만... 없는 것보단 낫다.

## 설정
여기선 톰캣 플러그인을 예로 든다. 우선 톰캣 서버 설정(서버 이름을 더블클릭 해서 진입한다. 서버 목록은 이클립스에서 Servers view를 활성화해야 볼 수 있다.)의 Overview 탭에서 'Automatically publish when resources change' 항목에 체크하고 (만약 'Serve modules without publishing'을 활성화했다면 이 과정은 생략해도 된다):

![](/images/hot-code-replace-1.png)

Modules 탭에서 보이는 Edit 버튼을 눌러 Auto reloading enabled의 체크를 해제한다.
이후 WAS를 디버그 모드로 구동한 뒤 소스를 수정하면 된다.

## 주의사항
hot code replace의 적용 범위는 메서드 본문에만 한정된다. 다음에 해당하는 코드를 추가하거나 변경할 땐 replace가 불가능하며 반드시 JVM을 재시작해야 한다:
- 클래스와 메서드의 선언부
- 클래스 변수 혹은 인스턴스 변수
- 스태틱 블록

replace가 불가능한 변경이 감지되면 아래와 같은 대화창이 나타나고 변경한 소스는 다음 재시작 때까지 반영되지 않는다.

![](/images/hot-code-replace-2.png)

replace에 실패했으니 무시하던지, JVM을 종료하던지, 아니면 재시작을 하라는 대화창이다. 무시할 경우 마지막으로 replace에 성공한 시점을 유지하지만 경우에 따라서 ClassNotFoundException 같은 예외가 발생할 수 있다.

대화창이 나타나는게 귀찮다면 옵션에서 아예 꺼버릴 수도 있다. 이클립스 mars 기준으로 `Preferences` > `Java` > `Debug` > `Hot Code Replace` > `show error when hot code replace fails.` 체크박스를 해제하면 된다.

그리고 간혹 익명 클래스나 중첩 클래스의 내용을 변경할 때 hot code replace가 이상하게 작동하는 때가 있다. 이 때는 그냥 재시작 하면 된다. ~~하여간안되면껏다켜는게진리~~
