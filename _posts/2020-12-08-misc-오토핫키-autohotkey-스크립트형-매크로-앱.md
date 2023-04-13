---
layout: post
date: 2020-12-08 14:55:00 +0900
title: '[misc] 오토핫키 AutoHotkey 스크립트형 매크로 앱'
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

#### 버전 정보

- AutoHotkey_1.1.3x


## 개요

AutoHotkey는 스크립트로 작성하는 키보드&마우스 매크로 애플리케이션이다. 특정 키를 눌렀을 때 기존과 다른 키가 입력되게 하는 간단한 키매핑부터, 일련의 키 입력을 반복하는 매크로, 시간차를 둔 입력 등 상상할 수 있는 거의 모든 것을 만들 수 있다.

스크립트의 기본 확장자는 `ahk`이며 앱 설치 후 스크립트를 실행하면 해당 프로그램이 백그라운드에서 키 입력을 감지한다. (트레이에 아이콘 있을 유)

누군가에 말에 의하면 AutoHotkey는 C++으로 개발된 앱이지만, 여기서 사용하는 스크립트는 완전히 독자적인 언어이며 C++의 문법과 다르다고 한다.

서브라임 텍스트에선 Java syntax를 쓰는게 가장 보기 좋다. 그런데 완전하지 않으므로 [AutoHotkey](https://packagecontrol.io/packages/AutoHotkey) 패키지를 설치하자. VSCODE에서도 누군가 이미 확장 기능을 만들어놨다(이쪽이 더 이쁘다).


## 스크립트 작성 방법

앱이 설치되어 있다면 스크립트에 기본적으로 필요한 것은 아무것도 없다. 바로 원하는 내용을 쓰면 된다.

예시:

```autohotkey
/*
Simple keymapping script
*/
a::b ; a를 누르면 b가 입력됨
```

`::`의 좌측에 트리거 키, 바로 다음 줄에 실행할 명령을 작성한다:

```autohotkey
#n::
Run Notepad
return
```

만약 `::`의 바로 우측 즉, 같은 라인에 명령을 작성할 경우 `return`이 라인 끝에 있는걸로 간주한다.

```autohotkey
#n::Run Notepad
```

### 코멘트

```autohotkey
; 세미콜론 뒤에 오는 문자는 코멘트 처리됨
```

```autohotkey
/*
요것은 코멘트 블록
*/
```

코멘트 블록이 연속으로 반복되면 스크립트 실행이 안되는데 될 때도 있는데, 어떤 조건에 그러는지는 몲... ~~쉬불쟝~~

### 여러 키에 같은 명령 할당

```autohotkey
^Numpad0::
^Numpad1::
MsgBox Pressing either Control+Numpad0 or Control+Numpad1 will display this message.
return
```

위와 같은 방식으로 트리거 키를 연속으로 작성하면 둘 이상의 키에 같은 명령을 할당할 수 있다.

### 키 비활성화

```autohotkey
RWin::return
```

`::`다음 바로 `return`을 작성하면 해당 키는 비활성화된다. 위의 경우 오른쪽 윈도우키를 비활성화한다.

### Hotstrings

Hotstrings는 일련의 연속적인 키 입력을 트리거로 사용하는 것을 말한다.

```autohotkey
::btw::by the way
```

가령 위의 스크립트는 <kbd>b</kbd><kbd>t</kbd><kbd>w</kbd>를 입력하고 스페이스나 엔터, 탭을 입력하면 'btw'가 'by the way'로 치환된다.

```autohotkey
:*:btw::by the way
```

이 경우엔 스페이스/엔터/탭 없이 바로 치환된다.

단순히 문자열 치환만 가능한 건 아니다. 아래를 보자:

```autohotkey
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

`kbd` 태그 입력 후 커서를 왼쪽으로 여섯 번 이동하는 스크립트다.

#### \#Hotstring

Hotstrings의 옵션을 설정하는 명령어.

```autohotkey
#Hotstring r c ; 이 아래에 작성된 Hotstrings의 트리거는 대소문자를 구별한다.
#Hotstring c0  ; 이 아래에 작성된 Hotstrings의 트리거는 대소문자를 무시한다.
```

### 엔터와 탭

엔터는 ``` `n```, 탭은 ``` `t```로 작성한다.

```autohotkey
:*:qwer::hel`nlo`tworld{!}
```

결과:

```
hel
lo    world!
```

여기서 `!`를 `{}`로 감싼 이유는 `!`가 <kbd>alt</kbd>의 prefix이기 때문이다.

### {}

`{}` 문자는 키 이름과 기타 옵션을 묶고 특수 문자를 문자 그대로 보내는데 사용한다. ~~뭔소리야~~ 가령, `{Tab}`은 <kbd>Tab</kbd>키이고, `{!}`는 문자 그대로 느낌표다.  
따라서 <kbd>alt + tab</kbd>을 내보내고 싶을땐 `!{Tab}`라고 작성한다.

TODO 특수 문자를 단축키로 입력받게 하려면 어떻게 해야 하는지:

```autohotkey
; + 입력받기
{+}::Send You pressed the plus key!  ; Bing은 이렇게 하라는데 에러 뜸
+::Send You pressed the plus key!    ; 그냥 이렇게 하면 됨
```

이렇게 하는 방법도 있는데 원리는 모름:

```autohotkey
; 특이하게도 win + = 조합으로 발동된다
LWin & +::Run "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"
```


### 단순 매핑

```autohotkey
`::Numpad0
i::SendInput {WheelDown}
```

`i::WheelDown`이라 작성해도 되나, 이렇게 하면 <kbd>i</kbd>의 다운과 업 이벤트에 반응을 해서 휠이 두 번 굴러간다.

### 연속적인 키 입력

```autohotkey
F9::SendInput +{a}+{b}+{c} ; 아래와 결과 같음
F10::SendInput {LShift down}{a}{b}{c}{LShift Up} ; 위와 같음
F11::SendInput Fork you
```


## 연산자/표현식

### := 할당 연산자

```
Var := expression
```

`Var` 변수(없으면 만듦)에 `expression`의 평가값을 할당한다.

참고로 여기서 `=`는 할당이 아니고 동등비교임.


## 제어문

### Loop

```
Loop [, Count]
```

`Count`만큼 바디(블록안의 스크립트 혹은 바로 뒤따르는 한 줄의 스크립트)를 반복한다.

#### 예시

숫자패드의 <kbd>-</kbd>를 누르면 마우스 휠 업 14번 반복 입력하는 스크립트:

```autohotkey
NumpadSub::
Loop, 14 {
  SendInput {WheelUp}
}
return
```

### While

```
While Expression
While (Expression)
```

`Expression`이 `True`로 평가되는 값이면 바디를 반복한다.

#### 예시

마우스 좌버튼이 눌려져 있으면 클릭을 반복하게 하는 스크립트:

```autohotkey
SetMouseDelay 30
LButton::
while (GetKeyState("LButton", "P")) {
  Click
}
return
```

### If-Else

```autohotkey
If expression {
  do something
} Else {
  do something else
}
```

일반적인 프로그래밍 언어들과 큰 차이는 없다. 

### \#If

문맥에 따른(context-sensitive) 단축키와 핫스트링을 만들 때 사용한다. 자매품으로 `\#IfTimeout`, `\#IfWinActive`, `\#IfWinNotExist`, `\#IfWinExist`, `\#IfWinNotActive`가 있다. 이런 걸 '지시어'라고 부르는 모양.

```autohotkey
#If True
  q::Send You pressed 'q' key.

#If False
  z::Send You pressed 'z' key. ; 이 단축키는 영원히 작동하지 않음
```

한 번 `#If` 문맥이 시작되면 그 아래에 작성한 스크립트는 모두 영향을 받는다. 문맥을 끊으려면(To turn off context sensitivity) 파라미터를 생략한 `#If`를 작성하면 된다(`#End`는 없음):

```autohotkey
s::d

#If False
  z::Send You pressed 'z' key. ; 작동하지 않음

  q::a ; 얘도 작동하지 않음

#If
  c::v ; 여기부턴 잘 됨
```


## 함수

### Send, SendRaw, SendInput, SendPlay, SendEvent

TODO

### GetKeyState()

```
KeyIsDown := GetKeyState(KeyName , Mode)
```

`KeyName`에 해당하는 키가 눌려져 있거나 토글상태인지 확인하는 함수. 키가 눌려져 있으면 `1`을 반환한다.  

`Mode`는 `P`혹은 `T`, 아니면 생략한다. `P`는 press, `T`는 토글을 의미하며, 생략할 경우:

> If omitted, the mode will default to that which retrieves the logical state of the key. This is the state that the OS and the active window believe the key to be in, but is not necessarily the same as the physical state.

라고 한다.


## 키 이름

[더 많은 키 목록](https://www.autohotkey.com/docs/KeyList.htm)

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

```autohotkey
/*
## script control
*/
+^!F5::Suspend ; Suspend script
+^!F6::Reload  ; Reload script
+^!F8::ExitApp ; Exit script
```

자주 누르는 일반적인 키(1, 2, a, b, 등등)에 매핑을 할당할 경우 이 스크립트를 항상 포함하는게 편하다.

### 키 히스토리

```autohotkey
#Persistent       ; keep running
#InstallKeybdHook ; Better for keys
KeyHistory

ESC::return
```

내가 방금 뭘 눌렀는지 이력으로 보여주는 창을 띄운다. 막 줄은 <kbd>ESC</kbd>로 종료되는 것을 방지한다.

### Office App 단축키 막기

출처: [https://superuser.com/questions/1457073/how-do-i-disable-specific-windows-10-office-keyboard-shortcut-ctrlshiftwinal](https://superuser.com/questions/1457073/how-do-i-disable-specific-windows-10-office-keyboard-shortcut-ctrlshiftwinal)

```autohotkey
#^!Shift::
#^+Alt::
#!+Ctrl::
^!+LWin::
^!+RWin::
Send {Blind}{vk07}
return
```

<kbd>ctrl + alt + shift + win</kbd> 키로 발동하는 MS Office 관련 단축키 비활성화 코드. 단점은 저 네 개를 한 번에 누르는 modifier 키 조합을 아예 못쓴다는 것.

### 현재 날짜와 시간

```autohotkey
:*:][wlrma::
FormatTime, CurrentDateTime,, yyyy-MM-dd hh:mm:ss
SendInput %CurrentDateTime%
return
```

`][지금`으로 발동한다.

### 빠른 연타 rapid fire

마우스 좌클릭:

```autohotkey
SetMouseDelay 30
LButton::
while GetKeyState("LButton", "P") {
  Click
}
return
```

<kbd>ctrl + g</kbd> 누르고 있으면 g 반복 입력:

```autohotkey
<^g::
While (GetKeyState("g", "P") && GetKeyState("LCtrl", "P")) {
  SendInput g
  Sleep 10
}
return
```

### 특정 위치로 커서 이동 + 클릭

```autohotkey
[::
MouseClick, Left, 232, 202
return
```

<kbd>[</kbd>를 누르면 지정한 좌표로 이동하여 좌클릭하는 스크립트. `MouseClick, Left`라고만 쓰면 현재 커서 위치에서 좌클릭한다.  
좌표는 WindowSpy(AutoHotkey 설치 시 같이 깔림) 앱에서 확인하면 되며, [CoordMode](https://www.autohotkey.com/docs/commands/CoordMode.htm)를 사용하지 않는 이상 절대위치가 아닌 앱 별 상대위치로 작동한다.

### 앱 실행/전환 단축키 확장

```autohotkey
#SingleInstance force
SetTitleMatchMode 2

#IfWinExist ahk_exe Notion.exe
  <#-::WinActivate
#IfWinNotExist ahk_exe Notion.exe
  <#-::Run "C:\Users\USER_NAME\AppData\Local\Programs\Notion\Notion.exe"

#IfWinExist ahk_exe msedge.exe
  <#=::WinActivate
#IfWinNotExist ahk_exe msedge.exe
  <#=::Run "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"
#If ; #End가 없어서 대신 쓰는 종료 키워드
```

`If-Else` 버전으로 작성하면 이렇게 된다:

```autohotkey
#SingleInstance force
SetTitleMatchMode 2

<#-::
If WinExist("ahk_exe Notion.exe") {
  WinActivate
} Else {
  Run "C:\Users\fixal\AppData\Local\Programs\Notion\Notion.exe"
}
return

<#=::
If WinExist("ahk_exe msedge.exe") {
  WinActivate
} Else {
  Run "C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe"
}
return
```

여기서 `#SingleInstance force`는 스크립트가 이미 실행 중일 때 이전 인스턴스를 종료하고 새 인스턴스를 시작하게 해서 중복 실행을 방지하는 설정이다.

`SetTitleMatchMode 2`는 윈도우 제목을 검색할 때 `WinTitle` 파라미터에 지정된 문자열을 일부만 포함해도 일치하다고 간주하게 하는 설정이다.



## 꼐속...

