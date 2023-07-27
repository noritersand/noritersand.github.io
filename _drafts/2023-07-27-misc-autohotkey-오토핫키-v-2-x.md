---
layout: post
date: 2023-07-27 13:13:00 +0900
title: '[misc] AutoHotkey v2.x'
categories:
  - misc
tags:
  - misc
  - autohotkey
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://www.autohotkey.com/](https://www.autohotkey.com/)
- [https://www.autohotkey.com/docs/AutoHotkey.htm](https://www.autohotkey.com/docs/AutoHotkey.htm)
- [https://www.autohotkey.com/docs/v2/](https://www.autohotkey.com/docs/v2/)

#### 버전 정보

- AutoHotkey v2.x


## 개요

AutoHotkey는 스크립트로 작성하는 키보드&마우스 매크로 애플리케이션이다. 특정 키를 눌렀을 때 기존과 다른 키가 입력되게 하는 간단한 키매핑부터, 일련의 키 입력을 반복하는 매크로, 시간차를 둔 입력 등 상상할 수 있는 거의 모든 것을 만들 수 있다.

스크립트의 기본 확장자는 `ahk`이며 앱 설치 후 스크립트를 실행하면 해당 프로그램이 백그라운드에서 키 입력을 감지한다. (트레이에 아이콘 있을 유)

누군가에 말에 의하면 AutoHotkey는 C++으로 개발된 앱이지만, 여기서 사용하는 스크립트는 완전히 독자적인 언어이며 C++의 문법과 다르다고 한다.

서브라임 텍스트에선 Java syntax를 쓰는게 가장 보기 좋다. 그런데 완전하지 않으므로 [AutoHotkey](https://packagecontrol.io/packages/AutoHotkey) 패키지를 설치하자. VSCODE에서도 누군가 이미 확장 기능을 만들어놨다(이쪽이 더 이쁘다).


## 2.x에서 달라진 점

[https://www.autohotkey.com/docs/v2/v2-changes.htm](https://www.autohotkey.com/docs/v2/v2-changes.htm)

### 함수 이름과 파라미터 사이의 콤마`,` 사라짐

`Send, 1`로 작성하면 안되고 `Send 1`로 작성해야 함


## snippets

### 스크립트 중단/재개, 종료, 다시 불러오기

```autohotkey
/*
## script control
*/
+^!F5::Pause -1 ; Pause script
+^!F6::Reload   ; Reload script
+^!F8::ExitApp  ; Exit script
```

2.x에선 `Suspend` 대신 `Pause`를 사용해야 하고(Suspend 상태가 되면 단축키로 다시 Unsuspend 하는 게 안됨), 파라미터가 있어야 한다.

### 함수를 선언하고 일정 시간마다 호출하기

```autohotkey
Persistent True

SetTimer SendNumber1, 1100 ; 1.1초마다 SendNumber1() 함수 실행
SetTimer SendNumber2, 2200 ; 2.2초마다 SendNumber2() 함수 실행

SendNumber1() {
    Send 1 ; 숫자 1 입력
}

SendNumber2() {
    Send 2 ; 숫자 2 입력
}
```