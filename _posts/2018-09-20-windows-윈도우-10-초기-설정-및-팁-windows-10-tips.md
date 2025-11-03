---
layout: post
date: 2018-09-20 10:54:00 +0900
title: '[Windows] 윈도우 10 초기 설정 및 팁'
categories:
  - windows
tags:
  - os
  - windows
  - environment
  - setup
  - shortcut
  - hotkey
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [Windows의 바로 가기 키](https://support.microsoft.com/ko-kr/help/12445/windows-keyboard-shortcuts)


## 개요

요즘 들어 혜자로워진 마소의 윈도우 10 초기 설정을 정리한 글.


## Windows 터미널

- [Windows 터미널 개요](https://docs.microsoft.com/ko-kr/windows/terminal/)
- [Windows 터미널 설치](https://docs.microsoft.com/ko-kr/windows/terminal/get-started)

2020년인가... 새로 나온 윈도우용 터미널. 앱 하나에서 윈도우의 각종 셸(CMD, 파워셸, 파워셸 레거시, Azure Cloud Shell, WSL 등)을 동시에 사용할 수 있고, 창 쪼개기 기능(이게 세션도 분리되는건지는 아직 몲)을 지원함.

[이 링크](https://www.microsoft.com/ko-kr/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)에서 설치하거나, 스토어에서 'Windows Terminal'을 검색하면 나온다.

![](/images/windows-terminal.png)

~~이거 쓰고 있으면 좀 해커 같아 보임~~

### 시작 위치 변경

**윈도우 터미널 버전이 올라가면서 GUI 설정으로도 변경할 수 있게 되었음.**

터미널의 시작 위치를 변경하려면 설정 파일 `settings.json`을 아래처럼 수정한다. 해당 파일은 터미널 앱의 설정 화면(<kbd>ctrl + ,</kbd>)에서 좌측 하단 `Json 파일 열기`를 누르면 열림:

```js
"profiles": {
  "defaults": {},
  "list": [
    {
      "guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
      "hidden": false,
      "name": "PowerShell",
      "source": "Windows.Terminal.PowershellCore",
      "startingDirectory": "C:/dev"
    }
    ... 생략
  ]
}
```

이런식으로 `startingDirectory`를 추가하면 된다.

참고로 이 설정 파일에서 `list` 배열 안에 있는 객체들의 순서가 바로:

![](/images/windows-terminal-new-tabs.png)

터미널에서 새 탭을 열 때 선택할 수 있는 뇨솤들의 순서다.

### 변경된 환경 변수를 적용하려면

새 탭이나 새 창을 열어도 갱신되지 않으니 터미널 앱을 재실행할 것. 

스크립트를 수정한 거라면 Dot sourcing operator`.`로 갱신 가능함.


## Winget, Windows Package Manager Client

[https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli)

Windows OS의 패키지 관리용 공식 CLI 툴. 리눅스의 `apt`와 비슷하다. 보통은 기본으로 설치되어 있음.

```bash
# 기본 도움말 보기
winget

# list 명의 도움말 보기
winget list --help

# KEYWORD로 패키지 검색
winget search KEYWORD

# PACKAGE_NAME 설치
winget install PACKAGE_NAME

# PACKAGE_NAME 제거
winget uninstall PACKAGE_NAME

# PACKAGE_NAME 패키지의 상세정보 표시. 로컬에 설치된 패키지가 아니라 패키지 저장소에 있는 최신 정보를 보여줌
winget show PACKAGE_NAME

# 설치된 패키지 목록을 출력. 버전 업그레이드가 가능한지도 표시됨
winget list

# PACKAGE_NAME 패키지 버전 업그레이드
winget upgrade PACKAGE_NAME
```

이 외에 이런 명령들이 있음:

- `source`: 패키지 원본 관리. 원본이란 패키지 저장소를 의미한다. 즉, 패키지 저장소를 관리하는 명령어
- `hash`: 해시 설치 관리자 파일 도우미
- `validate`: 매니페스트 파일의 유효성 검사
- `settings`: 설정 열기 또는 관리자 설정 설정
- `features`: 실험적 기능의 상태 표시
- `export`: 설치된 패키지 목록 내보내기
- `import`: 파일에 있는 모든 패키지를 설치합니다.


## 초기 설정

### 날짜와 시간 형식 변경

`설정 > 시간 및 언어 > 언어 및 지역 > 사용지역 언어`에서

- 시작 요일: 월요일로 변경
- 간단한/자세한 시간: 24시간제로 변경

### 클립보드 기록 저장

`설정 > 시스템 > 클립보드`에서 '클립보드 검색 기록' 켬.

### 이모지 패널

~~설정 > 장치 > 입력 > 고급 키보드 설정에서 '이모티콘을 입력한 후 자동으로 패널을 닫지 않음' 체크.~~

이 설정 어디갔는지 모르겠네... 🙄

### 바로 가기 키 끄기

`설정 > 접근성 키보드 설정`에서 시프트 연타 등의 바로 가기 키 설정 끄기.

### 텔넷 활성화

셸(관리자 권한)에서 아래 실행:

```bash
# PS C:\> pkgmgr /iu:"TelnetClient" # pkgmgr.exe는 deprecated 되었음.
dism /online /Enable-Feature /FeatureName:TelnetClient

# localhost:4000 텔넷 접속
telnet localhost 4000
```

### 로캘(로케일) 범주(Locale Categories) 변경

> 로캘 범주는 지역화 루틴에서 사용할 프로그램 로캘 정보 부분을 지정하는 데 사용하는 매니페스트 상수입니다.

~~뭔소리야~~ 잘 모르겠지만 프로그램에서 지역을 알기 위해 참조하는 환경 변수로 보인다.

어쨋든 파워셸을 관리자 권한으로 열고 아래 명령을 실행한다:

```bash
[System.Environment]::SetEnvironmentVariable('LC_ALL', 'ko_KR.UTF-8', 'Machine')
```

`LC_ALL`은 모든 로케일 관련 범주를 의미하는 환경 변수이며, 몇몇 앱에서는 이 설정이 유효하지 않을 수 있다. (git 히스토리의 한글 깨짐 해결방법으로 검색이 많이 되던데, 정작 gitk에선 효과가 없음)

참고:

- https://docs.microsoft.com/ko-kr/cpp/c-runtime-library/reference/setlocale-wsetlocale?view=msvc-160
- https://docs.microsoft.com/ko-kr/cpp/c-runtime-library/locale-categories?view=msvc-160
- https://docs.microsoft.com/ko-kr/cpp/c-runtime-library/locale-names-languages-and-country-region-strings?view=msvc-160

### 260자 경로 제한 풀기

레지스트리 편집기 > `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem > LongPathsEnabled`의 값을 1로 변경


## 최적화 설정

말이 최적화 설정이지 하드웨어만 좋으면 이런거 안해도 된다.

### 작업 기록 끄기

야동, 야짤을 누군가에게 들키기 싫으면:

`설정 > 개인 정보 > Windows 사용 권한 > 작업 기록`에서 '이 장치에서 내 활동 기록 저장'과 'Microsoft에 내 활동 기록 보내기'를 끈다.

### 업데이트용 임시 파일 보관하지 않기

`설정 > 업데이트 및 보안 > 배달 최적화`에서 '다른 PC에서 다운로드 허용'을 끔.

### 자동 프록시 끄기

`설정 > 네트워크 및 인터넷 > 프록시`에서 '자동으로 설정 검색'을 끔. 이거 끄면 브라우저 페이지 이동 속도 빨라진다는데, 정확히 무슨 기능인지 연구 필요함.

### 피드백 끄기

`설정 > 개인 정보 > 피드백 및 진단`에서 피드백, 진단 등의 기능을 끔.

### 백그라운드 앱 끄기

`설정 > 개인 정보 > 백그라운드 앱`에서 앱 찾아서 끄기.


## 기타 팁

### ~~파일 탐색기에서 파일 목록에 포커싱하기~~

~~파일 탐색기 열자 마자 <kbd>space</kbd> 누르면 파일 선택 모드로 바로 진입할 수 있음.~~

### 구버전 제어판 열기

윈도우 검색(<kbd>win + s</kbd>)에서 '제어판'으로 검색하면 나온다. 만약 검색이 안 되면 실행 대화 상자(<kbd>win + r</kbd>)에서 `control` 혹은 `Control Panel` 입력.

### 파일 탐색기(File Explorer) 아이콘 오버레이 우선순위 설정

![](/images/icon-overlay-order.png)

`컴퓨터\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers`

위 경로의 레지스트리를 수정한다.
공백이 많은 순으로 우선정렬 되니까 원하는 아이콘이 위로 올라오도록 공백의 개수를 조절하면 됨.

### 앱으로 인식되지 않는 파일을 시작 화면에 등록하기

`C:\ProgramData\Microsoft\Windows\Start Menu` 여기에 바로가기를 붙여넣고 검색(<kbd>win + s</kbd>)하면 앱으로 뜸.

`%APPDATA%\Microsoft\Windows\Start Menu\Programs` 여기에 놔도 된다. 이 경로는 `shell:programs`으로 접근할 수 있다.

### 시작 프로그램 등록

`%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup` 경로에 실행파일 바로가기 복붙하면 끟. 이 경로의 숏컷은 `shell:startup`.

### 바로가기의 시작 위치 변경

바로 가기의 속성에서 `시작 위치`를 원하는 곳(예: `C:\dev\git`)으로 변경하면 된다. 만약 `대상`의 값에 `--cd-to-home` 같은 옵션이 있으면 지워야 제대로 작동함.

### 스타워즈 episode 4

텔넷 활성화 후 터미널에서 아래 입력:

```bash
telnet towel.blinkenlights.nl
```

### 할리우드

리눅스에는 hollywood라는 할리우드 해커들이 띄우는 화면이 있는데, WSL에서 사용하려면:

```bash
sudo apt-add-repository ppa:hollywood/ppa
sudo apt-get update
sudo apt-get install byobu hollywood
hollywood
```

지우려면:

```bash
sudo apt-get remove byobu hollywood
```


## 환경 변수

### 미리 설정되어 있는 윈도우 환경 변수

- `%USERPROFILE%`: 사용자 홈 폴더
- `%HOMEPATH%`: 사용자 홈 폴더2, userprofile하고 뭔 차이인지...
- `%APPDATA%`: 사용자 홈 + `\AppData\Roaming`
- `%PROGRAMDATA%`: `C:\ProgramData`
- `%HOMEDRIVE%`: OS 설치 루트 경로. 보통은 `C:\`
- TODO

그리고 이런거 추가해두면 개발에 모기 주둥이만큼 도움 됨:

- `desktop`: `C:\Users\fixalot\Desktop`
- `tomcat-plugin`: `C:\project\workspace\.metadata\.plugins\org.eclipse.wst.server.core`
- `JAVA_HOME`: `C:\Program Files\Java\jdk1.8.0_112`


## shell: 프로토콜

윈도우의 특수 폴더(special folder)에 접근하는데 사용하는 명령어. 'shell: protocol' 혹은 'shell: commands'라 부른다. 

이 특수 폴더들은 일종의 가상 폴더라서 실제 파일 시스템 경로가 없으며, 환경 변수처럼 직접 경로나 값을 읽을 수 없다. 

그래서 그런지 파일 탐색기에서만 작동한다. 아직 셸에서 직접 경로를 얻는 방법은 못찾음. 셸에서 굳이 쓰겠다면, 파워셸에서는 `explorer shell:AppData`, CMD에서는 `start shell:appsfolder`와 같은 형태로 실행하는 방식으로만 가능하다.

아래는 `shell:` 프로토콜로 접근 가능한 특수 폴더의 목록이다:

- `shell:3D Objects`
- `shell:AccountPictures`
- `shell:AddNewProgramsFolder`
- `shell:Administrative Tools`
- `shell:AppData`: AppData 디렉터리
- `shell:AppDataDesktop`
- `shell:AppDataDocuments`
- `shell:AppDataFavorites`
- `shell:AppDataProgramData`
- `shell:AppMods`
- `shell:AppUpdatesFolder`
- `shell:Application Shortcuts`
- `shell:AppsFolder`: 앱 실행 링크 파일 모여있는 곳
- `shell:CD Burning`
- `shell:CSCFolder`
- `shell:Cache`
- `shell:Camera Roll`
- `shell:CameraRollLibrary`
- `shell:Captures`
- `shell:ChangeRemoveProgramsFolder`
- `shell:Common Administrative Tools`
- `shell:Common AppData`
- `shell:Common Desktop`
- `shell:Common Documents`
- `shell:Common Programs`
- `shell:Common Start Menu`
- `shell:Common Start Menu Places`
- `shell:Common Startup`
- `shell:Common Templates`
- `shell:CommonDownloads`
- `shell:CommonMusic`
- `shell:CommonPictures`
- `shell:CommonRingtones`
- `shell:CommonVideo`
- `shell:ConflictFolder`
- `shell:ConnectionsFolder`
- `shell:Contacts`
- `shell:ControlPanelFolder`
- `shell:Cookies`
- `shell:CredentialManager`
- `shell:CryptoKeys`
- `shell:Desktop`
- `shell:Development Files`
- `shell:Device Metadata Store`
- `shell:DocumentsLibrary`
- `shell:Downloads`
- `shell:DpapiKeys`
- `shell:Favorites`
- `shell:Fonts`
- `shell:GameTasks`
- `shell:History`
- `shell:HomeGroupCurrentUserFolder`
- `shell:HomeGroupFolder`
- `shell:ImplicitAppShortcuts`
- `shell:InternetFolder`
- `shell:Libraries`
- `shell:Links`
- `shell:Local AppData`
- `shell:Local Documents`
- `shell:Local Downloads`
- `shell:Local Music`
- `shell:Local Pictures`
- `shell:Local Videos`
- `shell:LocalAppDataLow`
- `shell:LocalizedResourcesDir`
- `shell:MAPIFolder`
- `shell:MusicLibrary`
- `shell:My Music`
- `shell:My Pictures`
- `shell:My Video`
- `shell:MyComputerFolder`
- `shell:NetHood`
- `shell:NetworkPlacesFolder`
- `shell:OEM Links`
- `shell:OneDrive`: 원드라이브
- `shell:OneDriveCameraRoll`
- `shell:OneDriveDocuments`
- `shell:OneDriveMusic`
- `shell:OneDrivePictures`
- `shell:Original Images`
- `shell:Personal`
- `shell:PhotoAlbums`
- `shell:PicturesLibrary`
- `shell:Playlists`
- `shell:PrintHood`
- `shell:PrintersFolder`
- `shell:Profile`
- `shell:ProgramFiles`
- `shell:ProgramFilesCommon`
- `shell:ProgramFilesCommonX64`
- `shell:ProgramFilesCommonX86`
- `shell:ProgramFilesX64`
- `shell:ProgramFilesX86`
- `shell:Programs`: 시작 메뉴의 프로그램 폴더
- `shell:Public`
- `shell:PublicAccountPictures`
- `shell:PublicGameTasks`
- `shell:PublicLibraries`
- `shell:Quick Launch`
- `shell:Recent`
- `shell:Recorded Calls`
- `shell:RecordedTVLibrary`
- `shell:RecycleBinFolder`
- `shell:ResourceDir`
- `shell:Retail Demo`
- `shell:Ringtones`
- `shell:Roamed Tile Images`
- `shell:Roaming Tiles`
- `shell:SavedGames`
- `shell:SavedPictures`
- `shell:SavedPicturesLibrary`
- `shell:Screenshots`
- `shell:SearchHistoryFolder`
- `shell:SearchHomeFolder`
- `shell:SearchTemplatesFolder`
- `shell:SearchTemplatesFolder`
- `shell:Searches`
- `shell:SendTo`
- `shell:Start Menu`
- `shell:Startup`: 시작 프로그램 폴더
- `shell:SyncCenterFolder`
- `shell:SyncResultsFolder`
- `shell:SyncSetupFolder`
- `shell:System`
- `shell:SystemCertificates`
- `shell:SystemX86`
- `shell:Templates`
- `shell:ThisDeviceFolder`
- `shell:ThisPCDesktopFolder`
- `shell:User Pinned`
- `shell:UserProfiles`
- `shell:UserProgramFiles`
- `shell:UserProgramFilesCommon`
- `shell:UsersFilesFolder`
- `shell:UsersLibrariesFolder`
- `shell:VideosLibrary`
- `shell:Windows`


## 단축키

[Windows의 바로 가기 키](https://support.microsoft.com/ko-kr/help/12445/windows-keyboard-shortcuts)
[Keyboard shortcuts in Windows](https://support.microsoft.com/en-us/windows/keyboard-shortcuts-in-windows-dcc61a57-8ff0-cffe-9796-cb9706c75eec)

### 전역

- <kbd>alt + enter</kbd>: 현재 선택된 요소의 속성 창.
- <kbd>ctrl + esc</kbd>: 시작 열기
- <kbd>ctrl + shift + esc</kbd>: 작업 관리자 실행
- <kbd>shift + f10</kbd>: 컨텍스트 메뉴 팝업(= 마우스 우클릭)
- <kbd>alt + space</kbd>: 활성화된 창의 '창 제어' 혹은 '창 시스템 메뉴' 열기. 창 제목을 우클릭한 것과 같음.
- <kbd>win + = </kbd>: 돋보기 실행. 실행 후 확대/축소는 <kbd>win + - </kbd> <kbd> wind + = </kbd>
- <kbd>win + ,</kbd>: 일시적 바탕화면 보기
- <kbd>win + .</kbd> <kbd>win + ;</kbd>: 이모지
- <kbd>win + a</kbd>: 알림 센터 열기 (11에서 바뀜)
- <kbd>win + b</kbd>: 트레이 아이콘 선택
- <kbd>win + alt + d</kbd>: 날짜 및 시간 표시 (11에서 안됨)
- <kbd>win + ctrl + c</kbd>: 흑백/컬러 전환
- <kbd>win + d</kbd>: 바탕화면 보기. 다시 WIN + D를 누르면 이전 상태로 돌아온다
- <kbd>win + e</kbd>: 파일 탐색기(내 PC) 실행
- <kbd>win + enter</kbd>: 내레이터 설정
- <kbd>win + g</kbd>: 윈도우 게임 바 열기. 화면 캡처 혹은 녹화 등의 기능을 제공
- <kbd>win + home</kbd>: 현재 사용중인 창을 제외한 모든 창 최소화.
- <kbd>win + i</kbd>: Metro 설정 열기
- <kbd>win + k</kbd>: 연결(무선 디스플레이 및 오디오)
- <kbd>win + l</kbd>: 로그오프 혹은 컴퓨터 잠금. 비밀번호 입력 화면으로 바뀜.
- <kbd>win + m</kbd>: 모든 창 최소화 - <kbd>win + shift + m</kbd>을 누르면 이전 상태로 돌아옴
- <kbd>win + p</kbd>: 프로젝트. 다중 화면 설정
- <kbd>win + pause</kbd>: 시스템 정보 창 열기
- <kbd>win + prtsc</kbd>: (PrintScreen) 화면 캡처 후 파일로 저장. 경로는 `%userprofiles%\Pictures\Screenshots`
- <kbd>win + r</kbd>: 실행 대화 상자 열기
- <kbd>win + s</kbd>: 윈도우 검색. 앱, 파일 등을 찾을 수 있음
- <kbd>win + shift + left</kbd> <kbd>win + shift + right</kbd>: 현재 창 이전/다음 모니터로 이동
- <kbd>win + shift + up</kbd>: 현재 창 수직 최대화
- <kbd>win + shift + s</kbd>: 캡처창 열기
- <kbd>win + u</kbd>: 접근성 센터 열기.
- <kbd>win + v</kbd>: 클립보드 이력
- <kbd>win + w</kbd>: Windows Ink 열기
- <kbd>win + x</kbd>: 시작 메뉴 열기
- <kbd>win + 스페이스바</kbd>: 입력 언어 및 자판 배열 전환
- <kbd>win + ctrl + 스페이스바</kbd>: 이전 입력으로 전환
- <kbd>win + /</kbd>: IME 재변환

### 창 크기/위치

- <kbd>win + 방향키</kbd>: 창 위치 이동 혹은 크기 변경
- <kbd>win + shift + 방향키</kbd>: 창 위치 이동 혹은 크기 변경

### 작업 표시줄

- <kbd>win + 숫자키</kbd>: 작업 표시줄에 고정된 앱의 실행 혹은 활성화
- <kbd>win + alt + 숫자키</kbd>: 작업 표시줄에 고정된 앱의 점프 목록 열기(마우스 우클릭으로 열리는 메뉴)
- <kbd>win + ctrl + 숫자키</kbd>: 작업 표시줄에 고정된 앱의 마지막 활성 창으로 전환한다. 같은 앱의 인스턴스가 둘 이상 실행되어 하나의 작업 표시줄 자리에 겹쳐있는 상황에서만 의미 있는 단축키.
- <kbd>win + shift + 숫자키</kbd>: 작업 표시줄에 고정된 앱의 새 인스턴스 시작
- <kbd>win + ctrl + shift + 숫자키</kbd>: 작업 표시줄에 고정된 앱을 **관리자 권한으로 새 인스턴스 시작**
- <kbd>win + t</kbd> <kbd>win + shift + t</kbd>: 작업 표시줄 단추 선택... 라는데 그냥 작업 표시줄에 탭 포커스 생긴다 보면 됨.

### 가상 데스크탑

- <kbd>win + tab</kbd>: 모든 가상 데스크탑과 실행중인 앱
- <kbd>win + ctrl + d</kbd>: 가상 데스크탑 추가
- <kbd>win + ctrl + f4</kbd>: 현재 가상 데스크탑 닫기
- <kbd>win + ctrl + left</kbd>: 왼쪽 가상 데스크탑 선택
- <kbd>win + ctrl + right</kbd>: 오른쪽 가상 데스크탑 선택

### 파일 탐색기

- 얘네 언젠가부터 작동 안함:
  - ~~<kbd>ctrl + tab</kbd>: 포커스가 주소 표시줄이나 검색 상자같은 곳에 있을 때 누르면 파일 목록으로 포커싱한다. 정식 명칭은 Property Tab Navigation인 모양. 진짜 탐색기에서만 유효하고 다른 앱의 파일 열기 탐색기에선 작동 안함.~~
  - ~~<kbd>ctrl + shift + tab</kbd>: 위와 비슷한데, 파일 목록 대신 열 머리 항목으로 포커싱.~~
- <kbd>alt + left arrow</kbd>: 뒤로
- <kbd>alt + right arrow</kbd>: 앞으로
- <kbd>alt + up arrow</kbd>: 위로
- <kbd>ctrl + mousewheel</kbd>: 보기변경
- <kbd>ctrl + shift + n</kbd>: 새폴더
- <kbd>ctrl + sfhit + e</kbd>: 좌측의 네비게이션이 현재 폴더 혹은 선택한 폴더로 이동하게 함.
- <kbd>alt + p</kbd>: 미리 보기 창 표시
- <kbd>alt + enter</kbd>: 속성 대화 상자 열

### 스티커메모

- <kbd>ctrl + b, i, u, t</kbd>: 굵게, 이탤릭, 밑줄, 취소선
- <kbd>ctrl + shift + l</kbd>: 목록 글머리
- <kbd>ctrl + tab</kbd>: 다음 창
- <kbd>ctrl + shift + tab</kbd>: 이전 창
- <kbd>ctrl + d</kbd>: 메모 삭제
- <kbd>ctrl + n</kbd>: 새 메모
- <kbd>ctrl + w</kbd>: 창 닫기

### Windows Terminal

- <kbd>win + ` </kbd>: 윈도우 터미널의 기본 셸로 지정된 앱 실행. 터미널이 실행된 상태에서만 작동한다.

끝.
