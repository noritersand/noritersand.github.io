---
layout: post
date: 2018-03-21 10:10:00 +0900
title: '[Git] Git 노트'
categories:
  - git
tags:
  - git
  - note
---

* Kramdown table of contents
{:toc .toc}


## GPG 키 설정하기

#### gnupg, pass 설치

```bash
sudo apt update
sudo apt install -y gnupg pass

gpg --version
pass --version
```

#### GPG 키 생성

다음 실행:

```bash
gpg --full-generate-key
```

프롬프트가 뜨면 아래처럼 설정한다:

- 유형: RSA and RSA
- RSA 비트 길이 4096
- 키 만료: 원하는 기간
- Real name: 원하는 이름
- Email address: 원하는 이메일
- Passphrase: 이 GPG 키를 가져올 때 사용하는 비밀번호(8자 이상 권장)

그리고 다음을 실행했을 때:

```bash
gpg --list-secret-keys --keyid-format=long
```

`uid` 오른쪽(상단에 위치할 수도 있음)이 생성한 키의 아이디다.

#### pass 초기화

```
pass init <위에서생성한GPG키의UID>
```

`~/.password-store` 파일이 생성된다. 파일 존재만 확인하면 됨:

```bash
# 확인
ls -la ~/.password-store
```

#### GPG TTY 설정

```
echo 'export GPG_TTY=$(tty)' >> ~/.bashrc
export GPG_TTY=$(tty)
```

#### Git Credential Manager

Git Credential Manager 설치 확인:

```
git credential-manager --version
```

없으면 설치 방법 검색해서 설치할 것.

```bash
# Git credential 설정 확인
git config --global --get-regexp '^credential\.'

# GCM을 GPG로 설정하기
git config --global credential.credentialStore gpg
```

#### 인증

```bash
git fetch
```

원격 저장소에 따라 다르지만, GitHub인 경우 GUI가 뜬다. 여기에 아이디/비번이든 토큰이든 입력해서 인증하면, 해당 값이 GPG로 암호화된다.

그리고 한 번 더 `git fetch`를 실행하면 passphrase를 물어보는데, 아까 입력했던 비밀번호를 입력하면 끗.

#### 키 저장 확인

```bash
find ~/.password-store -type f

# 또는

pass
```


## 서브모듈(submodule) 초기화 하기

```
repo/
├── .git/
└── child/
    └── .git/
```

1. `child/.git`을 완전히 제거한다.
2. `git rm --cached child` 명령으로 인덱스에서 제거
3. `git add child`로 다시 추가
4. 끗.


## Refspec이란?

로컬 ref(브랜치, 태그 등)와 리모트 ref(리모트의 브랜치, 태그 등)를 매핑하는 규칙 혹은 그 규칙에 맞춰 작성된 문자.

```
<로컬 ref>:<리모트 ref>
```

예를 몇 가지 들면:

```bash
# 로컬 main 브랜치를 리모트 main 브랜치로 push
git push origin main:main

# 로컬 dev 브랜치를 리모트 production 브랜치로 push
git push origin dev:production

# 리모트 feature/login 브랜치를 로컬 feature/auth 브랜치로 가져옴
git fetch origin feature/login:feature/auth
```

여기서 `main:main`, `dev:production`, `feature/login:feature/auth`가 refspec이다.


## Windows에 Git 서버 설치하기

- <http://gitblit.com/>
- <http://www.lesstif.com/pages/viewpage.action?pageId=26084460>

### git 서버 설치/구동

<http://www.lesstif.com/pages/viewpage.action?pageId=26084460>
위 링크 참고. (한 줄 요약: jetty가 내장된 gitblit으로 git 서버 구동.)

### JCE 설치

오라클에서 다운받은 압축파일을 풀면 jar파일이 몇개 있는데 요것들을 java설치경로/lib/security 아래에 덮어쓰기 한다.


## git filter-repo

<https://github.com/newren/git-filter-repo>

