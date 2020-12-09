---
layout: post
date: 2020-12-08 14:55:00 +0900
title: '[misc] AutoHotkey 스크립트형 매크로 앱'
categories:
  - misc
tags:
  - autohotkey
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://www.autohotkey.com/](https://www.autohotkey.com/)
- [https://www.autohotkey.com/docs/AutoHotkey.htm](https://www.autohotkey.com/docs/AutoHotkey.htm)

AutoHotkey는 스크립트로 작성하는 키보드&마우스 매크로 애플리케이션이다. 특정 키를 눌렀을 때 기존과 다른 키가 입력되게 하는 간단한 키매핑부터, 일련의 키 입력을 반복하는 매크로, 시간차를 둔 입력 등 상상할 수 있는 거의 모든 것을 프로그램 할 수 있다.

스크립트의 기본 확장자는 `ahk`이며 앱 설치 후 스크립트를 실행하면 해당 프로그램이 백그라운드에서 키 입력을 감지한다. (트레이에 아이콘 있을 유)

누군가에 말에 의하면 AutoHotkey는 C++으로 개발된 앱이다. 하지만 여기서 사용하는 스크립트는 완전히 독자적인 언어이므로 C++의 문법을 적용할 순 없다.  

서브라임 텍스트에선 Java syntax를 쓰는게 가장 보기 좋다.

## 스크립트 작성 방법

앱이 설치되어 있다면 스크립트에 기본적으로 필요한 것은 아무것도 없다. 바로 원하는 내용을 쓰면 된다.

예시:

```c
/*
  Simple keymapping script
*/
a::b ; a를 누르면 b가 입력됨
```

### 코멘트

```c
; this is comment line```

```c
/*
this is comment block
*/
```

코멘트 블록이 연속으로 반복되면 스크립트 실행이 안되는데 될 때도 있는데, 어떤 조건에 그러는지는 몲... ~~쉬불쟝~~

### 단순 매핑

```c
`::Numpad0 ;
i::SendInput {WheelDown} ;
```

`i::WheelDown`이라 작성해도 되나, 이렇게 하면 <kbd>i</kbd>의 다운과 업 이벤트에 반응을 해서 휠이 두 번 굴러간다.

### 반복문

```c
NumpadSub::
Loop 14
	SendInput {WheelUp} ;
Return
```

숫자패드의 <kbd>-</kbd>를 누르면 마우스 휠 업 14번 반복 입력

## snippets

### 스크립트 중단/재개, 종료, 다시 불러오기

```c
/*
## script control
*/
+^!F5::Reload   ; Reload script
+^!F8::Suspend  ; Suspend script
+^!F4::ExitApp  ; Exit script
```

자주 누르는 일반적인 키(1, 2, a, b, 등등)에 매핑을 할당할 경우 이 스크립트를 항상 포함하는게 편하다.

## 꼐속...
