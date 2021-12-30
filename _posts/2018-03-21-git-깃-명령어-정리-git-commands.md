---
layout: post
date: 2018-03-21 10:16:45 +0900
title: '[Git] 깃 명령어 정리 Git Commands'
categories:
  - git
tags:
  - git
---

* Kramdown table of contents
{:toc .toc}

#### 참고한 문서

- [https://git-scm.com/docs](https://git-scm.com/docs)
- [https://git-scm.com/book/ko/v2](https://git-scm.com/book/ko/v2)
- [https://ohshitgit.com/](https://ohshitgit.com/)
- [https://learngitbranching.js.org/?locale=ko](https://learngitbranching.js.org/?locale=ko)

## 개요

Git CLI에서 자주 쓰이는 명령어와 옵션을 정리한 글.

## 용어

- 워킹 트리<sup>working tree</sup>: 버전관리되는 파일이 실제로 존재하는 공간. 이전의 공식 명칭은 working directory였으나 [변경되었다](https://github.com/git/git/commit/2a0e6cdedab306eccbd297c051035c13d0266343).
- 스테이징 에어리어<sup>staging area</sup>: 인덱스라고도 부른다.
- 헤드<sup>HEAD</sup>: 엄밀히 말하면 '현재 바라보고 있는 커밋'이지만 '현재 브랜치'란 의미로도 쓰임. 명령어에서 `HEAD~숫자` 처럼 쓰이는 경우, 숫자는 HEAD 기준 ~회 전의 커밋을 의미한다. 가령 `HEAD~2`는 HEAD 기준 2회 전 커밋이다.
- 깃 디렉터리<sup>git directory</sup>: git 사용에 필요한 모든 정보가 있는 로컬 저장소.
- 델타<sup>delta</sup>: 변경 사항 혹은 변경 내용. 이전 버전과 다음 버전의 차이를 의미함.
- 리모트 트래킹 브랜치<sup>remote-tracking branches</sup>: 리모트 저장소에 있는 브랜치를 추적하는 레퍼런스. `fetch`는 리모트 트래킹 브랜치를 리모트 저장소의 내용대로 갱신하는 명령이다.

![](/images/git-local-operations.png)

[이미지 출처](https://git-scm.com/book/en/v2/Getting-Started-Git-Basics)

## [공통 옵션](https://git-scm.com/docs/git#_options)

- `--version`
- `--help`
- `-C <path>`
- `-c <name>=<value>`: 이번 명령에 한해 적용할 설정을 지정하는 옵션. 다른 설정(`config`로 지정한 값들)보다 우선권을 갖는다.
- `--config-env=<name>=<envvar>`
- `--exec-path[=<path>]`
- `--html-path`
- `--man-path`
- `--info-path`
- `-p`
- `--paginate`
- `-P`
- `--no-pager`
- `--git-dir=<path>`
- `--work-tree=<path>`
- `--namespace=<path>`
- `--super-prefix=<path>`
- `--bare`
- `--no-replace-objects`
- `--literal-pathspecs`
- `--glob-pathspecs`
- `--noglob-pathspecs`
- `--icase-pathspecs`
- `--no-optional-locks`: 락이 필요한 선택적 작업을 수행하지 않게 한다.
- `--list-cmds=group[,group…]`

## add

#### staging / do track

작업폴더의 파일을 깃이 추적하게 하거나 커밋을 위한 준비상태로 만듦.

```bash
git add *
git add .
git add *.java
git add README.TXT
```

#### 모든 추적 및 추적되지 않는 파일의 변경 내용을 추가

```bash
git add -A
git add --all
# git rm을 쓰지 않고 직접 삭제한 파일도 모두 스테이징할 때 쓰면 유용하다.
```

#### 대화형으로 파일 스테이징

```bash
git add -i
```

## [blame](https://git-scm.com/docs/git-blame)

~~바보같은 커밋을 비난하기 위한 명령어~~ 데이터의 각 줄을 누가 언제 마지막으로 고쳤는지 확인할 수 있으며, 주로 디버깅 용도로 사용한다. 비슷한 명령으로 `bisect`가 있다.

[Pro Git book: Git으로 버그 찾기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Git%EC%9C%BC%EB%A1%9C-%EB%B2%84%EA%B7%B8-%EC%B0%BE%EA%B8%B0)

#### 파일 커밋 정보 줄 단위로 보기

```bash
git blame 파일
```

#### 특정 라인만 보기

```bash
git blame -L 시작라인,종료라인 파일
git blame -L 12,22 simplegit.rb
```

#### 파일의 줄 단위의 복사, 붙여넣기, 이동 정보 보기

```bash
git blame -M 파일
```

#### 파일의 줄 단위의 이동과 원본 파일 정보 보기

```bash
git blame -C -C 파일
```

## [branch](https://git-scm.com/docs/git-branch)

#### 브랜치 생성

현재 브랜치 기반의 신규 브랜치를 생성한다.

```bash
git branch mybranch
```

#### 다른 커밋 기반의 브랜치 생성

'체크아웃으로 헤드 이동 후 브랜치 생성'의 단축형. 여기서 커밋은 체크섬 외에 다른 브랜치나 태그가 올 수도 있다.

```bash
git branch 브랜치명 커밋
```

#### 브랜치 확인

```bash
git branch  # 로컬 저장소의 브랜치만 출력
git branch -r  # 리모트 브랜치 목록 보기
git branch -a  # 로컬과 리모트 브랜치 모두 보기
git branch -v  # 마지막 커밋 메시지도 함께 출력한다
git branch -vv  # 추적중인 브랜치 확인(업스트림 브랜치 확인)
```

#### 업스트림<sup>upstream</sup> 브랜치 설정 \#1

깃이 리모트 저장소의 특정 브랜치를 추적하도록 설정한다. 추적 중인 브랜치를 업스트림 브랜치라고 하는데, 업스트림 브랜치가 설정되어 있으면 `pull`이나 `push`할 때 리모트의 저장소명과 브랜치명을 생략할 수 있다.

```bash
git branch -u origin/test3  # origin 리모트의 test3 브랜치로 업스트림 브랜치 설정
git branch --set-upstream-to=origin/test3  # 같음
```

단, 이 방법은 설정하려는 브랜치에 미리 리모트에 만들어져 있는 상태여야만 가능하다. 만약 로컬에서 새로 생성한 브랜치를 업스트림으로 설정하고 싶다면, `git push --set-upstream`을 사용한다.

업스트림 브랜치를 해제하려면 `--unset-upstream` 옵션을 사용한다:

```bash
git branch --unset-upstream  # 업스트림 브랜치 지정 해제
```

#### 머지 여부 확인

머지가 완료되었거나 그렇지 않은 브랜치만 표시한다. 삭제해도 되는 브랜치를 조회할 때 사용한다.

```bash
git branch --merged
git branch --no-merged
```

#### 현재 브랜치를 다른 브랜치에 덮어쓰기

새 브랜치를 생성할 때 `-f` 옵션을 사용하면 이미 존재하는 브랜치를 무시하고 덮어쓴다.

```bash
git branch -f 대상브랜치 [커밋]
```

#### 브랜치 이름변경

```bash
git branch -m NAME_FROM NAME_TO
```

#### 브랜치 삭제

```bash
git branch -d mybranch
git branch -D mybranch  # 브랜치 강제삭제(보통 non-merged 브랜치를 삭제할 때 사용)
```

#### 리모트에서 삭제된 브랜치를 로컬에서도 삭제

이 명령은 리모트 저장소에서 삭제된 모든 브랜치를 로컬 브랜치와 리모트 트래킹 브랜치에서 삭제한다.

주의: 파워쉘에선 안됨

```bash
# 출처: https://stackoverflow.com/questions/7726949/remove-tracking-branches-no-longer-on-remote
git fetch -p && git branch -vv | awk '/: gone]/{print $1}' | xargs git branch -d

# 출처: https://medium.com/@kcmueller/delete-local-git-branches-that-were-deleted-on-remote-repository-b596b71b530c
git fetch -p && git branch -vv | grep ': gone]'|  grep -v "\*" | awk '{ print $1; }' | xargs -r git branch -d
```

## [checkout](https://git-scm.com/docs/git-checkout)

#### 브랜치 전환

헤드를 특정 브랜치의 마지막(가장 최근) 커밋으로 전환한다. 전환할 때 워킹 트리와 스테이징 에어리어가 변경 없이 깨끗하다면 이 둘은 헤드와 동일한 상태가 된다. 만약 unstaged(modified), untracked file 상태의 파일이 있다면 **변경점을 유지하며 전환한다.**

```bash
# master 브랜치로 전환
git checkout master
```

`checkout` 명령은 때에 따라 실패하기도 하는데, 대표적인 예로 현재 브랜치의 변경점을 커밋하지 않았으면서 전환하려는 브랜치와 충돌이 발생하는 경우다.

#### 브랜치를 새로 만들면서 체크아웃

현재 브랜치와 이름만 다른 브랜치를 생성하며 동시에 체크아웃한다.

```bash
git checkout -b 브랜치
```

#### 리모트 저장소에 있는 브랜치를 로컬에 만들기

~~이 때 `--track` 옵션을 사용해야 업스트림 브랜치로 설정됨.~~  
최근 버전에선 생략해도 된다.

```bash
git checkout -b test3 --track origin/test3 # origin의 test3 브랜치를 로컬에 생성하며 체크아웃
```

#### 다른 커밋으로 헤드 이동

특정 커밋의 체크섬이나 태그를 입력해 해당 시점의 스냅샷으로 이동하는 것을 의미한다. 브랜치를 만들지 않고 헤드를 이동할 수 있다.

```bash
git checkout 체크섬
git checkout 6f9021d4a03586a787ebcef2f94dd2eca1aec941

git checkout 태그
git checkout v0.3.1.5
```

브랜치가 아닌 커밋을 체크아웃 하면 이를 '분리된 헤드<sup>detached HEAD</sup>' 상태에 있다고 한다. 깃은 이 상태에서 바로 작업하지 말고 별도의 브랜치를 생성하여 작업할 것을 권장하고 있다.

```bash
$ git checkout afa51b4684417b3cfae3768f93b1fe73a206acf3

Note: checking out 'afa51b4684417b3cfae3768f93b1fe73a206acf3'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>
```

#### 파일 변경사항 되돌리기

워킹 트리와 스테이징 에어리어를 헤드의 내용대로 되돌린다. 이미 추적중인 대상만 되돌릴 수 있다. `status`에 표시되는 기준으로 modified, renamed, deleted는 되돌리지만 new file은 되돌리지 않는다.

```bash
# 현재 경로의 모든 파일 되돌리기
git checkout -- .  
git checkout HEAD .
```

#### fetch 시점으로 헤드 이동

`fetch`로 리모트 저장소의 데이터를 받아왔을때 'FETCH_HEAD'라는 포인터가 생성된다. 해당 포인터의 스냅샷을 확인 후 원하는 브랜치로 전환해 머지한다.

```bash
git checkout FETCH_HEAD
git checkout 리모트저장소/리모트브랜치
```

#### fetch로 받아온 데이터를 머지하지 않고 새 브랜치로 생성

```bash
git checkout -b FETCH_HEAD
git checkout -b 생성할브랜치 리모트저장소/리모트브랜치
```

```bash
git checkout -b hotFix anotherServer/master  # 이 명령은 지정한 리모트 저장소의 브랜치를 업스트림 브랜치로 만든다.
```

#### 업스트림 브랜치 설정 \#2

체크아웃하며 업스트림 브랜치 설정하는 방법.

```bash
git checkout -b 로컬브랜치 리모트저장소/리모트브랜치
git checkout --track 리모트저장소/리모트브랜치 # 1.6.2 이상
```

```bash
git checkout --track origin/serverfix

Branch serverfix set up to track remote branch refs/remotes/origin/serverfix.
Switched to a new branch 'serverfix'
```

#### 태그로 체크아웃

태그(가 가리키는 커밋) 기반의 브랜치를 생성하고 동시에 체크아웃까지 하는 방법이다.

```bash
git checkout -b version2 v2.0.0  # v2.0.0 기반 브랜치 version2로 체크아웃
```

## [cherry-pick](https://git-scm.com/docs/git-cherry-pick)

특정 커밋 하나만 현재 브랜치에 리베이스한다.

[Pro Git book: Rebase와 Cherry-Pick 워크플로](https://git-scm.com/book/ko/v2/%EB%B6%84%EC%82%B0-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C%EC%9D%98-Git-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0#_rebase_cherry_pick)

```bash
git cherry-pick 커밋명
git cherry-pick 376361 # 현재 브랜치에 376361 커밋의 변경사항을 반영한 신규 커밋 생성
git cherry-pick master # 현재 브랜치에 master 브랜치의 커밋 중 가장 마지막 커밋의 변경사항을 반영한 신규 커밋 생성
```

#### options

- `-n` `--no-commit`: 커밋을 만들지 않은 상태로 체리픽 한다.

#### 주의사항

'특정 커밋만 반영한다'는 말을 오해하지 말자. 하나의 파일에 한해선 그간의 모든 변경사항이 반영된다:

![](/images/cherry-pick1.png)

가령 pick-me 브랜치에 파일 `CONFLICT_ME.md`를 수정한 커밋이 세 개가 있으며 main 브랜치에 체리픽으로 일부만 반영하려는 상황이라고 가정하자. 이때 수정한 커밋 중 가장 마지막 커밋을 지정해 체리픽을 하면 세 번째 커밋의 변경사항만 main 브랜치에 반영될 거라 짐작할 수도 있으나...

![](/images/cherry-pick2.png)

실제 결과는 그렇지 않다. 세 번째 커밋보다 앞에 있는 첫 번째와 두 번째 커밋 모두 `CONFLICT_ME.md` 파일에 대한 변경이므로 원래 의도와는 다르게 모든 커밋이 main 브랜치에 반영된다. (그 와중의 충돌은 덤)

## [clean](https://git-scm.com/docs/git-clean)

추적중이지 않은<sup>untracked</sup> 파일 삭제하기.

```bash
git clean -f
git clean -f -d -x  # ignore 설정된 파일을 포함하며 추적중이지 않은 파일과 폴더를 모두 삭제한다.
```

`clean.requireForce` 설정이 `true`가 아니면 `clean` 명령은 항상 `-f`, `-i`, `-n` 옵션 중 하나가 명시되어야 실행된다. 기본적으로 현재 폴더를 기준으로 하위를 재귀탐색하기 때문에 recursive 옵션은 따로 없다.

#### options

- `-f` `--force`: 삭제 기본 옵션. 설정에 따라 생략할 수도 있다.
- `-i` `--interactive`: 대화 모드로 삭제
- `-n` `--dry-run`: 지워질 파일 목록 미리보기
- `-d`: 폴더도 삭제한다.
- `-x`: ignore 룰이 적용된 파일**도** 삭제한다.
- `-X`: ignore 룰이 적용된 파일**만** 삭제한다.

## clone

#### 저장소 복제

현재 경로에 로컬 깃 저장소가 될 디렉터리를 만들고 리모트 저장소의 데이터를 모두 받아온다. 디렉터리명을 따로 명시하지 않으면 리모트 저장소의 이름과 동일하게 생성된다.

```bash
git clone ~/Documents/workspace/ex/cal/src
git clone file://c:/users/noritersand/noriterGit/localServer from-local-repo
git clone https://github.com/noritersand/laboratory.git from-github-repo
```

#### 리모트 저장소를 복제하면서 bare repository로 설정

```bash
git clone --bare 저장소주소 [디렉터리]
```

#### 복제 시 데이터 제한하기

```bash
git clone --depth 200 ~/Documents/work/  # 마지막 200개의 커밋만 복제한다.
```

## commit

staged 상태인 파일을 깃 디렉터리에 저장한다. 커밋 메시지를 입력받기 위해 미리 지정된 에디터가 자동으로 실행되며 에디터에서 메시지를 작성하고 종료하면 커밋이 완료된다. 이 때 커밋 메시지가 코멘트(#으로 시작하는 라인)로만 작성되어 있으면 커밋은 취소된다.

```bash
git commit
```

#### 에디터 없이 커밋

에디터 실행을 생략하고 메시지를 즉시 입력한다.

```bash
git commit -m "hello this is test commit"
```

#### Signed-off-by 추가

커밋 메시지에 Signed-off-by를 추가한다. `-s` 혹은 `--signoff` 옵션을 사용하면 커밋 메시지에 `user.name`과 `user.email`이 자동으로 추가된다.

```bash
git commit -s -m 'commit message test'
[master 1204311] commit message test
 1 file changed, 1 insertion(+)

git log -1 HEAD
commit 1204311e62660b46c375c6e4d9a61a48a9adeb1d
Author: noritersand <who@where.com>
Date:   Thu Nov 17 10:07:35 2016 +0900

    commit message test

    Signed-off-by: noritersand <who@where.com>
```

#### 자동 스테이징<sup>staging</sup>

`-a` 옵션을 사용하면 `add` 없이 커밋할 수 있다. 단, 추적 중인 파일만 자동으로 스테이징된다.

```bash
git commit -a -m "auto staging"
```

#### 마지막 커밋 수정

가장 최근의 커밋을 수정할 때 사용한다. staged 상태인 파일이 있으면 마지막 커밋의 파일 변경 이력에 추가하며, 그렇지 않으면 커밋 메시지만 수정한다.

```bash
git commit --amend
```

```bash
git commit -m 'initial commit'
git add forgotten_file
git commit --amend  # forgotten_file이 마지막 커밋에 추가된다.
```

#### 마지막 커밋을 수정하되 커밋 메시지는 재사용

```bash
git commit -C HEAD --amend
```

## config

깃 저장소의 설정을 조회/관리하는 명령어.

#### options

- `--local`: 저장소별 설정을 의미한다. 옵션을 생략했을 때의 기본값이지만, 다른 옵션 조합에 따라 기본값이 아닐 때도 있다.
- `--global`: 현재 로그인한 유저의 설정.
- `--system`: 모든 유저의 설정.

#### 작업자의 이름/이메일 설정

```bash
git config --global user.name "이름"
git config --global user.email "이메일"
```

**참고**: `user.name`과 `user.email`은 author와 committer로 사용되긴 하지만, 저장소 접근 권한과 관련된 설정은 아니다.

#### 기본 편집기 설정

```bash
git config --global core.editor vim
```

#### diff 도구 변경

```bash
git config --global diff.tool vimdiff

# VSCODE를 diff 도구로 설정
# VSCODE 실행 경로가 path에 추가된 상태여야 함
# 안될 수도 있다.
git config --global diff.tool vscode
git config --global difftool.vscode.cmd 'code --wait --diff $LOCAL $REMOTE'
```

#### 머지 도구 변경

```bash
git config --global merge.tool kdiff3

# VSCODE를 머지 도구로 설정
# VSCODE 실행 경로가 path에 추가된 상태여야 함
# 얘도 안될 수 있음... 😒
git config --global merge.tool vscode
git config --global mergetool.vscode.cmd 'extMerge "$BASE" "$LOCAL" "$REMOTE" "$MERGED"'
git config --global mergetool.vscode.cmd 'code --wait $MERGED'
```

#### 설정 확인

```bash
git config --local --list
git config --local -l
git config --global --list
git config --local core.editor
```

#### 단축어<sup>alias</sup> 만들기

```bash
git config --global alias.사용할키워드 '명령어'
```

```bash
git config --global alias.ss 'status'
git config --global alias.br 'branch'
git config --global alias.ck 'checkout'
git config --global alias.sw 'switch'
git config --global alias.rs 'restore'
git config --global alias.cp 'cherry-pick'
git config --global alias.unstage 'reset HEAD --'
git config --global alias.visual '!gitk'
git config --global alias.hide 'update-index --assume-unchanged'
git config --global alias.unhide 'update-index --no-assume-unchanged'
git config --global alias.hidden '! git ls-files -v | grep "^h" | cut -c3-'
git config --global alias.f 'fetch'
git config --global alias.fp 'fetch --prune'
git config --global alias.log2 'log --all --graph --pretty=oneline'
git config --global alias.log-graph 'log --graph --pretty=oneline'
git config --global alias.log-oneline 'log --pretty=oneline'
```

#### 단축어 목록 보기

```bash
git config --global --get-regexp alias
git config --global --list | grep alias
```

#### 설정 삭제

```bash
git config --global --unset alias.ss
```

#### SSL 검증 비활성화

윈도우 환경에선 가끔 `SSL Certificate problem: unable to get local issuer certificate` 에러가 터지며 `fetch`나 `push`가 안될때가 있는데, 이럴 때는 아래처럼 SSL 검증을 꺼버리면 해결되긴 한다:

```bash
git config --global http.sslVerify false
```

다만 이건 급할 때나 쓰는 임시 방편이니 가급적이면 이렇게:

```bash
git config --global http.sslbackend schannel
```

`http.sslbackend`를 `schannel`로 변경하자. [Schannel은 윈도우의 빌트인 암호화 공급자](https://docs.microsoft.com/ko-kr/windows/win32/secauthn/secure-channel?redirectedfrom=MSDN)이다. 정식 명칭은 Secure Channel

[깃이 기본적으로 'Linux' crypto backend를 사용하는게 원인](https://stackoverflow.com/questions/23885449/unable-to-resolve-unable-to-get-local-issuer-certificate-using-git-on-windows#answer-53064542)이라는 말이 있음.

그런데 깃 최초 설치 후 확인해보면 이렇게 나옴 🤔:

```bash
$ git config -l | grep ssl
http.sslbackend=openssl
http.sslcainfo=C:/Program Files/Git/mingw64/ssl/certs/ca-bundle.crt
```

만약 저장소가 여러개이면서 SSL 문제가 해결이 안되면 다음처럼:

```bash
git config --global http.https://noritersand.github.io.sslverify false
```

특정 리모트 주소만 검증을 끄도록 하는 방법도 있다.

## [diff](https://git-scm.com/docs/git-diff)

지정한 영역끼리의 변경 사항을 출력한다.

#### options

- `--check`: 충돌 문자 혹은 공백 오류가 있는지 확인
- `--name-only`: 변경된 파일의 이름만 출력
- `--name-status`: 변경된 파일의 이름만 출력하면서 변경 상태를 표시해 줌.

#### diff 도구 실행

```bash
git difftool
```

`diff.tool`로 지정한 도구를 실행한다.

#### unstaged와 staged의 비교

```bash
git diff
```

#### staged와 commited의 비교

```bash
git diff --cached
git diff --staged
```

#### 커밋끼리 비교

```bash
git diff HEAD~1 # HEAD의 변경사항 출력. git diff HEAD~1..HEAD와 같다.
git diff HEAD~3..HEAD~1 # 3회 전 커밋과 1회 전 커밋의 변경사항 전체 비교.
git diff 87a8baee219b8a9ad2dfd5415e5257b7c5389277..d5bedb1e2624ad080e0aae4ed66acd08c9958c43 ./README.md # 두 커밋 간 비교하되 README.md 파일만
```

좌측 리비전을 기준으로 우측 리비전에서 어떤 내용이 바뀐건지 보여준다. 따라서 더 오래된 커밋을 좌측에 놓는게 좋다. `..`은 공백으로 대체할 수 있다.

#### 브랜치간 비교

```bash
# 워킹트리와 draft/noritersand 브랜치의 전체 파일 비교
git diff draft/noritersand

# HEAD와 draft/noritersand 브랜치의 전체 파일 비교
git diff draft/noritersand HEAD
```

`HEAD`를 생략하면 워킹트리와 비교하니 주의.

```bash
# 'hugo' 브랜치와 현재 브랜치의 README.md 파일만 비교
git diff hugo ./README.md

# 'hugo' 브랜치와 'master' 브랜치의 README.md 파일만 비교
git diff hugo  master ./README.md

# 위와 같음
git diff hugo..master ./README.md
```

#### 충돌 문자나 공백 에러 확인

```bash
git diff --check  # 충돌 문자가 있거나 공백 에러가 있는지 확인
```

`--check` 옵션의 주 사용처는 머지 실패 시 충돌 확인이다. 충돌이 발생하면 해당 파일에 충돌 문자가 삽입되며 modified 상태가 되기 때문.

```bash
user@noritersand-desktop MINGW64 /c/dev/repo/git-test (test4)
$ git merge main
Auto-merging CONFLICT_ME.txt
CONFLICT (content): Merge conflict in CONFLICT_ME.txt
Automatic merge failed; fix conflicts and then commit the result.

user@noritersand-desktop MINGW64 /c/dev/repo/git-test (test4|MERGING)
$ git diff --check
CONFLICT_ME.txt:2: leftover conflict marker
CONFLICT_ME.txt:4: leftover conflict marker
CONFLICT_ME.txt:6: leftover conflict marker

user@noritersand-desktop MINGW64 /c/dev/repo/git-test (test4|MERGING)
$ git diff
diff --cc CONFLICT_ME.txt
index 6494d80,862c24c..0000000
--- a/CONFLICT_ME.txt
+++ b/CONFLICT_ME.txt
@@@ -1,2 -1,2 +1,6 @@@
  Hello world!
- 1111
 -3333
++<<<<<<< HEAD
++1111
++=======
++3333
++>>>>>>> main
```

## [diff-tree](https://git-scm.com/docs/git-diff-tree)

'두 개의 트리 개체를 통해 블롭의 내용과 모드를 비교'한다고 한다. (뭔 소린지...)  
대충 커밋끼리 파일 단위로 어떻게 다른지 표시해주는 명령어라고 보면 됨.

```bash
git diff-tree -p feature  # 현재 브랜치 기준으로 feature 브랜치의 차이점 출력
git diff-tree -p feature master  # 현재 feature 브랜치 기준으로 master 브랜치의 차이점 출력
```

TODO 현재 브랜치랑 비교할 때 `HEAD`를 명시한 것과 아닌 것의 결과가 다른데 왜 그런지 몲. 😒

#### options

- `-m`: `diff-tree`는 기본적으로 머지 커밋과의 차이점은 생략하는데, 이 옵션을 지정하면 생략하지 않음
- `-p` `-u` `--patch`: 패치로 사용할 수 있는 내용을 출력한다.
- `-s` `--no-patch`: 패치 내용을 출력하지 않게 한다.
- `--pretty[=<format>]` `--format=<format>`

## [fetch](https://git-scm.com/docs/git-fetch)

리모트 저장소의 데이터를 로컬 저장소로 다운로드한다. 옵션 지정이 없을 경우, 서버의 데이터를 모두 가져오지만 로컬 브랜치로의 머지는 생략한다.

```bash
git fetch  # origin 저장소에서 모든 브랜치의 데이터 다운로드
git fetch pb  # pb 저장소에서 데이터 다운로드
git fetch --all  # 모든 리모트 저장소에서 fetch
```

#### 태그 받아오기

리모트 저장소의 태그를 모두 받아온다. 태그'만' 받는다.

```bash
git fetch --tags
```

#### 브랜치 전환 없이 merge

Fast-forward 머지에 한해 가능한 방법이다.

```bash
git fetch . foo:master # 로컬 브랜치 foo를 로컬 브랜치 master로 머지
git fetch origin foo:foo # 리모트 브랜치 foo를 로컬 브랜치 foo로 머지
```

`:` 기준 좌측이 가져올 브랜치다. 로컬 브랜치면 `.`을, 리모트 브랜치면 리모트 이름을 입력한다. 우측은 당연히 로컬 브랜치이므로 그런거 없어도 됨.

## [format-patch](https://git-scm.com/docs/git-format-patch)

메일 전송용으로 규격화된 패치 만들기. **주의: 머지 커밋은 패치 대상에서 제외됨**.  
커밋 단위로 패치 파일이 생성되므로 여러 커밋을 하나로 묶고 싶으면 스쿼시를 먼저 해야 한다.

```bash
git format-patch -3  # 현재 브랜치에서 3개의 커밋을 패치로 생성(HEAD 포함 3개)
git format-patch 브랜치_커밋아이디_혹은_태그  # 현재 브랜치부터 지정한 커밋까지의 커밋을 패치로 생성(지정한 커밋은 포함하지 않음)
git diff > FILE  # diff 출력 내용을 파일로 생성. 검색에서 나오긴 했는데 apply가 안되는건 함정...
```

#### 생성한 패치 적용하기

`format-patch`는 파일명 앞에 커밋 순서대로 번호를 붙여주니 오름차순으로 적용하면 된다.

```bash
git apply FILE
git am FILE
```

#### am과 apply의 차이

패치를 적용하는 명령어 중 `am`은 `format-patch`로 생성된 패치만 적용할 수 있고 커밋이 바로 생성된다.  
`apply`는 `format-patch` 포함 `diff`로 생성한 패치도 적용할 수 있다고 하며(된다는데난외않되😑), 커밋을 생성하지 않는다.

## [gc](https://git-scm.com/docs/git-gc)

오래된 이력과 레퍼런스를 정리, 압축해서 저장소를 최적화하는 명령어

```bash
git gc --aggressive
git gc --prune=now
```

gc 중 다음과 같은 메시지가 나타날 수 있는데:

```bash
PS C:\dev\git\noritersand.github.io> git gc --prune=now
Enumerating objects: 396731, done.
Counting objects: 100% (396731/396731), done.
Delta compression using up to 8 threads
Compressing objects: 100% (110001/110001), done.
Writing objects: 100% (396731/396731), done.
Total 396731 (delta 254114), reused 396729 (delta 254112), pack-reused 0
Unlink of file '.git/objects/pack/pack-08670f85649525b5541e3f6725eca14532346f6b.pack' failed. Should I try again? (y/n)
```

여기선 그냥 'N'을 입력해주면 된다. 하지만 이게 어쩔 땐 한도 끝도 없이 나올 때가 있다. 이런 경우:

```bash
# 파워쉘
$env:GIT_ASK_YESNO = 'false'

# git bash
export GIT_ASK_YESNO=false
```

이렇게 환경 변수를 설정해놓고 돌리면 된다. [관련 글](https://stackoverflow.com/questions/4389833/unlink-of-file-failed-should-i-try-again)

#### options

- `--aggressive`: 더 많은 시간을 들여서 '공격적'으로 최적화하는 옵션이라고 함.
- `--auto`
- `--prune=<date>`: 지정한 날짜보다 오래된 객체 중 연결이 끊긴 객체를 정리한다. 날짜를 지정하지 않으면 2주가 기본값이며, `gc` 명령을 옵션없이 실행했을 때의 기본 옵션이다.
- `--no-prune`
- `--quiet`
- `--force`
- `--keep-largest-pack`

## [gitk](https://git-scm.com/docs/gitk)

```
gitk [<options>] [<revision range>] [--] [<path>…​]
```

커밋 이력를 보여주는 GUI 툴. `git log`와 동일한 [옵션](https://noritersand.github.io/git/git-%EB%AA%85%EB%A0%B9%EC%96%B4-%EC%A0%95%EB%A6%AC-git-commands/#heading-options-4)을 사용할 수 있다.

```bash
gitk [git log options]
gitk # HEAD의 커밋 이력 보기
gitk --all # 저장소의 모든 커밋 이력 보기
gitk main draft/noritersand legacy # main, draft/noritersand, legacy 브랜치의 커밋 이력 보기
```

![](/images/gitk.png)

## help

도움말 보기

```bash
git help config
git config --help
git help -a # 모든 명령어 보기
```

## init

#### 디렉터리를 git 저장소로 만들기

```bash
git init
```

#### bare repository

워킹 트리가 없는 저장소를 만든다. 이 명령은 로컬 저장소가 아닌 리모트 저장소를 생성할 때 사용한다.

```bash
git init --bare
```

## [log](https://git-scm.com/docs/git-log)

```
git log [<options>] [<revision range>] [[--] <path>…​]
```

커밋 이력을 조회하는 명령어.

#### 단순 조회

```bash
git log # 현재 브랜치의 이력 조회
git log hotfix123 # hotfix123 브랜치 이력 조회
git log master origin/master # 로컬의 master와 origin remote의 master 브랜치 이력 조회
```

#### options

- `--all`: 헤드와 모든 refs의 이력을 출력함. 즉, 헤드 + 추적 중인 브랜치 + 태그의 이력이다.
- `-p`: 각 커밋에 적용된 패치<sup>patch</sup>(=반영된 변경사항)를 보여준다.
- `-c`: 머지 커밋과 부모 커밋을 비교해 변경사항을 모두 출력하는 옵션
- `--merges`: 머지 커밋만 출력.
- `--no-merges`: 머지 커밋이 아닌 것만 출력한다.
- `--min-parents=<number>` `--max-parents=<number>`: 부모 커밋이 최소(혹은 최대) 몇 개가 있는 커밋인지를 특정할 때 사용하는 옵션이다. 가령 `--max-parents=1`은 부모 커밋이 하나인 커밋만 출력하라는 의미라 `--no-merges`와 같고, `--min-parents=2`는 부모 커밋이 최소 2개여야 한다는 의미니까 `--merges`와 같다.
- `--no-min-parents` `--no-max-parents`: 기본값을 무시할 때 사용한다. `--no-min-parents`는 `--min-parents=0`과 같은데 모든 커밋을 출력하라는 옵션이다. `--no-max-parents`는 `--max-parents=-1`과 같고(음수 1 임) 최대 부모 커밋의 제한을 해제한다는 의미다.
- `--stat`: 각 커밋에서 수정된 파일의 통계정보를 보여준다.
- `--shortstat`: `--stat` 옵션의 결과 중에서 수정한 파일, 추가된 줄, 삭제된 줄만 보여준다.
- `--name-only`: 커밋 정보중에서 수정된 파일의 목록만 보여준다.
- `--name-status`: 수정된 파일의 목록을 보여줄 뿐만 아니라 파일을 추가한 것인지, 수정한 것인지, 삭제한 것인지도 보여준다.
- `--abbrev-commit`: 40자 짜리 SHA-1 체크섬을 전부 보여주는 것이 아니라 처음 몇 자만 보여준다.
- `--relative-date`: 정확한 시간을 보여주는 것이 아니라 '2주 전'처럼 상대적인 형식으로 보여준다.
- `--graph`: 브랜치와 머지 히스토리 정보까지 아스키 그래프로 보여준다.
- `--pretty[=<format>]` `--format=<format>`: 지정한 형식으로 보여준다. 이 옵션에는 `oneline`, `short`, `full`, `fuller`, `format`이 있다. `format`은 원하는 형식으로 출력하고자 할 때 사용한다.
- `--walk-reflogs`: 헤드가 이동한 순서대로 로그 출력

```bash
git log -p -2  # 패치내용을 출력하되 2개의 이력만 표시
git log --pretty=oneline  # 각 커밋들의 메시지와 체크섬만 한 줄씩 출력된다.
git log -1 HEAD~3  # 헤드 기준 세번째 전의 커밋 로그 보기
git log v1.0 v2.4  # v1.0 태그에서 v2.4 태그 사이의 로그 보기
git log --oneline --decorate --graph --all  # 현재 브랜치의 모든 커밋 로그를 그래프로 보기
```

#### pretty=format의 placeholder

- `%H`: Commit hash
- `%h`: Abbreviated commit hash
- `%T`: Tree hash
- `%t`: Abbreviated tree hash
- `%P`: Parent hashes
- `%p`: Abbreviated parent hashes
- `%an`: Author name
- `%ae`: Author e-mail
- `%ad`: Author date (format respects the --date= option)
- `%ar`: Author date, relative
- `%cn`: Committer name
- `%ce`: Committer email
- `%cd`: Committer date
- `%cr`: Committer date, relative
- `%s`: Subject

```bash
git log --pretty=format:"%h %s" --graph
```

[Pro Git book: 더 많은 옵션](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%BB%A4%EB%B0%8B-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EC%A1%B0%ED%9A%8C%ED%95%98%EA%B8%B0)

#### 조회 범위 제한 옵션

- `-(n)`: 최근 n 개의 커밋만 조회한다.
- `--since`, `--after`: 명시한 날짜 이후의 커밋만 검색한다.
- `--until`, `--before`: 명시한 날짜 이전의 커밋만 조회한다.
- `--author`: 입력한 저자의 커밋만 보여준다.
- `--committer`: 입력한 커미터의 커밋만 보여준다.

```bash
git log --since=2.weeks
git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \ --before="2008-11-01" --no-merges -- t/
```

## ls-files

스테이징 에어리어나 워킹 트리에 있는 파일들의 정보를 표시한다. 기본적으로 명령을 실행한 경로를 기준으로 재귀탐색한다.

#### options

- `-c` `--cached`
- `-d` `--deleted`
- `-m` `--modified`
- `-o` `--others`
- `-i` `--ignored`
- `-s` `--stage`
- `-v`: 파일의 상태를 표시하되 '실제로는 변경되었으나 그렇지 않은것으로 간주된<sup>assumed unchanged</sup>' 파일은 소문자로 표시한다.

```bash
git ls-files -v | grep ^h  # assumed unchanged 파일만 표시
```

#### 파일 상태

- `H`: cached
- `S`: skip-worktree
- `M`: unmerged
- `R`: removed/deleted
- `C`: modified/changed
- `K`: to be killed
- `?`: other

## [ls-remote](https://git-scm.com/docs/git-ls-remote)

저장소의 정보를 조회하는 명령어. 옵션을 명시하지 않으면 연결된 저장소의 태그, 머지 리퀘스트, 브랜치 등을 모두 조회한다.

```bash
# 헤드만 보기
git ls-remote --heads

# 특정 저장소의 헤드만 보기
git ls-remote -h https://github.com/noritersand/noritersand.github.io
```

#### options

- `-h` `--heads`: 헤드만 출력, 즉 브랜치만 보는 옵션.
- `-t` `--tags `: 태그만 출력한다.
- `--refs `: 'peeled tags'와 HEAD 같은 'pseudorefs'를 제외하고 출력한다. ~~peeled tags는 저도 모르니 묻지 마십씨오~~

## [merge](https://git-scm.com/docs/git-merge)

현재 브랜치에 다른 브랜치를 머지한다. 만약 충돌<sup>conflict</sup>이 발생하면 깃은 자동으로 머지를 중단하고 충돌이 발생한 파일에 각 커밋의 내용을 표시해준다.

깃의 머지는 두 개의 부모 커밋을 가리키는 특별한 커밋을 만들어 낸다. 두 개의 부모가 있는 커밋은 '한 부모의 모든 작업과 나머지 부모의 모든 작업, 그리고 그 두 부모의 모든 부모들의 작업을 포함한다'라는 의미가 있다. [^1]

```bash
git merge 브랜치1 [브랜치2 브랜치3 브랜치3 ...]
git merge iss123  # 헤드 브랜치(현재 브랜치)에 iss123 브랜치를 머지
git merge iss123 hotfix  # 헤드 브랜치에 iss123과 hotfix를 머지
```

나열되는 브랜치들은 현재 헤드가 가리키는 브랜치에 어떤 브랜치들을 머지할 것인지를 의미한다. 그래서 다음의 경우:

```bash
git merge A B
```

현재 브랜치<sup>current branch</sup>에 A 브랜치와 B 브랜치를 머지한다.

#### options

- `--ff`: 가능할 경우 Fast-forward 머지를, 안되면 머지 커밋을 생성한다.
- `--no-ff`: 묻지도 따지지도 않고 머지 커밋을 생성한다.
- `--ff-only`: Fast-forward 머지가 불가능할 경우 머지를 취소한다.
- `--no-commit`: 커밋하지 않고 머지. 파일이 변경된 상태에서 머지를 멈춘다고 보면 됨.
- `--abort`: 머지 취소. 충돌 상태일 때 `merge` 명령을 실행하기 전으로 되돌린다.
- `--autostash`, `--no-autostash`: 스태싱을 자동으로 하거나 하지 않음. `MERGE_AUTOSTASH`라는 참조를 사용한다.

#### 머지 도구 실행

```bash
git mergetool
```

머지 도구로 지정된 앱을 실행한다. 이때 `_BACKUP`, `_LOCAL` 등의 이름이 붙은 백업파일들이 자동 생성된다.

#### fetch 후 머지

fetch로 받아온 데이터를 현재 브랜치와 머지한다.

```bash
git fetch
git merge FETCH_HEAD
```

주의: `FETCH_HEAD`는 `fetch` 명령을 실행한 시점에 체크아웃하고 있던 브랜치 기준으로 만들어진다. 따라서 `FETCH_HEAD`를 머지할 땐 항상 `fetch`를 먼저 할 것.

#### 여러 커밋을 하나로 묶어서 머지: 스쿼시

```bash
git merge --squash TARGET_BRANCH
```

머지할 브랜치의 커밋 이력을 하나로 압축한 별도의 커밋을 만들고 헤드 브랜치에 머지한다. 일반 머지와 다르게 하나의 부모커밋(헤드 브랜치 기준)만 갖는다.

[Git: 커밋 합치기](https://noritersand.github.io/git/2018/12/28/git-%EC%BB%A4%EB%B0%8B-%ED%95%A9%EC%B9%98%EA%B8%B0/)

## mv

파일 이동 혹은 파일명 변경

```bash
git mv FILE_FROM FILE_TO
```

## [pull](https://git-scm.com/docs/git-pull)

fetch 후 자동 머지.

```bash
git pull [리모트저장소] [브랜치]
```

리모트 저장소에서 데이터를 받아온다는 것은 `fetch`와 같지만 `pull`은 머지까지 한다는 점이 다르다.

```bash
git pull  # 업스트림 브랜치를 현재 브랜치로 pull
git pull origin master  # origin 저장소의 master 브랜치를 현재 브랜치로 pull
git pull --rebase  # fetch 후 머지 대신 리베이스
```

명시한 리모트 브랜치를 현재 브랜치로 pull 한다는 점에 주의할 것. 브랜치를 명시하지 않으면 업스트림 브랜치를 현재 브랜치에 자동으로 머지한다. 옵션을 따로 지정하지 않았다면, 가능한 경우 FF 머지를 시도한다.

## [push](https://git-scm.com/docs/git-push)

로컬 저장소의 데이터를 리모트 저장소에 업로드한다.

```bash
git push [리모트저장소] [브랜치]
git push  # origin 리모트 저장소에 현재 브랜치를 업로드
git push origin other  # origin에 other 브랜치 업로드. 리모트에 other 브랜치가 없으면 새로 생성한다.
```

#### 업스트림 브랜치 설정 \#3

로컬 저장소를 `init`으로 생성했거나 로컬에서 새로 생성한 브랜치일 때, `push`와 동시에 업스트림 브랜치를 설정하는 방법이다.

```bash
git push --set-upstream origin master
Branch master set up to track remote branch master from origin.
Everything up-to-date
```

#### 브랜치 전환 없이 push

```
git push 리모트저장소 로컬브랜치:서버브랜치
```

`push` 하려는 브랜치로 전환하기 귀찮거나, 로컬 브랜치의 이름과 리모트 브랜치의 이름이 다를 때 사용한다:

```bash
git push origin hotfix # 로컬 브랜치인 hotfix를 리모트 hotfix로 push. 만약 리모트에 hotfix 브랜치가 없으면 생성.
git push origin feature:feature # 로컬 feature를 리모트 feature로 push
git push origin serverfix:awesome  # 현재 로컬 브랜치가 serverfix 일때 리모트 브랜치 awesome을 생성하고 push
```

#### 신규 로컬 저장소를 생성하고 리모트에 업로드

```bash
touch README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://noritersand@127.0.0.1:8443/r/test.git
git push --set-upstream origin master
```

#### 로컬 저장소의 데이터 업로드

```bash
git remote add origin https://noritersand@127.0.0.1:8443/r/test.git
git push --set-upstream origin master
```

#### 리모트 저장소의 태그나 브랜치 삭제

```bash
git push origin --delete other  # origin 저장소의 other 브랜치 삭제
git push origin :v0.9  # origin 저장소의 v0.9 태그 삭제(--delete는 콜론으로 대체할 수 있음)
```

#### 생성한 태그 공유

push는 태그를 포함하지 않는다. 따라서 명시적인 명령으로 따로 올려야 한다.

```bash
git push origin v1.5  # origin 저장소에 v1.5 태그 업로드
git push --tags  # 생성한 태그를 모두 업로드
```

## rebase

`rebase`의 기본 기능은 현재 브랜치를 다른 브랜치에 머지하는 것이지만, 옵션을 사용해 여러 커밋을 하나로 합치거나 커밋 메시지를 수정하기도 한다.

#### rebase로 머지

`merge` 명령이 두 브랜치의 최종결과만을 기준으로 머지한다면 리베이스는 브랜치의 변경사항을 순서대로 다른 브랜치에 적용하며 머지한다. 저장소의 커밋 로그와 이력을 한 줄로 정리해주기 때문에 보통 완료된 브랜치를 마스터에 머지할 때 사용한다.

```bash
git rebase master  # 현재 브랜치를 master 브랜치로 리베이스
```

위의 경우 현재 브랜치(HEAD)의 델타를 패치<sup>patch</sup>로 만들어놓고, 현재 브랜치를 master의 마지막 커밋으로 이동한 뒤, 만들어뒀던 패치를 반영하는것과 결과가 같다. (가지를 줄기서부터 잘라 다른 위치에 접붙이는 걸 상상해 보자)

자세한 내용은 아래 링크를 참고:

- [Pro Git book: Git브랜치 Rebase하기](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)
- [Pro Git book: Rebase의 위험성](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0#_rebase_peril)

#### 대화형 리베이스 도구로 여러 커밋 수정

```bash
git rebase -i HEAD~3  # 헤드부터 2회 전 커밋까지를 대화형으로 수정
```

**주의: `git rebase HEAD~N`에서 N은 다른 명령어와 다르게 N회 전 이 아니라 N-1회 전 커밋을 의미한다.**  
예를 들어 `git show HEAD~2`로 보이는 커밋(1)과 `git rebase -i HEAD~2`에서 맨 위에 표시되는 커밋(2)은 다른데, (1)은 (2)의 부모 커밋이다.

```bash
pick d27530d6b (HEAD 기준 2회 전)
pick e770ff0be (HEAD 기준 1회 전)
pick 7715f75fa (HEAD)

# Rebase 02c62e3c3..7715f75fa onto 02c62e3c3 (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified). Use -c <commit> to reword the commit message.
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

[Pro Git book: 커밋 메시지를 여러 개 수정하기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EB%8B%A8%EC%9E%A5%ED%95%98%EA%B8%B0#_changing_multiple)

#### rebase로 커밋 합치기

[누구나 쉽게 이해할 수 있는 Git 입문: rebase -i 로 커밋 모두 통합하기](https://backlog.com/git-tutorial/kr/stepup/stepup7_5.html)

## [reflog](https://git-scm.com/docs/git-reflog)

`log`와 비슷하지만 `log`가 커밋 이력을 출력한다면 `reflog`는 헤드 이동 이력을 출력한다.

```bash
# 마지막 세 번의 헤드 이동 이력과 ISO 형식의 시간을 역순으로 출력
git reflog -3 --date=iso
```

```bash
$ git reflog -5

2fbc899 HEAD@{0}: checkout: moving from master to master
2fbc899 HEAD@{1}: pull: Merge made by the 'recursive' strategy.
7107b9e HEAD@{2}: checkout: moving from d to master
53576ad HEAD@{3}: merge c: Fast-forward
2bc9237 HEAD@{4}: checkout: moving from c to d
```

참고로 `HEAD@{1}`란 표현은 헤드 변경 이력 중 HEAD 기준 바로 직전의 이력을 의미한다. 이 표현은 `merge` 등의 명령에서도 사용할 수 있다.

```bash
git merge --squash HEAD@{1} # 헤드와 헤드의 직전 이력을 스쿼시 머지
```

## remote

#### 리모트 저장소 목록

로컬 저장소에 등록된 리모트 저장소를 모두 출력하되 단축이름만 표시한다.

```bash
git remote
```

#### 리모트 저장소 URL 확인

리모트 저장소의 단축이름과 URL을 표시한다.

```bash
git remote -v
```

#### 리모트 저장소 URL 바꾸기

```bash
git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
```

#### 리모트 저장소 추가

```bash
git remote add 단축이름 주소
git remote add remoteRepo https://noritersand@dev.naver.com/git/exlernforscm.git
```

#### 리모트 저장소 살펴보기

특정 로컬 저장소의 구체적인 정보(URL, 추적중인 브랜치 등)를 표시한다.

```bash
git remote show origin  # origin 저장소의 상세정보 표시
```

#### 리모트 저장소 이름 바꾸기

```bash
git remote rename NAME_FROM NAME_TO
```

#### 리모트 저장소 삭제

```bash
git remote rm 리모트저장소이름
```

#### 리모트 저장소에서 삭제된 브랜치를 로컬의 리모트 트래킹 브랜치 목록에서 제거

```bash
git prune
git fetch --prune
git remote prune 리모트저장소
```

셋의 차이는 [여기](https://stackoverflow.com/questions/20106712/what-are-the-differences-between-git-remote-prune-git-prune-git-fetch-prune)를 참고.
어쨋든 이 명령은 로컬을 정리하는 것이지, 리모트의 뭔가를 지우는게 아니니 안심하고 실행해도 된다.

## reset

헤드를 이동한다.

#### staged 되돌리기(스테이징 취소)

`add`로 스테이징 에어리어에 등록한 파일을 unstaged 상태로 바꾼다.

```bash
git reset HEAD 파일 # 특정 파일만 스테이징 취소
git reset HEAD -- # 모든 파일 스테이징 취소
```

#### 커밋 되돌리기 \#1

체크섬이나 `HEAD~숫자`를 사용해서 헤드가 지난 커밋을 가리키게 할 수 있다. 사실 이건 되돌리기보단 지난 커밋으로 이동한다고 보는게 더 정확한 표현이다.

#### options

- `--soft`: 헤드만 옮긴다. 스테이징 에어리어와 워킹 트리는 유지
- `--mixed`: 명시하지 않을때의 기본값. 헤드와 스테이징 에어리어를 특정 커밋으로 변경한다. 워킹 트리는 유지
- `--hard`: 헤드와 스테이징 에어리어, 워킹 트리를 모두 특정 커밋으로 변경한다.

```bash
git reset --soft HEAD~2  # 헤드만 2회 전 커밋으로 이동
git reset --hard 4990ef  # 헤드를 4990ef 체크섬으로 이동하고 스테이징 에어리어, 워킹 트리를 헤드와 동일하게 변경
```

## [restore](https://git-scm.com/docs/git-restore)

2.23 버전에서 `switch`와 함께 새로 나온 명령어. 워킹 트리 혹은 스테이징 에어리어를 되돌린다. new file은 되돌리지 않는다.

```bash
git restore . # 워킹 트리의 모든 파일을 되돌림
git restore -S -W . # 워킹 트리와 인덱스의 모든 파일을 되돌림
```

`-W`와 `-S`는 워킹 트리와 스테이징 에어리어 중 어느쪽을 되돌릴지 지정하는 옵션인데, 만약 둘 다 되돌리고 싶다면 그냥 둘 다 쓰면 된다.

#### options

- `-s <tree>` `--source=<tree>`
- `-p` `--patch`
- `-W` `--worktree`: 워킹 트리만 되돌린다. 지정하지 않았을 때의 기본값이다.
- `-S` `--staged`: 스테이징 에어리어만 되돌린다.
- `-q` `--quiet`
- `--progress`
- `--no-progress`
- `--ours`
- `--theirs`
- `-m` `--merge`
- `--conflict=<style>`
- `--ignore-unmerged`
- `--ignore-skip-worktree-bits`
- `--recurse-submodules`
- `--no-recurse-submodules`
- `--overlay`
- `--no-overlay`
- `--pathspec-from-file=<file>`
- `--pathspec-file-nul`

## [revert](https://git-scm.com/docs/git-revert)

#### 커밋 되돌리기 \#2

1회 전의 커밋으로 되돌리되 단순히 헤드를 이동하는게 아니라, 되돌려지는 내용을 기록한 **새로운 커밋을 생성**한다.

```bash
git revert HEAD  # 직전 커밋의 revert 커밋 생성
```

위의 경우 헤드 기준으로 가장 마지막 커밋의 변경사항을 되돌리는 커밋을 생성한다.
가령 직전의 커밋이 'fix-a-lot.md'라는 파일을 생성한 커밋이라면 `git revert HEAD`는 'fix-a-lot.md'를 삭제하는 커밋을 만드는 것이다.

```bash
git revert HEAD~1  # 1회 전 커밋의 리버트 커밋 생성
git revert HEAD~3  # 3회 전 커밋의 리버트 커밋 생성
git revert 3bd5055389bd059f0f781bfcfe3190bb7dfa9e5e  # 3bd505 커밋의 리버트 커밋 생성
git revert HEAD~4..HEAD  # HEAD부터 3회 전 커밋까지의 변경사항을 되돌린 커밋 생성
```

`HEAD~4..HEAD`의 경우 오타가 아니라 3회 전 커밋까지가 맞다. 그런데 `HEAD~4`까지만 쓰면 4회전 커밋이다. 🤔

#### options

- `e` `--edit`
- `-n` `--no-commit`: 워킹 트리와 스테이징 에어리어의 상태만 되돌리고 커밋은 생성하지 않는다.
- `-m parent-number` `--mainline parent-number`: 머지 커밋을 리버트 할 때 사용하는 옵션.

#### 머지 커밋을 되돌리기

```bash
commit fc30cfe9255837f6810eb75adacaf93696828afe (HEAD -> revert-test)
Merge: 7be20ca ca433d3
# 7be20ca에 ca433d3을 머지하여 만들어진 커밋 fc30cf를 조회한 로그
```

머지 커밋을 되돌릴 때는 깃이 어느 부모가 메인라인(되돌리지 않을 커밋)인지 알 수 있도록 `-m` 옵션과 함께 부모 번호(1부터 시작하는 부모 커밋을 가리키는 숫자)를 입력해줘야 한다:

```bash
git revert fc30cf -m 1
```

여기서 1이란 `git log`에서 보이는 부모 커밋 중 가장 왼쪽을 의미한다. 위 예시에선 `7be20ca`가 메인라인이 되며 `ca433d3`의 변경사항이 되돌려진다.

## rm

#### 파일/폴더 삭제

```bash
git rm readme.txt
```

rm 명령어는 깃이 추적중인 파일 혹은 폴더에만 사용할 수 있다.

#### 깃의 추적을 중단시키기

실제 파일은 남기고 깃의 관리 대상에서만 제외한다.

```bash
git rm --cached readme.txt
```

#### file-glob 패턴으로 범위삭제

```bash
git rm log/\*.log  # log/디렉터리의 확장명이 log인 파일 모두 삭제
git rm \*.~  # ~로 끝나는 파일 모두 삭제
```

## [show](https://git-scm.com/docs/git-show)

커밋 정보 조회

```bash
git show HEAD  # 헤드 브랜치의 커밋 정보 조회
git show v1.1  # v1.1 태그의 커밋 정보 조회
git show 1c002dd4b536e7479fe34593e72e6c6c1819e53b  # 체크섬으로 조회
git show 1c002dd4b  # 체크섬은 중복이 없는한 앞의 일부분만 명시해도 인식한다.
```

#### options

옵션은 [diff-tree](https://git-scm.com/docs/git-diff-tree)와 동일하게 사용할 수 있다고 한다. 따라서 여기에는 자주 쓰는 옵션만 적는다(뭐 다 그렇게 했지만):

- `-p`: 패치 형태로 출력
- `-c`: `log`와 마찬가지로 머지 커밋은 해당 커밋에 포함된 파일 목록만 출력하는게 기본값인데, 이 옵션을 사용하면 머지 커밋과 부모 커밋을 비교해 변경사항을 모두 출력한다.

## [stash](https://git-scm.com/docs/git-stash)

커밋이나 스테이지가 아닌 별도의 공간에 변경사항을 임시 저장하거나 저장한 내용을 다시 불러오는 명령어.

#### options

- `-a` `--all`: 무시 대상이거나 추적중이지 않은 파일도 대상으로 포함하고 `git clean` 실행.
- `-u` `--include-untracked`
- `--no-include-untracked `
- `--only-untracked `: 추적중이지 않은 파일을 대상으로 포함하거나 포함하지 않는다. 혹은 추적중이지 않는 파일만 대상으로 한다.
- `-k` `--keep-index`
- `--no-keep-index`: This option is only valid for push and save commands. All changes already added to the index are left intact.
- `-p` `--patch`: 스태시 대상을 대화형으로 선택.

#### 스태시 생성(임시 저장본 만들기)

추적 중인 파일의 모든 변경사항을 스태시에 저장되며 워킹 트리와 스테이징 에어리어는 헤드와 같아진다.

```bash
git stash  # 스태시 생성. stash save와 같음
git stash save
git stash save '스태시 이름'
git stash -k  # staged 상태의 파일은 무시하고 스테시에 저장
git stash -u  # --include-untracked: 추적중이지 않은 파일도 스태시로 저장
```

#### 스태시 확인

```bash
git stash list  # 스태시 목록 확인
git stash show  # 첫 번째 스태시 상세 확인
git stash show stash@{0}  # 리눅스에서만 됨
git stash show 'stash@{0}'  # 윈도우 파워쉘은 이렇게
git stash show 0  # git stash show stash@{0}과 같음
```

#### 스태시 적용(임시 저장본 불러오기)

현재 브랜치에 스태시에 저장된 내용을 적용한다.

```bash
git stash apply  # 가장 최근의 스태시를 현재 브랜치에 적용
git stash pop  # 스태시를 적용하고 스택에서 삭제
```

`apply`는 스태시를 적용하되 저장한 스태시는 그대로 유지한다. 반면 `pop`은 적용한 스태시를 스택에서 삭제한다. 스태시는 기본적으로 modified 상태로 적용된다. `--index` 옵션을 사용하면 스태시를 저장할 때 staged 상태였던 파일을 다시 staged 상태로 만들어준다.

```bash
git stash pop --index
```

#### 스태시 삭제

```bash
git stash drop  # 첫 번째 스태시 삭제
git stash drop 3  # 네 번째 스태시 삭제
git stash clear  # 모든 스태시 삭제
```

#### 스태시를 적용한 새 브랜치 만들기

```bash
git stash branch BRANCH_NAME  # 가장 최근의 스태시를 적용한 새 브랜치 생성
git stash branch issue541 stash@{1}  # 두 번째 스태시를 적용한 issue541 브랜치 생성
```

## status

저장소 상태 확인

```bash
git status
```

## [switch](https://git-scm.com/docs/git-switch)

```
git switch [<options>] [--no-guess] <branch>
git switch [<options>] --detach [<start-point>]
git switch [<options>] (-c|-C) <new-branch> [<start-point>]
git switch [<options>] --orphan <new-branch>
```

2.23 버전에서 새로 나온 명령어. checkout의 브랜치 전환 기능을 분리한 명령어다.

브랜치를 생성하며 전환하는 옵션일 때, `<start-point>`를 명시하지 않으면 현재 브랜치를 기반으로 신규 브랜치를 생성한다. 만약 `<start-point>`가 리모트의 브랜치인 경우 `<start-point>`를 업스트림 브랜치로 설정한다.

```bash
git switch hotfix0401 # hotfix0401 브랜치로 전환
git switch -c new-me # 현재 브랜치에서 new-me 브랜치 생성하며 전환
git switch -c release origin/release # origin/release에서 release 브랜치를 생성하며 전환하고, 업스트림 브랜치로 설정.
git switch -c abc 4d0dc05b1f # 4d0dc05b1f 커밋에서 abc 브랜치 생성하며 전환
```

#### options

- `-c` `--create <new-branch>`
- `-C` `--force-create <new-branch>`
- `-d` `--detach`
- `--guess` `--no-guess`
- `-f` `--force`
- `--discard-changes`
- `-m` `--merge`
- `--conflict=<style>``
- `-q` `--quiet`
- `--progress`
- `--no-progress`
- `-t` `--track`
- `--no-track`
- `--orphan <new-branch>``
- `--ignore-other-worktrees`
- `--recurse-submodules`
- `--no-recurse-submodules`

## svn

#### SVN 저장소 복제

```bash
git svn clone SVN저장소주소
```

#### 표준 레이아웃 SVN 저장소 복제

표준 레이아웃(trunk와 branches, tags 폴더가 같은 위치에 있는 구조)을 사용하는 SVN 저장소를 복제할 때 사용한다.

```bash
git svn clone -s SVN저장소주소
```

#### 비표준 레이아웃 SVN 저장소 복제

trunk와 branches, tags 폴더의 위치를 각각 지정하는 방식이다.

```bash
git svn clone -T 트렁크경로\
-b 브랜치경로\
-t 태그경로\
svn저장소
```

#### 표준 레이아웃 SVN 저장소의 특정 리비전 복제

```bash
git svn clone -s -r 2321
```

#### 표준 레이아웃을 사용하는 SVN 저장소를 복제하고 모든 리모트 브랜치에 접두어 추가하기

```bash
git svn clone -s --prefix svn/ svn저장소
```

#### 상위의 SVN 저장소에서 갱신하고 재정렬하기

```bash
git svn rebase
```

#### 상위의 SVN 저장소에 커밋데이터 올리기

```bash
git svn dcommit
```

#### 상위 SVN 저장소에 푸싱될 커밋 목록 보기

```bash
git svn dcommit -n
```

#### SVN 저장소 로그 보기

```bash
git svn log
```

#### SVN 저장소 파일의 커밋정보 줄단위로 보기

```bash
git svn blame 파일
```

## tag

태그 조회와 생성/삭제 명령어

```bash
git tag
```

#### lightweight 태그 만들기

특정 커밋지점의 포인터를 생성(책갈피와 비슷한 개념)

```bash
git tag v2.2  # 현재 브랜치의 마지막 커밋에 v2.2 태그 생성
```

#### annotated 태그 만들기

이름, 이메일, 날짜, 메시지를 저장하는 태그를 생성함

```bash
git tag -a v1.1 -m "my version 1.1"
```

#### 특정 커밋에 태그 만들기

체크섬을 알고 있다면 예전 커밋에도 태그할 수 있다.

```bash
git tag v0.8 9fceb02
```

#### 태그에 서명

GPG(GNU Privacy Guard) 개인키로 태그에 서명

```bash
git tag -s 태그명 [-m "태그메시지"]
git tag -s v1.5 -m "my signed 1.5 tag"
```

#### 태그 서명 검증

태그서명에 사용된 키가 공개키인지 검증한다.

```bash
git tag -v v1.0
```

#### 태그 삭제

```bash
git tag -d v0.9
```

## update-index

파일을 인덱스(=스테이징 에어리어)에 등록하거나 수정한다.

#### options

- `--add`
- `--remove`
- `--refresh`: 워킹트리에 대한 인덱스를 갱신한다.
- `--really-refresh`: `--refresh`와 비슷하지만, 이 옵션은 '변경되지 않음' 상태인 파일도 갱신한다.
- `--assume-unchanged`: 특정 파일을 변경되지 않은 것으로 간주한다. 이후 해당 파일은 스테이지 목록에 나타나지 않는다.
- `--no-assume-unchanged`: '변경되지 않음' 상태의 파일을 되돌린다.

#### 변경된 파일을 스테이지 대상 목록<sup>Unstaged Changes</sup>에서 제외하기

```bash
git update-index --assume-unchanged IGNORE_ME  # IGNORE_ME 파일을 변경되지 않은 것으로 간주함.
```

#### '변경되지 않음' 되돌리기 \#1

assumed unchanged 상태의 파일을 되돌림.

```bash
git update-index --no-assume-unchanged IGNORE_ME
```

#### '변경되지 않음' 되돌리기 \#2

```bash
git update-index --really-refresh
```

[^1]: [https://learngitbranching.js.org/](https://learngitbranching.js.org/)