Git 히스토리를 재작성하기 위한 서드파티 툴. 예를 들어 특정 파일이나 민감한 정보 등을 히스토리에서 제거할 때 사용한다.

```bash
# 설치
pip install git-filter-repo

# 특정 파일 제거하기
git-filter-repo --path out/some-awesome-file.apk --invert-paths

# 100MB를 초과하는 모든 파일 제거하기
git-filter-repo --strip-blobs-bigger-than 100M
```

GitLab에서 GitHub로 저장소를 옮길 때 사용했다. GitHub는 무료 티어일 때 LFS를 100MB까지만 지원하기 때문에, 그 이상의 파일은 히스토리에서 지워줬어야 했다.

ℹ️ 히스토리를 재작성하는 공식 기능으로 `git-filter-brach`가 이미 있지만 [성능이 나쁘다며 오히려 이 툴을 추천](https://git-scm.com/docs/git-filter-branch#_warning)한다.


## 트리 해시 Tree Hash

트래 해시는 특정 커밋 시점에서의 프로젝트 디렉터리 구조(트리)를 나타내는 해시값이다. 커밋된 파일들과 디렉터리 구조를 바탕으로 계산한 해시값이며, 파일의 내용이나 파일 시스템의 구조가 바뀌면 같이 변경된다.

커밋 해시와 비교했을 때, 특정 커밋의 전체 상태를 나타내는 커밋 해시와 다르게, 트리 해시는 해당 커밋에서의 파일과 디렉터리 구조만을 나타낸다. 그래서 작성자나 날짜가 변경되더라도 트리 해시는 영향을 받지 않는다. 그리고 커밋 해시는 Git 저장소에서 고유한 값을 갖지만, 트리 해시는 파일의 내용과 구조가 동일하다면 여러 커밋에서 같은 값이 존재할 수 있다.

트리 해시를 확인하려면 `cat-file`이나 `rev-parse` 명령을 쓴다.

```bash
git cat-file commit HEAD

# 혹은

git rev-parse HEAD:./
```


## detected dubious ownership in repository at ...

```
fatal: detected dubious ownership in repository at '//wsl.localhost/Ubuntu/home/fixalot/repo/bun-testbed'
To add an exception for this directory, call:

  git config --global --add safe.directory '%(prefix)///wsl.localhost/Ubuntu/home/fixalot/repo/bun-testbed'
```

WSL에 있는 로컬 저장소를 호스트의 git으로 접근하려 할 때 이런 메시지와 오류가 발생한다. Git이 알려주는대로 `safe.directory` 설정을 추가하면 바로 해결된다.


## Git Credential

### Host Provider

Git Credential Manager(GCM)가 원격 저장소 URL을 보고 GitHub, GitLab, Bitbucket, Azure Repos 등 어떤 호스팅 서비스인지 자동으로 판별해 그에 맞는 인증 방식(OAuth, 개인 액세스 토큰 등)을 적용해주는 기능. 자동 판별이 실패하거나 강제로 지정하고 싶으면 `credential.provider` 설정으로 직접 명시할 수 있다. 생략했을 때의 기본값은 `auto`다.

ℹ️ 호스팅 서비스를 판별한 뒤 그에 맞는 인증 흐름(브라우저 팝업, 기기 코드 등)을 고르는 데 관여하는 설정이다.

```bash
git config --global credential.provider github
```

예를 들어 Provider가 GitHub일 때는(자동 판별이든 직접 지정이든) 세부 인증 방식을 `credential.gitHubAuthModes`로 지정할 수 있다. 값으로는 `browser`(웹 브라우저로 OAuth 로그인), `device`(기기 코드 인증), `pat`(개인 액세스 토큰 직접 입력) 등이 가능하다.

```bash
git config --global credential.gitHubAuthModes browser
```

### Helper

Git이 원격 저장소 인증 시 아이디/비밀번호(또는 토큰)를 어떤 방식으로 받아오고 캐싱할지 정하는 설정. `manager`, `cache`, `store` 등의 값으로 설정할 수 있고, 생략했을 때의 기본값은 환경에 따라 다르다.

아이디/비번을 매번 물어보는 경우가 있는데, 최신 버전에선 보통 이럴 일이 없지만 혹시 발생하면 일단 `제어판 > 사용자 계정 > 자격 증명 관리자`의 `Windows 자격 증명`에서 Git 항목을 확인해보자. 그래도 안되면 helper를 `manager`로 설정한다:

```bash
git config --global credential.helper manager
```

ℹ️ 아래처럼 절대 경로로 실행 파일을 직접 지정하면 Host와 WSL의 인증을 분리할 수 있음:

```
credential.helper=/usr/local/bin/git-credential-manager
```

### Store

credential helper(manager)가 인증 정보를 실제로 어디에 저장할지 지정하는 설정. `gpg`, `wincredman`, `plaintext` 등의 값으로 설정할 수 있고, 생략했을 때의 기본값은 환경에 따라 다르다. (Windows는 `wincredman`)

```bash
git config --global credential.credentialStore gpg
```

### git-credential-manager-core.exe: No such file or directory

만약 `C:/Program Files/Git/mingw64/libexec/git-core/git-credential-manager-core.exe: No such file or directory` 라는 메시지의 오류가 발생한다면, 원인은 git 설정 중 `credential.helper`의 이름이 잘못됐을 가능성이 크다.

우선 아래처럼 설정을 지워보고:

```bash
git config --global --unset-all credential.helper
```

해결이 안되면 `credential.helper`를 직접 지정해보자:

```bash
git config --global credential.helper "C:\Program Files\Git\mingw64\libexec\git-core\git-credential-wincred.exe"
```

참고로 credential helper를 확인하려면 아래처럼 입력한다:

```bash
git config -l | grep credential
# 출력 -> credential.helper=/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager-core.exe

git config --get-all --show-origin credential.helper
# 출력 -> file:/home/fixalot/.gitconfig   /mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager-core.exe
```

### WSL 환경의 git credential.helper

WSL에서는 `credential.helper`를 지정하지 않으면, 매번 비밀번호(혹은 토큰)를 입력해야 한다. 아래처럼 직접 지정해 줄 것:

```bash
# WSL에서
git config --global credential.helper "/mnt/c/Program\\ Files/Git/mingw64/libexec/git-core/git-credential-wincred.exe"
```

⚠️ 이렇게 설명하면 Host의 자격 증명을 같이 사용함. Host와 WSL의 인증을 분리하고 싶다면 이 방법을 사용하면 안됨.


## 자격 증명 관리자 Credential Manager

Windows에선 보통 Git을 설치할 때 'Git Credential Manager for Windows'를 같이 설치하는데, 요걸로 권한이 필요한 저장소에 접속할 때 필요한 자격증명을 관리한다.

문제는 다른 계정으로 바꾸는 방법을 모르겠다는 건데... 임시 방편으로 Windows 설정 메뉴인 '자격 증명 관리자'를 열어서 지워버리거나 직접 수정하는 방법이 있다.


## GitHub CLI

공식 도움말: https://cli.github.com/manual/

```bash
# 설치
winget install --id GitHub.cli
```

path에 자동으로 추가됨. 터미널 재실행 후:

```bash
gh auth login
```

이후 로그인 진행한 뒤, 원하는 저장소로 이동해서 필요한 명령 실행하면 된다.

```bash
# job 목록 보기
gh run list --limit 5

# 상세 보기
gh run view 1590799398 --verbose

# job 재실행
gh run rerun 1591056086

# job 취소
gh run cancel 1591056086
```


## ... have diverged

공개된 브랜치와 같은 이름의 브랜치를 강제 push 등으로 업로드하면, 강제 push 전에 해당 브랜치를 로컬로 내려받은 사람은 리모트 브랜치와 로컬 브랜치의 히스토리가 갈라진 상태가 됨. Git에선 이걸 diverged라고 부른다.


## revert는 작업 브랜치에서

master 브랜치에 feature 브랜치를 머지하는 상황이라 가정했을 때, 머지한 내용을 되돌리고 싶으면 master 브랜치를 리버트 하는게 아니라 feature 브랜치를 리버트하고 그 다음 다시 머지하는게 낫다. master를 리버트한 뒤에 feature에서 같은 라인을 수정 후 다시 머지하려고 하면 충돌이 발생하거나 변경 사항이 없다며 머지가 아예 안될 수도 있기 때문.

사실 master를 리버트 했다 해도, 리버트 후의 master에서 새로 만든 브랜치에 feature를 체리픽 하거나, revert 커밋을 feature로 역머지 후 작업하는 등 방법은 얼마든지 있긴하지만... 작업 브랜치가 아닌 메인 혹은 중간 브랜치에서 리버트를 자제한다 정도의 규칙이라고 생각하는게 맞겠다.

만약 feature 브랜치의 커밋이 잘게 쪼개져있어서 모두 리버트하기 번거로운 상황이라면 feature를 스쿼시 리베이스 후 리버트하면 된다.


## 대소문자 문제

특정 브랜치가 대소문자로 분리되어 중복 생성되는 경우가 발생할 수 있다. 이 경우 설정을 다음처럼:

```bash
git config core.ignorecase false
```

대소문자를 무시하지 말라고 해주면 됨.

A가 대소문자를 무시한 상태로 커밋을 올렸더니 feature 대신 Feature 브랜치가 생겼다고 하자. 대소문자를 무시하지 않는 설정을 하고 있던 B는 `pull` 후에 Feature 브랜치를 발견한다.

이 현상을 해소하기 위해 A는 대소문자를 무시하지 않도록 설정을 변경하고 다시 `push`한다. B는 `.git\refs\remotes\origin\Feature` 디렉터리를 `.git\refs\remotes\origin\feature`로 강제변경한 뒤 `git fetch --prune` 명령을 실행해서 Feature 브랜치가 삭제되도록 한다.


## 오토 패킹이 자주 발동하면

<https://stackoverflow.com/questions/8633981/what-does-auto-packing-the-repository-for-optimum-performance-mean/16233094>

dangling object가 많으면 이런 현상이 있다고 함. `git fsck`로 확인했을 때, dangling commit이 너무 많다고 판단되면 `git gc --prune=now`로 날리는 방법이 있다고 함.

혹은 아예:

```bash
git config gc.auto 0
```

설정에서 Garbage Collection 자동실행 주기를 0으로 만들어버리는 방법도 있다.


## 충돌이 발생하지 않는 조건

- 머지해오는 브랜치가 현재 브랜치의 자손일 때(Fast-forward 머지일 때)
- 다른 파일을 변경했을 때
- **같은 파일을 서로 다른 브랜치에서 변경했지만 변경 내용이 완전히 같을 때**


## Git Bash for Windows의 시작 위치 변경

바로 가기의 속성에서 `시작 위치`를 원하는 곳(예: `C:\dev\git`)으로 변경하면 되나, 속성에서 `대상`으로 지정된 실행파일의 뒤에 `--cd-to-home` 옵션을 지우지 않으면 효과가 없다.


## config의 user

`user.name`과 `user.email`은 작성자와 커미터로 사용되긴 하지만, 저장소 접근 권한과 연관된 것은 아니다. 만약 pull/push 등의 명령이 거부되었다면 문제는 `user` 설정이 아니므로 다른곳을 찾아봐야 함. 예를 들어 'Git Credential Manager for Windows(Windows용 Git 자격 증명 관리자)'를 사용한다면 같은 저장소에 여러 권한을 설정할 수가 없는데, SSH 설정 등으로 해결할 수 있다 하나 귀찮으므로 GitHub의 콜라보레이터 추가로 해결해부렀다.

참고로 자격 증명 관리자는 Atom, git bash, fork 등에서 사용하며, eclipse는 자체 관리하기 때문에 Windows의 자격 증명 관리자를 사용하지 않는다.


## author와 committer의 차이

- author: 코드를 실제로 작성한 자(커밋을 처음 만든 사람)
- committer: 실제 작성자를 대신하여 코드 작성자로 간주되는 자(커밋을 마지막으로 리베이스한 사람)

패치(patch)를 적용한다고 했을 때 패치로 가져오는 변경 사항을 실제 작성한 사람이 author, 패치를 브렌치에 적용한 사람은 committer다.

이 정보는 리베이스 등으로 커밋 아이디가 변경됐을 때 아래처럼 입력해 알 수 있다:

```bash
git log -1 --pretty=fuller
```


## git으로 파일내용이나 커밋로그 검색하기

<https://blog.outsider.ne.kr/849>


## merge, rebase, cherry-pick의 차이

<http://dogfeet.github.io/articles/2012/git-delta.html#-merge-rebase-cherry-pick->


## 기본 에디터 변경하기

```bash
git config --global core.editor "'C:/Program Files/Sublime Text 3/sublime_text.exe' -w"
```


## pull 명령에 자동 rebase 설정

```bash
git config --global pull.rebase true
```


## help

```bash
git help config
git config --help
```


## 되돌리기

modified 되돌리기: `git checkout -- .`
staged 되돌리기: `git reset HEAD`


## checkout: 특정 브랜치나 태그, 체크섬으로 이동

```bash
git checkout master # master 브렌치로 이동
git checkout -- . # 현재 경로의 모든 파일 되돌리기
git checkout HEAD . # 현재 경로의 모든 파일 되돌리기
```


## reset

```bash
git reset HEAD # 모든 파일의 스테이징 취소
git reset HEAD~1 # 이전 커밋으로 되돌림
```


## revert

reset과 차이점은 HEAD가 원래의 커밋을 가리키는게 아니라 새로운 커밋을 생성하여 변경한 내용을 기록한다.

```bash
git revert HEAD # 이전 커밋으로 되돌림
```


## rebase

merge의 경우 두 브랜치의 최종결과만을 기준으로 병합한다면 rebase는 브랜치의 변경 사항을 순서대로 다른 브랜치에 적용하며 병합한다. 저장소의 커밋 로그와 이력을 한 줄로 정리해주기 때문에 보통 완료된 브랜치를 마스터에 병합할 때 사용한다.


## cherry-pick

커밋하나만 rebase


## fetch

fetch는 데이터를 모두 가져오지만 머지는 생략한다. (pull = fetch + merge)

```bash
git fetch # origin 저장소에서 받아온다.
```


## patch

컴퓨터 프로그램에 추가하여 오류를 제거하거나 오류를 제거하기위한 소프트웨어


## fast-forward merge

현재 브랜치가 머지할 브랜치의 부모 커밋일 경우 발생. (어느 한 쪽이 다른 한 쪽으로 단순히 '앞으로 가기'만 하기 때문에 이런 이름이 붙음)


## 3-way merge

머지할 브랜치 둘과 공통 부모 커밋 셋의 커밋을 기준으로 머지하는 것


## branch

특정 커밋(의 최신버전)을 가리키는 포인터


## HEAD

현재 작업중인 브랜치를 가리키는 포인터. checkout 명령은 이 포인터를 변경하는 것이다.


## remote의 HEAD

원격 저장소의 default branch를 의미한다.


## 레퍼런스

레퍼런스란 커밋 체크섬을 이름으로 사용할 수 있도록 관리되는 파일 시스템이다.

예를 들어 master라는 브랜치는 사실 특정 커밋을 가리키는 포인터인데, 이 포인터가 어느 커밋을 가리키는지를 알 수 있도록 master라는 파일에 해당 커밋의 체크섬을 저장한다.

```bash
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

$ git show HEAD
commit f38273c9ecbea5a009667316425883a556f9ca47 (HEAD -> master, origin/master, origin/HEAD)

$ cat .git/refs/heads/master
f38273c9ecbea5a009667316425883a556f9ca47
```

`.git/refs/heads/master` 파일을 열어보면 master 브랜치의 마지막 커밋에 해당하는 체크섬이 저장되어 있는걸 확인할 수 있다.
