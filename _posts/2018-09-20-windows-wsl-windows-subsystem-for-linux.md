---
layout: post
date: 2018-09-20 10:54:00 +0900
title: '[Windows] WSL (Windows Subsystem for Linux)'
categories:
  - windows
tags:
  - windows
  - linux
  - os
  - wsl
  - powershell
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [WSL 설치 \| Microsoft Learn](https://docs.microsoft.com/ko-kr/windows/wsl/install)
- [WSL의 기본 명령 \| Microsoft Learn](https://docs.microsoft.com/ko-kr/windows/wsl/basic-commands)


## 개요

WSL은 가상 머신 등의 설정 없이 Windows 상에서 리눅스 명령어를 직접 실행할 수 있는 환경을 말한다. 2020년엔 버전업 된 [WSL 2](https://docs.microsoft.com/ko-kr/windows/wsl/compare-versions)가 나왔다.


## WSL 설치

이제 그냥 요거 한 방으로 됨:

```bash
# 기본 OS인 Ubuntu로 WSL 설치
wsl --install

# 설치 된 배포판 목록(도커 포함)과 버전 출력
wsl -l -v
```

버전 확인해서 2가 아니면 뭔가 잘못된 거니까 가이드 보고 다시 설치하자.

그 다음 새로 생성된 Ubuntu 앱 바로가기를 누르던지, 아니면 PowerShell이나 CMD에서 `wsl`을 치면 WSL 터미널로 진입한다.


## WSL 제거와 재설치

배포판만 재설치:

```bash
# 배포판 확인
wsl -l -v

# 배포판 제거
wsl --unregister Ubuntu

# Ubuntu 배포판 설치
wsl --install -d Ubuntu
```

WSL 통째로 재설치:

```bash
# WSL 기능 제거
wsl --uninstall

# 재설치
wsl --install
```


## 서브시스템의 실제 경로

WSL1: 루트의 실제 경로는 설치한 서브시스템별로 다르지만, 공통적으로 `%USERPROFILE%\AppData\Local\Packages` 까지는 같고 `\LocalState\rootfs`로 끝난다.

예를 들어 우분투는 `C:\Users\norit\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs` 요렇게 됨.

**WSL2**: 버전 2에선 셸에서 `powershell.exe /c start .`을 입력하면 해당하는 경로로 Windows 탐색기가 열린다. 혹은 실행 대화 상자나 탐색기에서 `\\wsl.localhost` 혹은 `\\wsl$`을 입력하면 OS별 루트 경로에 바로 접근할 수 있다.


## WSL에서 호스트 디렉터리 접근

`/mnt` 아래에 있는 드라이브들이 호스트(WSL이 설치된 Windows의 루트 경로) 디렉터리다. (배포판이 Ubuntu인 경우임. 다른 건 안써봐서 몲...)

```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
...
drvfs           476G  100G  377G  21% /mnt/c
drvfs           930G   69G  862G   8% /mnt/d
```


## [CMD 혹은 PowerShell에서 Linux 명령어 실행](https://docs.microsoft.com/ko-kr/windows/wsl/filesystems#run-linux-tools-from-a-windows-command-line)

```bash
wsl ls -la

dir | wsl grep git
```

리눅스 명령어 앞에 `wsl`을 붙이면 된다.


## WSL 설치 후 패키지 업그레이드

#### Ubuntu

```bash
# apt 저장소 업데이트
sudo apt update

# 업그레이드 가능 패키지 확인
sudo apt list --upgradeable

# 모든 패키지 업그레이드
sudo apt full-upgrade
```


## 우분투 터미널 꾸미기: Zsh, Powerlevel10k, ls color

[개발자를 위한 Windows 셋업 \| 니콜라스 유튜브](https://nomadcoders.co/windows-setup-for-developers/lectures/1833)

Zsh는 리눅스 기본 셸인 Bash의 확장 버전이고, Powerlevel10k은 테마 같은거다.

#### Zsh

[Zsh](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)

설치:

```bash
# zsh 설치
sudo apt install zsh

# oh-my-zsh 설치
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# zsh 실행
zsh

# bash 실행
bash

# 돌아가기
exit
```

#### 폰트 설치

Powerlevel10k를 사용하려면 전용 폰트가 필요하다. [여기서 받고](https://github.com/romkatv/powerlevel10k/#user-content-fonts) 설치한다.

그리고 사용하는 터미널에 폰트 설정까지 해줘야 아이콘 등의 셸 꾸미기 요소가 제대로 보인다.

Windows Terminal의 경우, `설정 > Ubuntu 프로필 > 모양`에서 우분투의 폰트를 변경하거나 `settings.json`의 Ubuntu 프로필에 `"font": { "face": "MesloLGS NF" }`를 추가한다.

#### PowerLevel10k

[PowerLevel10k](https://github.com/romkatv/powerlevel10k/#oh-my-zsh) 

설치는 아래 명령으로:

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Git 소스를 받은 다음 테마 설정: 

```bash
sed -i 's/^ZSH_THEME=.*/ZSH_THEME="powerlevel10k\/powerlevel10k"/' ~/.zshrc
```

터미널 재실행하면 자동으로 PowerLevel10k 환경설정을 시작한다. 나중에 다시 바꾸려면 `p10k configure`.

#### ls color

마지막으로 `ls` color 변경은 아래 실행:

```bash
echo 'LS_COLORS="ow=01;36;40" && export LS_COLORS' >> ~/.zshrc
```

하면 끗.

#### 기본 셸 변경하기

기본 셸이 Bash로 되돌려진 경우, 다시 Zsh로 변경하려면 아래 실행:

```bash
# 로그인 셸을 Zsh로 바꾸기
chsh -s $(which zsh)
```


## WSL에서 Windows의 Path 환경 변수 사용하지 않기

<https://stackoverflow.com/questions/51336147/how-to-remove-the-win10s-path-from-wsl>

루트 권한으로 `/etc/wsl.conf` 파일을 열어서:

```bash
sudo vi /etc/wsl.conf
```

아래를 추가한다:

```bash
[interop]
appendWindowsPath = false
```

그리고 WSL 재시작:

```bash
wsl --shutdown
```

그래도 안 되면 [여기](https://docs.microsoft.com/ko-kr/windows/wsl/filesystems#disable-interoperability)를 보자.


## 알아두기

### 처음부터 루트로 로그인하기

```js
wsl -u root
```

### WSL 기본 로그인 유저 바꾸기

`/etc/wsl.conf`의 `[user] default` 항목을 수정한 뒤 WSL을 재시작한다.

### WSL에서 Git Credential Manager for Windows 사용하기

Windows 환경에서는 기본값으로 '자격 증명 관리자'를 사용하는데, 이걸 변경하는 것.

WSL 터미널에서 아래를 입력하면 된다:

```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager-core.exe"
```

[출처](https://stackoverflow.com/questions/45925964/how-to-use-git-credential-store-on-wsl-ubuntu-on-windows)
