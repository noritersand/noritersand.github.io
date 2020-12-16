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

누군가에 말에 의하면 AutoHotkey는 C++으로 개발된 앱이다. 하지만 여기서 사용하는 스크립트는 완전히 독자적인 언어이며 C++의 문법과 다르다.

서브라임 텍스트에선 Java syntax를 쓰는게 가장 보기 좋다. 그런데 완전하지 않으므로 [AutoHotkey](https://packagecontrol.io/packages/AutoHotkey) 패키지를 설치하자. VSCODE에서도 누군가 이미 확장 기능을 만들어놨다(이쪽이 더 이쁘다).

## 스크립트 작성 방법

앱이 설치되어 있다면 스크립트에 기본적으로 필요한 것은 아무것도 없다. 바로 원하는 내용을 쓰면 된다.

예시:

```c
/*
Simple keymapping script
*/
a::b ; a를 누르면 b가 입력됨
```

`::`의 좌측에 트리거 키, 바로 다음 줄에 실행할 명령을 작성한다:

```c
#n::
Run Notepad
return
```

만약 `::`의 바로 우측, 즉 같은 라인에 명령을 작성할 경우 `return`이 라인 끝에 있는걸로 간주한다.

```c
#n::Run Notepad
```

### 코멘트

```c
; 세미콜론 뒤에 오는 문자는 코멘트 처리됨
```

```c
/*
요것은 코멘트 블럭
*/
```

코멘트 블록이 연속으로 반복되면 스크립트 실행이 안되는데 될 때도 있는데, 어떤 조건에 그러는지는 몲... ~~쉬불쟝~~

### 여러 키에 같은 명령 할당

```c
^Numpad0::
^Numpad1::
MsgBox Pressing either Control+Numpad0 or Control+Numpad1 will display this message.
return
```

위와 같은 방식으로 트리거 키를 연속으로 작성하면 둘 이상의 키에 같은 명령을 할당할 수 있다.

### 키 비활성화

```c
RWin::return
```

`::`다음 바로 `return`을 작성하면 해당 키는 비활성화된다. 위의 경우 오른쪽 윈도우키를 비활성화한다.

### Hotstrings

Hotstrings는 일련의 연속적인 키 입력을 트리거로 사용하는 것을 말한다.

```c
::btw::by the way
```

가령 위의 스크립트는 <kbd>b</kbd><kbd>t</kbd><kbd>w</kbd>를 입력하고 스페이스나 엔터, 탭을 입력하면 'btw'가 'by the way'로 치환된다.

```c
:*:btw::by the way
```

이 경우엔 스페이스/엔터/탭 없이 바로 치환된다.

단순히 문자열 치환만 가능한 건 아니다. 아래를 보자:

```c
/*
## <kbd></kbd>
*/
:*:,kbd.::
SendInput <kbd></kbd>
Loop, 6 {
	SendInput {Left}
}
return
```

<kbd>kbd</kbd> 태그 입력 후 커서 이동하는 스크립트다.

#### \#Hotstring

Hotstrings의 옵션을 설정하는 명령어.

```c
#Hotstring r c ; 이 아래에 작성된 Hotstrings의 트리거는 대소문자를 구별한다.
#Hotstring c0 ; 이 아래에 작성된 Hotstrings의 트리거는 대소문자를 무시한다.
```

### 엔터와 탭

엔터는 <code>\`n</code>, 탭은 <code>\`t</code>로 작성한다.

```c
:*:qwer::hel`nlo`tworld{!}
```

결과:

```
hel
lo	world!
```

여기서 `!`를 `{}`로 감싼 이유는 `!`가 <kbd>alt</kbd>의 prefix이기 때문이다.

### 단순 매핑

```c
`::Numpad0
i::SendInput {WheelDown}
```

`i::WheelDown`이라 작성해도 되나, 이렇게 하면 <kbd>i</kbd>의 다운과 업 이벤트에 반응을 해서 휠이 두 번 굴러간다.

### 연속적인 키 입력

```c
F9::SendInput {LShift down}{a}{b}{c}{LShift Up}
F10::SendInput Hello mother forker
```

### 반복문

```c
NumpadSub::
Loop, 14 {
	SendInput {WheelUp}
}
return
```

숫자패드의 <kbd>-</kbd>를 누르면 마우스 휠 업 14번 반복 입력

## 주요 키 목록

### Modifier Keys

- `Win`: 윈도우키, prefix는 `#`
- `Shift`: 시프트, prefix는 `+`
- `Control|Ctrl`: 컨트롤, prefix는 `^`
- `Alt`: 알트, prefix는 `!`
- `LWin`: 왼쪽 윈도우키, prefix는 `<#`
- `RWin`: 오른쪽 윈도우키, prefix는 `>#`
- `LShift`: 왼쪽 시프트, prefix는 `<+`
- `RShift`: 오른쪽 시프트, prefix는 `>+`
- `LControl|LCtrl`: 왼쪽 컨트롤, prefix는 `<^`
- `RControl|RCtrl`: 오른쪽 컨트롤, prefix는 `>^`
- `LAlt`: 왼쪽 알트, prefix는 `<!`
- `RAlt`: 오른쪽 알트, prefix는 `>!`

prefix는 `{}` 안에서 작동하지 않는다.

### Cursor Control Keys

- `ScrollLock`: <kbd>ScrollLock</kbd>
- `Delete|Del`: <kbd>Delete</kbd>
- `Insert|Ins`: <kbd>Insert</kbd>
- `Home`: <kbd>Home</kbd>
- `End`: <kbd>End</kbd>
- `PgUp`: <kbd>PageUp</kbd>
- `PgDn`: <kbd>PageDown</kbd>
- `Up`: <kbd>↑</kbd> (up arrow key)
- `Down`: <kbd>↓</kbd> (down arrow key)
- `Left`: <kbd>←</kbd> (left arrow key)
- `Right`: <kbd>→</kbd> (right arrow key)

## snippets

### 스크립트 중단/재개, 종료, 다시 불러오기

```c
/*
## script control
*/
+^!F5::Suspend  ; Suspend script
+^!F6::Reload   ; Reload script
+^!F8::ExitApp  ; Exit script
```

자주 누르는 일반적인 키(1, 2, a, b, 등등)에 매핑을 할당할 경우 이 스크립트를 항상 포함하는게 편하다.

### 키 히스토리

```c
#Persistent ; keep running
#InstallKeybdHook ; Better for keys
KeyHistory

ESC::return
```

내가 방금 뭘 눌렀는지 이력으로 보여주는 창을 띄운다. 막 줄은 <kbd>ESC</kbd>로 종료되는 것을 방지한다.

### 현재 날짜와 시간

```c
:*:][wlrma::
FormatTime, CurrentDateTime,, yyyy-M-d hh:mm:ss
SendInput %CurrentDateTime%
return
```

`][지금`으로 발동한다.

## 꼐속...
