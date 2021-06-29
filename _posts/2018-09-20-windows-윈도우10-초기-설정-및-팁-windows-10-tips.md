---
layout: post
date: 2018-09-20 10:54:00 +0900
title: '[Windows] 윈도우10 초기 설정 및 팁 Windows 10 Tips'
categories:
  - windows
tags:
  - os
  - windows
  - telnet
  - environment
  - setup
  - shortcut
  - hotkey
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://support.microsoft.com/ko-kr/help/12445/windows-keyboard-shortcuts](https://support.microsoft.com/ko-kr/help/12445/windows-keyboard-shortcuts)
- [https://docs.microsoft.com/ko-kr/windows/wsl/install-win10](https://docs.microsoft.com/ko-kr/windows/wsl/install-win10)

요즘 들어 혜자로워진 마소의 윈도우10 초기 설정을 정리한 글.

## Windows 터미널

[Windows 터미널 개요](https://docs.microsoft.com/ko-kr/windows/terminal/)  
[Windows 터미널 설치](https://docs.microsoft.com/ko-kr/windows/terminal/get-started)

2020년인가... 새로 나온 윈도우용 터미널. 앱 하나에서 윈도우의 각종 쉘(CMD, 파워쉘, 파워쉘 레거시, Azure Cloud Shell, WSL 등)을 동시에 사용할 수 있고, 창 쪼개기 기능(이게 세션도 분리되는건지는 아직 몲)을 지원함.

[이 링크](https://www.microsoft.com/ko-kr/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)에서 설치하거나, 스토어에서 'Windows Terminal'을 검색하면 나온다.

![](/images/windows-terminal.png)

~~이거 쓰고 있으면 좀 해커 같아 보임~~

### 시작 위치 변경

터미널의 시작 위치를 변경하려면, 설정(<kbd>ctrl + ,</kbd>)에서 `Json 파일 열기`로 설정 파일을 열고:

```js
{
  "guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
  "hidden": false,
  "name": "PowerShell",
  "source": "Windows.Terminal.PowershellCore",
  "startingDirectory": "C:/dev/git"
}
```

이런식으로 `startingDirectory`를 추가하면 된다.

## [OpenSSH](https://docs.microsoft.com/ko-kr/windows-server/administration/openssh/openssh_install_firstuse)

putty는 안녕. 터미널은 이제 파워쉘로 접속한다.

우선 설치를 하자. 파워쉘(관리자 권한)에서 다음 줄 실행:

```bash
# OpenSSH 설치
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'
```

클라이언트 기능을 활성화하고:

```bash
# Install the OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```

접속한다:

```bash
# 접속
ssh username@servername

# servername 서버에 22번 포트로 접속
ssh username@servername -p 22
```

만약 SSH 서버를 구성하려면 아래 명령어로 서버 기능을 활성화하면 되는데:

```bash
# Install the OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

나머지 설명은 [도움말](https://docs.microsoft.com/ko-kr/windows-server/administration/openssh/openssh_install_firstuse)을 보자.

## [WSL<sup>Windows Subsystem for Linux</sup>](https://docs.microsoft.com/ko-kr/windows/wsl/install-win10)  

WSL은 가상 머신 등의 설정 없이 윈도우 상에서 리눅스 명령어를 직접 실행할 수 있는 환경을 말한다. 2020년엔 버전업 된 [WSL 2](https://docs.microsoft.com/ko-kr/windows/wsl/compare-versions)가 나왔다.

설치 방법은 우선 'Linux용 Windows 하위 시스템' 옵션 기능을 사용하도록 설정하고:

```bash
# PowerShell 관리자 권한으로 실행, 재시작 필요
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

# 예전 버전의 도움말에선 이렇게 알랴줌
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

그 다음, WSL 2를 쓸 생각이 없다면 바로 서브 시스템을 설치하면 된다. [요것](https://aka.ms/wslstore) 혹은 [요놈](ms-windows-store://collection/?CollectionId=LinuxDistros)을 눌러 나오는 앱 중 하나를 골라 설치하거나:

![](/images/wsl-store-resize.png)

~~아니면 파워쉘에서 직접:~~

```bash
Invoke-WebRequest -Uri https://aka.ms/wsl-ubuntu-1604 -OutFile Ubuntu.appx -UseBasicParsing
```

~~명령어로 다운로드/설치 하면 됨~~  
오늘(2021-01-20) 확인해보니 설치가 제대로 안되며, 도움말에서도 쉘 명령어로 설치하라는 내용은 사라짐. 되는 방법 찾기 귀찮으니 그냥 스토어 가서 까세영. 😒

## [Chocolatey](https://chocolatey.org/install)

윈도우판 `apt-get`이다. 파워쉘(관리자 권한)에서 다음 줄 실행:

```bash
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))
```

설치 후 쉘에서:

```bash
choco install winscp
# winscp 설치

choco install notepadplusplus
# notepad++ 설치

choco install firefox-dev googlechrome googlechrome.dev
# 파폭과 크롬 설치
```

이런식으로 앱 설치 가능.

### 서브 시스템의 실제 경로

WSL1: 루트의 실제 경로는 설치한 서브 시스템별로 다르지만, 공통적으로 `%USERPROFILE%\AppData\Local\Packages` 까지는 같고 `\LocalState\rootfs`로 끝난다.  
예를 들어 우분투는 `C:\Users\norit\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs` 요렇게 됨.

**WSL2**: 버전 2에선 쉘의 홈에서 `powershell.exe /c start .`을 입력하면 해당하는 경로로 윈도우 탐색기가 열린다. 혹은 실행 대화 상자나 탐색기에서 `\\wsl$`을 입력하면 OS별 루트 경로에 바로 접근할 수 있다.

## 윈도우10 초기 설정

### 시건 형식 변경

`설정` > `날짜 및 시간 설정` >  `지역` > `데이터 형식 변경`에서

- 시작 요일: 월요일로 변경
- 간단한/자세한 시간: 12시간제에서 24시간제로 변경

### 텔넷 활성화

쉘(관리자 권한)에서 아래 실행:

```bash
# PS C:\> pkgmgr /iu:"TelnetClient" # pkgmgr.exe는 deprecated 되었음.
dism /online /Enable-Feature /FeatureName:TelnetClient
```

### 멀티 클립보드

`설정` > `시스템` > `클립보드`에서 '클립보드 검색 기록' 켬.

### 이모지 패널

`설정` > `장치` > `입력` > `고급 키보드 설정`에서 '이모티콘을 입력한 후 자동으로 패널을 닫지 않음' 체크 해제.

### 바로 가기 키 끄기

`설정` > `접근성 키보드 설정`에서 시프트 연타 등의 바로 가기 키 설정 끄기.

### 260자 경로 제한 풀기

레지스트리 편집기 > `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\FileSystem` > `LongPathsEnabled`의 값을 1로 변경

## 최적화 설정

말이 최적화 설정이지 하드웨어만 좋으면 이런거 안해도 된다.

### 작업 기록 끄기

야동, 야짤을 누군가에게 들키기 싫으면:

`설정` > `개인 정보` > `Windows 사용 권한` > `작업 기록`에서 '이 장치에서 내 활동 기록 저장'과 'Microsoft에 내 활동 기록 보내기'를 끈다.

### 업데이트용 임시 파일 보관하지 않기

`설정` > `업데이트 및 보안` > `배달 최적화`에서 '다른 PC에서 다운로드 허용'을 끔.

### 자동 프록시 끄기

`설정` > `네트워크 및 인터넷` > `프록시`에서 '자동으로 설정 검색'을 끔. 이거 끄면 브라우저 페이지 이동 속도 빨라진다는데, 정확히 무슨 기능인지 연구 필요함.

### 피드백 끄기

`설정` > `개인 정보` > `피드백 및 진단`에서 피드백, 진단 등의 기능을 끔.

### 백그라운드 앱 끄기

`설정` > `개인 정보` > `백그라운드 앱`에서 앱 찾아서 끄기.

## 기타 팁

### 구버전 제어판 열기

윈도우 검색<kbd>win + s</kbd>에서 '제어판'으로 검색하면 나온다. 만약 검색이 안되면 실행 대화 상자<kbd>win + r</kbd>에서 `Control Panel` 입력.

### ~~탐새끼~~ 탐색기 아이콘 오버레이 우선순위 설정

![](/images/icon-overlay-order.png)

`컴퓨터\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\ShellIconOverlayIdentifiers`

위 경로의 레지스트리를 수정한다.
공백이 많은 순으로 우선정렬 되니까 원하는 아이콘이 위로 올라오도록 공백의 개수를 조절하면 됨.

### 앱으로 인식되지 않는 파일을 시작 화면에 등록하기

`C:\ProgramData\Microsoft\Windows\Start Menu` 여기에 바로가기를 붙여넣고 검색<kbd>win + s</kbd>하면 앱으로 뜸.  
`%APPDATA%\Microsoft\Windows\Start Menu\Programs` 여기에 놔도 된다. 이 경로는 `shell:programs`으로 접근할 수 있다.

### 시작 프로그램 등록

`%APPDATA%\Microsoft\Windows\Start Menu\Programs\Startup` 경로에 실행파일 바로가기 복붙하면 끟. 이 경로의 숏컷은 `shell:startup`.

### 바로가기의 시작 위치 변경

바로 가기의 속성에서 `시작 위치`를 원하는 곳(e.g. `C:\dev\git`)으로 변경하면 된다. 만약 `대상`의 값에 `--cd-to-home` 같은 옵션이 있으면 지워야 제대로 작동함.

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

## shell: 프로토콜로 접근 가능한 특수 폴더 목록

- `shell:3D Objects`
- `shell:AccountPictures`
- `shell:AddNewProgramsFolder`
- `shell:Administrative Tools`
- `shell:AppData`
- `shell:AppDataDesktop`
- `shell:AppDataDocuments`
- `shell:AppDataFavorites`
- `shell:AppDataProgramData`
- `shell:AppMods`
- `shell:AppUpdatesFolder`
- `shell:Application Shortcuts`
- `shell:AppsFolder`
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
- <kbd>ctrl + shift + esc</kbd>: 작업 관리자 실행
- <kbd>shift + f10</kbd>: 컨텍스트 메뉴 팝업(=마우스 우클릭)
- <kbd>win +  +</kbd>: 돋보기 실행. 실행 후 확대/축소는 <kbd>win + -</kbd>/<kbd> wind + =</kbd>
- <kbd>win + ,</kbd>: 일시적 바탕화면 보기
- <kbd>win + .</kbd>/<kbd>win + ;</kbd>: 이모지
- <kbd>win + a</kbd>: 알림 센터 열기
- <kbd>win + b</kbd>: 트레이 아이콘 선택
- <kbd>win + ctrl + c</kbd>: 흑백/컬러 전환
- <kbd>win + d</kbd>: 바탕화면 보기. 다시 WIN + D를 누르면 이전 상태로 돌아온다
- <kbd>win + e</kbd>: 윈도우 탐색기(내 PC) 실행
- <kbd>win + enter</kbd>: 내레이터 설정
- <kbd>win + g</kbd>: 게임 표시줄 열기. 화면 캡처 혹은 녹화 등의 기능을 제공
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
- <kbd>win + shift + left</kbd> or <kbd>win + shift + right</kbd>: 현재 창 이전/다음 모니터로 이동
- <kbd>win + shift + up</kbd>: 현재 창 수직 최대화
- <kbd>win + shift + s</kbd>: 캡처 표시줄 열기
- <kbd>win + u</kbd>: 접근성 센터 열기.
- <kbd>win + v</kbd>: 클립보드 이력
- <kbd>win + w</kbd>: Windows Ink 열기
- <kbd>win + x</kbd>: 시작 메뉴 열기
- <kbd>win + alt + d</kbd>: 날짜 및 시간 표시
- <kbd>win + 방향키</kbd>: 창 위치 이동 혹은 크기 변경
- <kbd>win + shift + 방향키</kbd>: 창 위치 이동 혹은 크기 변경
- <kbd>win + 스페이스바</kbd>: 입력 언어 및 자판 배열 전환
- <kbd>win + ctrl + 스페이스바</kbd>: 이전 입력으로 전환
- <kbd>win + /</kbd>: IME 재변환

#### 작업 표시줄

- <kbd>win + 숫자키</kbd>: 작업 표시줄에 고정된 앱의 실행 혹은 활성화
- <kbd>win + shift + 숫자키</kbd>: 작업 표시줄에 고정된 앱의 새 인스턴스 시작
- <kbd>win + alt + 숫자키</kbd>: 작업 표시줄에 고정된 앱의 점프 목록 열기(마우스 우클릭으로 열리는 메뉴)
- <kbd>win + ctrl + 숫자키</kbd>: 작업 표시줄에 고정된 앱의 마지막 활성 창으로 전환(뭔 소리람...)
- <kbd>win + ctrl + shift + 숫자키</kbd>: 작업 표시줄에 고정된 앱을 **관리자 권한으로 새 인스턴스 시작**
- <kbd>win + t</kbd> or <kbd>win + shift + t</kbd>: 작업 표시줄 단추 선택. 실행 중인 창을 선택

#### 데스크탑

- <kbd>win + tab</kbd>: 모든 데스크탑과 실행중인 앱
- <kbd>win + ctrl + d</kbd>: 데스크탑 추가
- <kbd>win + ctrl + f4</kbd>: 현재 데스크탑 닫기
- <kbd>win + ctrl + left</kbd>: 왼쪽 데스크탑 선택
- <kbd>win + ctrl + right</kbd>: 오른쪽 데스크탑 선택

### 탐색기

- <kbd>alt + left arrow</kbd>: 뒤로
- <kbd>alt + right arrow</kbd>: 앞으로
- <kbd>alt + up arrow</kbd>: 위로
- <kbd>ctrl + mousewheel</kbd>: 보기변경
- <kbd>ctrl + shift + n</kbd>: 새폴더
- (탐색기 열자 마자) <kbd>spacebar</kbd>: 단축키라기보단 팁인데, 탐색기에서 파일 선택모드로 바로 진입할 수 있음.

### 스티커메모

- <kbd>ctrl + b, i, u, t</kbd>: 굵게, 이탤릭, 밑줄, 취소선
- <kbd>ctrl + shift + l</kbd>: 목록 글머리
- <kbd>ctrl + tab</kbd>: 다음 창
- <kbd>ctrl + shift + tab</kbd>: 이전 창
- <kbd>ctrl + d</kbd>: 메모 삭제
- <kbd>ctrl + n</kbd>: 새 메모
- <kbd>ctrl + w</kbd>: 창 닫기

끗.
