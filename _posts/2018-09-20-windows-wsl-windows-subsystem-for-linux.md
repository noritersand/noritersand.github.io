---
layout: post
date: 2018-09-20 10:54:00 +0900
title: '[Windows] WSL, Windows Subsystem for Linux'
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

WSL은 가상 머신 등의 설정 없이 윈도우 상에서 리눅스 명령어를 직접 실행할 수 있는 환경을 말한다. 2020년엔 버전업 된 [WSL 2](https://docs.microsoft.com/ko-kr/windows/wsl/compare-versions)가 나왔다.


## 설치

이제 그냥 요거 한 방으로 됨:

```bash
# 기본 OS인 Ubuntu로 wsl 설치
wsl --install

# 설치 된 wsl 목록(도커 포함)과 버전 확인
wsl -l -v
```

버전 확인해서 2가 아니면 뭔가 잘못된 거니까 가이드 보고 다시 설치하자.

그 다음 새로 생성된 Ubuntu 앱 바로가기를 누르던지, 아니면 파워셸이나 CMD에서 `wsl`을 치면 WSL 터미널로 진입한다.


## 서브 시스템의 실제 경로

WSL1: 루트의 실제 경로는 설치한 서브 시스템별로 다르지만, 공통적으로 `%USERPROFILE%\AppData\Local\Packages` 까지는 같고 `\LocalState\rootfs`로 끝난다.

예를 들어 우분투는 `C:\Users\norit\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc\LocalState\rootfs` 요렇게 됨.

**WSL2**: 버전 2에선 셸에서 `powershell.exe /c start .`을 입력하면 해당하는 경로로 윈도우 탐색기가 열린다. 혹은 실행 대화 상자나 탐색기에서 `\\wsl.localhost` 혹은 `\\wsl$`을 입력하면 OS별 루트 경로에 바로 접근할 수 있다.


## WSL에서 호스트 디렉터리 접근

우분투 말고는 설치를 안해봐서 정확하진 않으나 `/mnt` 아래에 있는 드라이브들이 호스트(WSL이 설치된 윈도우의 루트 경로) 디렉터리다.

```bash
$ df -h
Filesystem      Size  Used Avail Use% Mounted on
...
drvfs           476G  100G  377G  21% /mnt/c
drvfs           930G   69G  862G   8% /mnt/d
```


## [CMD 혹은 파워셸에서 Linux 명령어 실행](https://docs.microsoft.com/ko-kr/windows/wsl/filesystems#run-linux-tools-from-a-windows-command-line)

```bash
wsl ls -la

dir | wsl grep git
```

리눅스 명령어 앞에 `wsl`을 붙이면 된다.


## 우분투 터미널 꾸미기: Zsh, Powerlevel10k, ls color

[개발자를 위한 윈도우 셋업 \| 니콜라스 유튜브](https://nomadcoders.co/windows-setup-for-developers/lectures/1833)

Zsh는 리눅스 기본 셸인 Bash의 확장 버전이고, Powerlevel10k은 테마 같은거다.

일단 [Zsh](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)는:

```bash
# zsh 설치
apt install zsh

# oh-my-zsh 설치
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# zsh 실행
zsh

# bash로 돌아가기
bash
```

그리고 Powerlevel10k는 일단 [폰트를 받고](https://github.com/romkatv/powerlevel10k/#user-content-fonts), 우분투의 폰트 설정을 변경한다.

윈도우 터미널의 경우 설정에서 우분투의 폰트를 변경하거나 `settings.json`의 우분투 프로파일에 `"font": { "face": "MesloLGS NF" }`를 추가하면 된다.

[PowerLevel10k](https://github.com/romkatv/powerlevel10k/#oh-my-zsh) 설치는 아래 명령으로:

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

깃 소스를 받은 다음 `~/.zshrc` 파일에 `ZSH_THEME="powerlevel10k/powerlevel10k"` 설정. 터미널 재실행하면 자동으로 powerlevel10k 환경설정을 시작한다. 나중에 다시 바꾸려면 `p10k configure`.

마지막으로 ls color는 아래 실행:

```bash
echo 'LS_COLORS="ow=01;36;40" && export LS_COLORS' >> ~/.zshrc
```

하면 끗.

기본 셸 Zsh로 변경하려면 아래 실행:

```bash
# 로그인 셸을 Zsh로 바꾸기
chsh -s $(which zsh)
```

#### 작성자 저장용 PowerLevel10k 환경설정

어디에 붙은건지 헷갈림을 방지하기 위해 Prompt Style은 아래처럼 한다:

- WSL은 `(3) Rainbow.`
- 원격 서버의 일반 유저는 `(2) Classic.`
- 원격 서버의 루트 유저는 `(1) Lean.`
- Docker에선 이상하게 선택 불가하고 `(4) Pure.`로 고정됨.


## WSL용 쉘 사용자 정의 설정 모음

```bash
# .bashrc 또는 .zshrc 파일에 추가
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias sb='/mnt/c/Program\ Files/Sublime\ Text/subl.exe'
alias adb='adb.exe'
```


## WSL에서 윈도우의 환경 변수 사용하지 않기

[https://stackoverflow.com/questions/51336147/how-to-remove-the-win10s-path-from-wsl](https://stackoverflow.com/questions/51336147/how-to-remove-the-win10s-path-from-wsl)

WSL에서 윈도우의 환경 변수를 사용하지 않는 방법이다.

루트 계정으로 전환해서 `/etc/wsl.conf` 파일을 만들고 아래처럼 작성한다:

```bash
[interop]
  appendWindowsPath=false
```

그리고 WSL 재시작:

```bash
wsl --shutdown
```

그래도 안 되면 [여기](https://docs.microsoft.com/ko-kr/windows/wsl/filesystems#disable-interoperability)를 보자.


## WSL에서 Git Credential Manager for Windows 사용하기

윈도우 환경에서는 기본값으로 '자격 증명 관리자'를 사용하는데, 이걸 변경하는 것.

WSL 터미널에서 아래를 입력하면 된다:

```bash
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager-core.exe"
```

[출처](https://stackoverflow.com/questions/45925964/how-to-use-git-credential-store-on-wsl-ubuntu-on-windows)


## WSL 기본 로그인 유저 바꾸기

우분투인 경우:

```js
ubuntu config --default-user 유저_아이디
```

비밀번호를 잊어버려 재설정 할 때 쓴다.


