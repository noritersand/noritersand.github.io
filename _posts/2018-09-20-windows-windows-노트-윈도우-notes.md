---
layout: post
date: 2018-09-21 12:00:00 +0900
title: '[Windows] Windows 노트'
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

- [Keyboard shortcuts in Windows \| Microsoft Learn](https://support.microsoft.com/en-us/windows/keyboard-shortcuts-in-windows-dcc61a57-8ff0-cffe-9796-cb9706c75eec)
- [Windows의 바로 가기 키 \| Microsoft Learn](https://support.microsoft.com/ko-kr/windows/windows의-바로-가기-키-dcc61a57-8ff0-cffe-9796-cb9706c75eec)


## 개요

Windows 10, 11에서 공통 사항 분리한 글


## Windows 터미널

- [Windows 터미널 개요](https://docs.microsoft.com/ko-kr/windows/terminal/)
- [Windows 터미널 설치](https://docs.microsoft.com/ko-kr/windows/terminal/get-started)

2020년인가... 새로 나온 Windows용 터미널. 앱 하나에서 Windows의 각종 셸(CMD, PowerShell, PowerShell Core, Azure Cloud Shell, WSL 등)을 동시에 사용할 수 있고, 창 쪼개기 기능(이게 세션도 분리되는건지는 아직 몲)을 지원함.

[이 링크](https://www.microsoft.com/ko-kr/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)에서 설치하거나, 스토어에서 'Windows Terminal'을 검색하면 나온다.

![](/images/windows-terminal.png)

~~이거 쓰고 있으면 좀 해커 같아 보임~~

### 터미널 단축키 변경

설정(<kbd>ctrl + ,</kbd>)에서 `settings.json` 열고 `keybinding` 항목을 수정하면 됨.

#### ~~<kbd>f1</kbd>로 커맨드 팔레트 열기~~ ❌

```json
        {
            "id": "Terminal.ToggleCommandPalette",
            "keys": "f1"
        },
        {
            "id": null,
            "keys": "ctrl+shift+p"
        },
```

`설정 > 작업 메뉴`를 열어서 변경해도 되...는데, 이렇게 했더니 셸의 안에서 <kbd>f1</kbd> 입력이 필요할 때 안되길래 되돌림. 예를 들어 `htop`에선 <kbd>f1</kbd> 입력으로 도움말을 연다.

### 시작 위치 변경

**Windows 터미널 버전이 올라가면서 GUI 설정으로도 변경할 수 있게 되었음.**

터미널의 시작 위치를 변경하려면 설정 파일 `settings.json`을 아래처럼 수정한다. 해당 파일은 터미널 앱의 설정에서 좌측 하단 `Json 파일 열기`를 누르면 열림:

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

이런 식으로  `startingDirectory`를 추가하면 된다.

참고로 이 설정 파일에서 `list` 배열 안에 있는 객체들의 순서가 바로:

![](/images/windows-terminal-new-tabs.png)

터미널에서 새 탭을 열 때 선택할 수 있는 뇨솤들의 순서다.

### Git Bash 추가하기

터미널 설정에서 `새 프로필 추가 > 새 빈 프로필` 누르고 `명령줄` 항목을 다음처럼 입력한다:

```
C:\Program Files\Git\bin\bash.exe
```

⚠️ Git 설치 경로 기본값은 `C:\Program Files\Git\`이지만 다를 수도 있음

### 변경된 환경 변수를 적용하려면

새 탭이나 새 창을 열어도 갱신되지 않으니 터미널 앱을 재실행할 것. 

스크립트를 수정한 거라면 Dot sourcing operator`.`로 갱신 가능함.


## Windows Command-Line Utilities 윈도우 명령줄 유틸리티

Windows에서 명령줄 인터페이스를 통해 실행할 수 있는 독립적인 유틸리티다. 셸에 내장된 명령이 아니며, Windows에 기본 제공되는 유틸리티가 많다. 대체로 `C:\Windows\System32\` 또는 `C:\Windows\SysWOW64\`에 위치한다.

### icacls

파일/디렉터리의 권한(ACL)을 조회하고 수정하는 명령줄 유틸리티.

```powershell
icacls .\test\

# .\test\ BUILTIN\Administrators:(I)(OI)(CI)(F)
#         NT AUTHORITY\SYSTEM:(I)(OI)(CI)(F)
#         BUILTIN\Users:(I)(OI)(CI)(RX)
#         NT AUTHORITY\Authenticated Users:(I)(M)
#         NT AUTHORITY\Authenticated Users:(I)(OI)(CI)(IO)(M)
```

#### 권한 태그

- `I`: Inherited. 상위 폴더에서 상속된 권한
- `OI`: Object Inherit. 하위 파일로 권한 상속
- `CI`: Container Inherit. 하위 폴더로 권한 상속
- `F`: Full control
- `M`: Modify
- `RX`: Read & execute
- `R`: Read
- `W`: Write

### Handle

[Handle - Sysinternals | Microsoft Learn](https://learn.microsoft.com/ko-kr/sysinternals/downloads/handle)

Microsoft Sysinternals에 포함된 빌드 도구로, Windows에서 어떤 프로세스가 파일이나 디렉터리 등의 리소스를 사용 중인지 확인하는 유틸리티다.

⚠️ 이 유틸리티로도 프로세스를 못찾는 경우가 많음. 🤨 PowerToys에 비슷한 기능을 제공하는 [File Locksmith](https://learn.microsoft.com/ko-kr/windows/powertoys/file-locksmith)가 있는데, 이 쪽이 더 잘 찾는다.

별도로 설치해야 실행할 수 있으니 Chocolatey로 설치하자:

```bash
choco install handle -y
```

```bash
handle.exe "C:\project\test"

# explorer.exe   pid: 1234   type: File   C:\project\test\...
# node.exe       pid: 5678   type: File   C:\project\test\...

kill 1234
```

이렇게 파일을 잡고 있는 프로세스의 `pid`를 찾는데 쓴다.

실제 작동을 구현해보려면, 파워셸 창을 하나 열고 아래 입력:

```powershell
$path = "$env:TEMP\handle-test.txt";
"test" | Set-Content $path;

$stream = [System.IO.File]::Open(
    $path,
    [System.IO.FileMode]::Open,
    [System.IO.FileAccess]::Read,
    [System.IO.FileShare]::None
);
```

다음으로 관리자 권한의 파워셸 창을 아래처럼 확인했을 때 pid가 나와야 정상:

```powershell
handle.exe -au $env:temp\handle-test.txt

# ...
# pwsh.exe           pid: 26616  type: File          10B4: C:\Users\fixal\AppData\Local\Temp\handle-test.txt
```

이제 첫 번째 파워셸에서 스트림을 닫으면:

```powershell
$stream.Close();
```

관리자 파워셸에서 아무것도 안나와야 정상

```powershell
handle.exe -au $env:temp\handle-test.txt

# ...
# No matching handles found.
```

### where.exe

명령이나 실행 파일의 위치를 찾는 명령줄 유틸리티.

```bash
where.exe node

# C:\Program Files\nodejs\node.exe
# C:\Users\user\AppData\Local\hermes\node\node.exe
```

🚨 PowerShell에서는 `where`가 `Where-Object`의 별칭이라서 `.exe`를 붙여줘야 함

ℹ️ PowerShell 명령어 `Get-Command`가 `where.exe`를 대체한다.

### find

특정 문자열을 검색하는 유틸리티. 다른 명령과 파이프로 연결해 출력을 필터링하는 용도로도 사용함.

```bash
dir | find "Videos" # "Videos"를 포함하는 행 출력
```

`find` 뒤에 오는 큰따옴표로 감싸진 문자열을 포함하는 행을 출력한다. `/i` 옵션이 없으면 대소문자를 구분함.

### findstr

`find`와 비슷하지만 정규식 검색이 기본값인 유틸리티.

```bash
# '8081'로 필터링
netstat -nao | findstr '8081'
```

### xcopy

파일과 폴더 트리를 복사하는 유틸리티.

```bash
xcopy SOURCE DESTINATION /s /e /y
```

#### Options

- `/S`: 비어 있지 않은 폴더와 하위 폴더를 복사.
- `/E`: 비어 있는 경우를 포함하여 폴더와 하위 폴더를 복사. /S /E 스위치와 같다.
- `/Y`: 기존 대상 파일을 덮어쓸지 여부를 묻지 않는다.

### robocopy

파일과 폴더 트리를 복사하는 유틸리티. `xcopy`의 후속 격으로, 재시도/미러링/멀티스레드 복사 등 더 강력한 기능을 제공한다.

```bash
robocopy SOURCE DESTINATION /e /z /mt
```

#### Options

- `/E`: 비어 있는 경우를 포함하여 폴더와 하위 폴더를 복사.
- `/MIR`: 대상을 원본과 동일하게 미러링한다(원본에 없는 대상 파일/폴더는 삭제됨). ⚠️ 대상 경로를 잘못 지정하면 파일이 삭제될 수 있으니 주의.
- `/Z`: 네트워크 재시작 모드로 복사. 전송이 끊겨도 이어받기가 가능하다.
- `/MT[:N]`: 멀티스레드로 복사한다. `N`을 생략하면 기본값 8개 스레드 사용.
- `/R:N`: 실패 시 재시도 횟수 지정. 기본값은 100만 번으로 매우 크므로 보통 낮춰서 씀.
- `/W:N`: 재시도 간 대기 시간(초). 기본값은 30초.
- `/LOG:파일`: 진행 상황을 지정한 파일에 기록한다.

### netstat

현재 컴퓨터의 네트워크 연결과 포트 상태를 보여주는 유틸리티.

```bash
# PID와 함께 모든 연결과 수신 대기 포트를 숫자 형식으로 출력하되
netstat -nao

# '8081'로 필터링
netstat -nao | findstr '8081'
```

### [nslookup](https://docs.microsoft.com/ko-kr/windows-server/administration/windows-commands/nslookup)

DNS 서버에서 도메인 정보를 조회하는 유틸리티.

```bash
# noritersand.github.io 도메인에 대한 DNS 정보 조회
nslookup noritersand.github.io

# dns.google.com 서버에서 icanhazip.com 검색
nslookup icanhazip.com dns.google.com
```

### tasklist

실행 중인 프로세스 목록을 출력하는 유틸리티.

```bash
# 프로세스 목록을 출력하되 '18292'로 필터링
tasklist | findstr '18292'
```

### taskkill

실행 중인 프로세스를 종료하는 유틸리티.

```bash
# PID가 5888인 프로세스 중지
taskkill /f /pid 5888
```

### diskpart

Windows 전통의 디스크 관리 유틸리티. 디스크, 파티션, 볼륨 등을 확인하고 지정/변경할 수 있다.

```bash
diskpart

# Microsoft DiskPart 버전 10.0.18362.1
# ...

DISKPART> list volume

#  볼륨 ###  Ltr  레이블      Fs    형식       크기     상태          정보
#  --------  ---  ----------  ----  ---------  -------  ------------  --------
#  Volume 0         시스템 예약       NTFS   파티션          549 MB  정상         시스템
#  Volume 1     C                NTFS   파티션          237 GB  정상         부팅
#  Volume 2     D                NTFS   파티션          931 GB  정상

DISKPART> list disk

#  디스크 ###  상태           크기     사용 가능     Dyn  Gpt
#  ----------  -------------  -------  ------------  ---  ---
#  디스크 0    온라인        238 GB       1024 KB
#  디스크 1    온라인        931 GB           0 B

DISKPART> select disk 0

# 0 디스크가 선택한 디스크입니다.

DISKPART> list partition

#  파티션 ###  종류              크기     오프셋
#  ----------  ----------------  -------  -------
#  파티션 1    주                  549 MB  1024 KB
#  파티션 2    주                  237 GB   550 MB
#  파티션 3    복구                 562 MB   237 GB
```

### route

라우팅 테이블을 출력하거나 수정하는 유틸리티

```bash
# 도움말 보기
route /?

# 기본 출력
route PRINT
```

### ping

특정 IP나 도메인에 연결 확인용 데이터(ICMP Echo 요청)를 보내 응답 여부와 응답 시간을 확인하는 유틸리티. ICMP(Internet Control Message Protocol)를 사용하기 때문에 상대 컴퓨터에서 ICMP를 차단하면 네트워크 연결이 정상이어도 `ping`은 작동하지 않는다.

```bash
# KT 서버에 중단없이 연결 테스트 + 현재 시각 출력
ping -t 168.126.63.1
```

### tree

지정한 드라이브 또는 경로의 디렉터리 구조를 트리 형태로 출력한다.

```bash
tree /f /a
```

수동으로 작성하고 싶으면 [여기](https://tree.nathanfriend.com/)를 쓰자.

#### Options

- `/F`: 각 디렉터리의 파일 이름도 같이 출력한다.
- `/A`: 계층을 표현하는 특수문자를 ASCII 문자로 대체한다.

### [certutil](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/certutil)

파일의 해시값을 계산하거나 인증서 관련 정보를 관리하는 유틸리티.

```bash
certutil -hashfile 파일명 [해시알고리즘]
```

해시 알고리즘은 `MD2`, `MD4`, `MD5`, `SHA1`, `SHA256`, `SHA384`, `SHA512` 중에 하나여야 함. 생략했을 때의 기본값은 `SHA1`.

```bash
certutil -hashfile .\example.txt MD5

# SHA1의 .\example.txt 해시:
# f9c3df25f671c015100347d51cef76ee
# CertUtil: -hashfile 명령이 성공적으로 완료되었습니다.
```

### WinGet, Windows Package Manager Client

<https://github.com/microsoft/winget-cli>

Windows OS의 패키지 관리용 공식 CLI 툴. 리눅스의 `apt`와 비슷하다. Windows 버전에 따라 미리 설치되어 있기도 하다.

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

# PACKAGE_NAME 패키지의 상세정보 표시
winget show PACKAGE_NAME

# 설치된 패키지 목록을 출력. 버전 업그레이드가 가능한지도 표시됨
winget list

# PACKAGE_NAME 패키지 버전 업그레이드
winget upgrade PACKAGE_NAME
```

이 외에 이런 하위 명령어가 있음:

- `winget source`: 패키지 데이터의 출처를 관리할 때 씀
- `winget hash`: 설치 관리자에 대한 SHA256 해시를 생성
- `winget validate`: 매니페스트 파일의 유효성 검사
- `winget settings`: WinGet 설정 파일을 열거나 관리자 설정을 관리한다.
- `winget features`: 실험적 기능의 상태 표시
- `winget export`: 설치된 패키지 목록 내보내기
- `winget import`: 패키지 목록이 담긴 파일을 읽어 지정된 패키지를 설치한다.
- `winget pin`: 특정 패키지의 업데이트를 제한하거나 버전을 고정한다.
- `winget configure`: 구성 파일을 읽어 Windows 환경을 원하는 상태로 설정한다.


## rg

강력한 터미널용 텍스트 검색 도구. 오픈소스 프로그램인 [ripgrep](https://github.com/burntsushi/ripgrep)을 설치하면 사용할 수 있다.

Windows 기본 프로그램이 아니라 별도로 설치해야 한다:

```powershell
# Chocolatey
choco install ripgrep

# Scoop
scoop install ripgrep

# Winget
winget install BurntSushi.ripgrep.MSVC
```

이 도구는 `--files` 옵션을 사용하지 않으면 기본적으로 **파일의 내용 안에서** 주어진 검색어를 검색한다.

그리고 `-F` 옵션을 사용하지 않으면 주어진 검색어를 기본적으로 **정규식으로 해석한다.**

```powershell
# 현재 폴더와 그 하위를 재귀 검색
rg useState

# 특정 폴더에서 검색
rg TODO src/

# 특정 파일에서 검색
rg useState App.tsx

# 여러 검색어를 OR 조건으로 검색
rg "TODO|FIXME|HACK"

# 정규식으로 검색
rg "use[A-Z]\w+"
rg "^# " docs/

# 특정 확장자만 검색
rg useState -g "*.tsx"

# 여러 확장자 지정
rg useState -g "*.ts" -g "*.tsx"

# 특정 파일이나 디렉터리 제외
rg TODO -g "!*.test.tsx"
rg TODO -g "!node_modules"

# 검색 결과 주변 줄까지 출력
rg -C 2 useState

# 특정 문자열이 포함된 파일의 개수 확인
rg --count-matches useState
```

ℹ️ 검색어는 따옴표가 없어도 되지만, PowerShell에서는 `|`, `$`, `*` 같은 특수문자가 의미를 가질 수 있으니 이 경우에 한하여 따옴표로 감싸는 게 권장된다.

#### Options

- `-l` `--files-with-matches`: 일치하는 내용이 있는 파일의 이름만 출력한다.
- `-L` `--files-without-match`: 일치하는 내용이 없는 파일의 이름만 출력한다.
- `-i` `--ignore-case`: 대소문자를 구분하지 않고 검색한다.
- `-n` `--line-number`: 검색 결과에 줄 번호를 표시한다. 기본 옵션.
- `-N` `--no-line-number`: 검색 결과에서 줄 번호를 표시하지 않는다.
- `-g` `--glob`: 특정 파일 패턴만 검색하거나 제외한다. 예: `-g "*.tsx"`
- `-w` `--word-regexp`: 단어 단위로 일치하는 경우만 검색한다.
- `-F` `--fixed-strings`: 검색어를 정규식이 아닌 일반 문자열로 취급한다.
- `-C` `--context`: 검색 결과 주변의 지정한 줄 수를 함께 출력한다. 예: `-C 2`
- `--count-matches`: 각 파일에서 검색어가 일치한 횟수를 출력한다.
- `--files`: 파일 내용을 검색하지 않고 검색 대상 파일 목록만 출력한다.
- `--hidden`: 숨김 파일과 디렉터리도 검색한다.
- `--glob '!<패턴>'`: 특정 파일이나 디렉터리를 검색 대상에서 제외한다.
- `-v` `--invert-match`: 검색어와 일치하지 않는 줄을 출력한다.


## 환경 변수

### 미리 설정되어 있는 Windows 환경 변수

- `%USERPROFILE%`: 사용자 홈 폴더
- `%HOMEPATH%`: 사용자 홈 폴더2, userprofile하고 뭔 차이인지...
- `%APPDATA%`: 사용자 홈 + `\AppData\Roaming`
- `%LOCALAPPDATA%`: 사용자 홈 + `AppData\Local`
- `%PROGRAMDATA%`: `C:\ProgramData`
- `%HOMEDRIVE%`: OS 설치 루트 경로. 보통은 `C:\`

그리고 이런거 추가해두면 개발에 모기 주둥이만큼 도움 됨:

- `desktop`: `C:\Users\fixalot\Desktop`
- `tomcat-plugin`: `C:\project\workspace\.metadata\.plugins\org.eclipse.wst.server.core`
- `JAVA_HOME`: `C:\Program Files\Java\jdk1.8.0_112`


## shell: shortcuts

Windows의 known folder에 접근하는데 사용하는 명령어. 셸 명령어(Shell Commands) 또는 셸 바로가기('shell: shortcuts')라 부른다. 

known folder의 canonical name을 `shell:` 뒤에 붙인 형태다.

이 폴더들은 일종의 가상 폴더라서 실제 파일 시스템 경로가 없으며, 환경 변수처럼 직접 경로나 값을 읽을 수 없다. 그래서 그런지 파일 탐색기에서만 작동한다. 아직 셸에서 직접 경로를 얻는 방법은 못찾음. 셸에서 굳이 쓰겠다면, PowerShell에서는 `explorer shell:AppData`, CMD에서는 `start shell:appsfolder`와 같은 형태로 실행하는 방식으로만 가능하다.

#### 현재 로그인 사용자

- `shell:AccountPictures`
- `shell:AppData`: `C:\Users\fixal\AppData\Roaming` AppData 디렉터리
- `shell:AppDataDesktop`
- `shell:AppDataDocuments`
- `shell:AppDataFavorites`
- `shell:AppDataProgramData`
- `shell:Application Shortcuts`
- `shell:AppsFolder`: 앱 실행 링크 파일 모여있는 곳
- `shell:Camera Roll`
- `shell:CameraRollLibrary`
- `shell:Captures`
- `shell:Contacts`
- `shell:Cookies`
- `shell:CredentialManager`
- `shell:CryptoKeys`
- `shell:Desktop`
- `shell:Development Files`
- `shell:DocumentsLibrary`
- `shell:Downloads`
- `shell:DpapiKeys`
- `shell:Favorites`
- `shell:GameTasks`
- `shell:History`
- `shell:ImplicitAppShortcuts`
- `shell:Links`
- `shell:Local AppData`
- `shell:Local Documents`
- `shell:Local Downloads`
- `shell:Local Music`
- `shell:Local Pictures`
- `shell:Local Videos`
- `shell:LocalAppDataLow`
- `shell:MAPIFolder`
- `shell:MusicLibrary`
- `shell:My Music`
- `shell:My Pictures`
- `shell:My Video`
- `shell:NetHood`
- `shell:OEM Links`
- `shell:OneDrive`
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
- `shell:Profile`
- `shell:Programs`: `C:\Users\사용자이름\AppData\Roaming\Microsoft\Windows\Start Menu\Programs` 시작 메뉴의 프로그램 폴더
- `shell:Quick Launch`
- `shell:Recent`
- `shell:Recorded Calls`
- `shell:RecordedTVLibrary`
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
- `shell:Searches`
- `shell:SendTo`
- `shell:Start Menu`: `C:\Users\사용자이름\AppData\Roaming\Microsoft\Windows\Start Menu` 현재 사용자의 시작 메뉴 루트
- `shell:Startup`: `C:\Users\fixal\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup` 시작 프로그램 폴더
- `shell:SyncResultsFolder`
- `shell:SyncSetupFolder`
- `shell:Templates`
- `shell:User Pinned`
- `shell:UserProgramFiles`
- `shell:UserProgramFilesCommon`
- `shell:UsersFilesFolder`
- `shell:VideosLibrary`

#### 모든 사용자/공용

- `shell:Common Administrative Tools`
- `shell:Common AppData`
- `shell:Common Desktop`
- `shell:Common Documents`
- `shell:Common Programs`: `C:\ProgramData\Microsoft\Windows\Start Menu\Programs` 모든 사용자 프로그램 메뉴
- `shell:Common Start Menu`: `C:\ProgramData\Microsoft\Windows\Start Menu` 모든 사용자 시작 메뉴 루트
- `shell:Common Start Menu Places`
- `shell:Common Startup`
- `shell:Common Templates`
- `shell:CommonDownloads`
- `shell:CommonMusic`
- `shell:CommonPictures`
- `shell:CommonRingtones`
- `shell:CommonVideo`
- `shell:Public`
- `shell:PublicAccountPictures`
- `shell:PublicGameTasks`
- `shell:PublicLibraries`

#### 시스템 실제 경로

- `shell:Fonts`
- `shell:ProgramFiles`: `C:\Program Files`
- `shell:ProgramFilesCommon`: `C:\Program Files\Common Files`
- `shell:ProgramFilesCommonX64`
- `shell:ProgramFilesCommonX86`: `C:\Program Files (x86)\Common Files`
- `shell:ProgramFilesX64`: `C:\Program Files`
- `shell:ProgramFilesX86`: `C:\Program Files (x86)`
- `shell:ResourceDir`
- `shell:System`
- `shell:SystemCertificates`
- `shell:SystemX86`
- `shell:Windows`

#### 가상 셸 경로

- `shell:3D Objects`
- `shell:AddNewProgramsFolder`
- `shell:Administrative Tools`
- `shell:AppMods`
- `shell:AppUpdatesFolder`
- `shell:CD Burning`
- `shell:CSCFolder`
- `shell:Cache`
- `shell:ChangeRemoveProgramsFolder`
- `shell:ConflictFolder`
- `shell:ConnectionsFolder`
- `shell:ControlPanelFolder`: `제어판 > 모든 제어판 항목` 메뉴
- `shell:Device Metadata Store`
- `shell:HomeGroupCurrentUserFolder`
- `shell:HomeGroupFolder`
- `shell:InternetFolder`
- `shell:Libraries`
- `shell:LocalizedResourcesDir`
- `shell:MyComputerFolder`
- `shell:NetworkPlacesFolder`
- `shell:PrintersFolder`
- `shell:RecycleBinFolder`
- `shell:Retail Demo`
- `shell:SyncCenterFolder`
- `shell:ThisDeviceFolder`
- `shell:ThisPCDesktopFolder`
- `shell:UserProfiles`
- `shell:UsersLibrariesFolder`
