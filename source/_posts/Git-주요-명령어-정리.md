---
title: 'Git: 주요 명령어 정리'
categories:
  - git
date: 2018-03-21 10:16:45
tags:
  - git
---

#### 참고한 글
- https://git-scm.com/book/ko/v2
- http://learnbranch.urigit.com/

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

## branch

#### 브랜치 생성
현재 브랜치 기반의 신규 브랜치를 생성한다.
```bash
git branch mybranch
```

#### 다른 커밋 기반의 브랜치 생성
'체크아웃으로 헤드 이동 후 브랜치 생성'의 단축형. 여기서 커밋은 체크섬 외에 다른 브랜치나 태그가 올 수도 있다.
```
git branch 브랜치명 커밋
```

#### 브랜치 확인
```bash
git branch  # 로컬 저장소의 브랜치만 출력
git branch -r  # 리모트 브랜치 목록 보기
git branch -a  # 로컬과 리모트 브랜치 모두 보기
git branch -v  # 마지막 커밋 메시지도 함께 출력한다
git branch -vv  # 추적중인 브랜치 확인
```

#### 머지 여부 확인
머지가 완료되었거나 그렇지 않은 브랜치만 표시한다. 삭제해도 되는 브랜치를 조회할 때 사용한다.
```bash
git branch --merged
git branch --no-merged
```

#### 현재 브랜치를 다른 브랜치에 덮어쓰기
새 브랜치를 생성할 때 -f 옵션을 사용하면 이미 존재하는 브랜치를 무시하고 덮어쓴다.
```
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

## blame
바보같은 커밋을 비난하기 위한 명령어 데이터의 각 줄을 누가 언제 마지막으로 고쳤는지 확인할 수 있으며 디버깅 용도로 사용한다. [Git으로 버그 찾기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Git%EC%9C%BC%EB%A1%9C-%EB%B2%84%EA%B7%B8-%EC%B0%BE%EA%B8%B0)

#### 파일 커밋 정보 줄 단위로 보기
```
git blame 파일
```

#### 특정 라인만 보기
```
git blame -L 시작라인,종료라인 파일
```
```bash
git blame -L 12,22 simplegit.rb
```

#### 파일의 줄 단위의 복사, 붙여넣기, 이동 정보 보기
```
git blame -M 파일
```

#### 파일의 줄 단위의 이동과 원본 파일 정보 보기
```
git blame -C -C 파일
```

## checkout

#### 브랜치 이동
다른 브랜치의 마지막 커밋(가장 최근 커밋)으로 헤드만 이동한다. 이 말은 깃 디렉토리만 다른 커밋의 스냅샷으로 변경된다는 뜻이다. 워킹 트리(working tree, 실제 작업공간)와 스테이징 에어리어(staging area, 인덱스)는 그대로 유지된다.
```bash
git checkout master
# 만약 master라는 브랜치가 로컬에 존재하지 않으면 리모트 저장소의 데이터를 체크아웃한다.
```

#### 다른 커밋으로 헤드 이동
특정 커밋의 체크섬이나 태그를 입력해 해당 시점의 스냅샷으로 이동하는 것을 의미한다. 브랜치를 만들지 않고 헤드를 이동할 수 있다.

체크아웃으로 과거의 이력에 해당하는 커밋으로 이동했을 때, 깃은 이를 '분리된 헤드(detached HEAD)' 상태에 있다고 하며 이 상태에서 바로 작업하는 것보다 브랜치를 생성하여 작업할 것을 권장하고 있다.
```
git checkout 체크섬
git checkout 태그
```
```bash
git checkout 6f9021d4a03586a787ebcef2f94dd2eca1aec941
git checkout v0.3.1.5
```

#### 브랜치를 새로 만들면서 체크아웃
```
git checkout -b 브랜치
```

#### 파일 변경 사항 되돌리기
워킹 트리와 스테이징 에어리어를 깃 디렉토리의 내용대로 되돌린다. 이미 추적중인 대상만 되돌릴 수 있다. status에 표시되는 기준으로 modified, renamed, deleted는 되돌리지만 new file은 되돌리지 않는다.
```bash
git checkout -- .  # 현재 경로의 모든 파일 되돌리기
git checkout HEAD .  # 현재 경로의 모든 파일 되돌리기
```

#### fetch 시점으로 헤드 이동
fetch로 리모트 저장소의 데이터를 받아왔을때 "FETCH_HEAD"라는 포인터가 생성된다. 해당 포인터의 스냅샷을 확인 후 원하는 브랜치로 이동해 머지한다.
```
git checkout FETCH_HEAD
git checkout 리모트저장소/리모트브랜치
```

#### fetch로 받아온 데이터를 머지하지 않고 새 브랜치로 생성
```
git checkout -b FETCH_HEAD
git checkout -b 생성할브랜치 리모트저장소/리모트브랜치
```
```bash
git checkout -b hotFix anotherServer/master  # 이 명령은 지정한 원격 저장소의 브랜치를 업스트림 브랜치로 만든다.
```

#### 업스트림(upstream) 브랜치 설정 #1
깃이 리모트 저장소의 특정 브랜치를 추적하도록 설정하는 방법이다. 추적 중인 브랜치를 업스트림 브랜치라고 하는데, 업스트림 브랜치가 설정되어 있으면 pull이나 push 사용 시 리모트의 저장소명과 브랜치명을 생략할 수 있다.
```
git checkout -b 로컬브랜치 리모트저장소/리모트브랜치
git checkout --track 리모트저장소/리모트브랜치 # 1.6.2 이상
```
```bash
git checkout --track origin/serverfix

Branch serverfix set up to track remote branch refs/remotes/origin/serverfix.
Switched to a new branch 'serverfix'
```

#### 태그로 체크아웃
태그(정확히는 태그가 가리키는 커밋) 기반의 브랜치를 생성하고 동시에 체크아웃까지 하는 방법이다.
```bash
git checkout -b version2 v2.0.0  # v2.0.0 기반 브랜치 version2로 체크아웃
```

## cherry-pick
cherry-pick은 커밋 하나만 리베이스하는 것이다. 다음 페이지에서 자세한 사항을 확인할 수 있다. [Git 프로젝트 운영하기](https://git-scm.com/book/ko/v2/%EB%B6%84%EC%82%B0-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C%EC%9D%98-Git-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%EA%B4%80%EB%A6%AC%ED%95%98%EA%B8%B0#_rebase_cherry_pick)

#### 선택 머지
```
git cherry-pick 커밋명
```

#### 커밋하지 않고 선택 머지
```
git cherry-pick -n 커밋명
```

## clean
추적중이지 않은(untracked) 파일 삭제하기.
```bash
git clean -f
git clean -f -d -x  # ignore 설정된 파일을 포함하며 추적중이지 않은 파일과 폴더를 모두 삭제한다.
```
clean.requireForce 설정이 true가 아니면 clean 명령은 항상 `-f`, `-i`, `-n` 옵션 중 하나가 명시되어야 실행된다. 그리고 현재 폴더를 기준으로 하위를 재귀탐색하기 때문에 recursive 옵션은 따로 없다.

**options**:
- `-f` | `--force` 삭제 기본 옵션. 설정에 따라 생략할 수도 있다.
- `-i` | `--interactive` 대화 모드로 삭제
- `-n` | `--dry-run` 지워질 파일 목록 미리보기
- `-d` 폴더도 삭제한다.
- `-x` ignore 룰이 적용된 파일**도** 삭제한다.
- `-X` ignore 룰이 적용된 파일**만** 삭제한다.

## clone

#### 저장소 복제
현재 경로에 로컬 깃 저장소가 될 디렉토리를 만들고 리모트 저장소의 데이터를 모두 받아온다. 디렉토리명을 따로 명시하지 않으면 리모트 저장소의 이름과 동일하게 생성된다.
```bash
git clone ~/Documents/workspace/ex/cal/src
git clone file://c:/users/noritersand/noriterGit/localServer localGitRepo
git clone https://noritersand@dev.naver.com/git/lernforscm.git naverGitRepo
git clone git://github.com/schacon/grit.git gitHubRepo
```

#### 리모트 저장소를 복제하면서 bare repository로 설정
```
git clone --bare 저장소주소 [디렉토리]
```

#### 복제 시 데이터 제한하기
```bash
git clone --depth 200 ~/Documents/work/  # 마지막 200개의 커밋만 복제한다.
```

## commit
변경내용을 확정함. staged 상태인 파일을 깃 디렉토리에 저장한다. 커밋 메시지를 입력받기 위해, 지정되어있는 에디터가 자동으로 실행되며 메시지를 작성하고 에디터를 종료하면 커밋이 완료된다. 이 때 커밋 메시지가 주석(#으로 시작하는 라인)으로만 작성되어 있으면 커밋은 취소된다.
```bash
git commit
```

#### 에디터 없이 커밋
에디터 실행을 생략하고 메시지를 즉시 입력한다.
```bash
git commit -m "hello this is test commit"
```

#### Signed-off-by 추가
커밋 메시지에 Signed-off-by를 추가한다. `-s` 혹은 `--signoff` 옵션을 사용하면 커밋 메시지에 user.name과 user.email이 자동으로 추가된다.
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

#### 자동 스테이징(staging)
`-a` 옵션을 사용하면 add 없이 커밋할 수 있다. 단, 추적 중인 파일만 자동으로 스테이징된다.
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
깃 저장소의 설정을 조회/관리하는 명령어. `--global` 옵션이 없으면 저장소별 설정을 의미한다.

#### 작업자의 이름/이메일 설정
```bash
git config --global user.name "이름"
git config --global user.email "이메일"
```

#### 기본 편집기 설정
```bash
git config --global core.editor 편집기
```

#### Diff 도구 설정
```bash
git config --global merge.tool vimdiff
```

#### 설정 확인
```bash
git config --list
git config -l
git config --global --list
git config core.editor
```

#### 단축어(alias) 만들기
```
git config --global alias.사용할키워드 '명령어'
```
```bash
git config --global alias.unstage 'reset HEAD --'
git config --global alias.visual '!gitk'
git config --global alias.ss 'status'
```

#### 단축어 목록 보기
```bash
git config --get-regexp alias
git config --list | grep alias
```

#### 단축어 삭제
```bash
git config --global --unset alias.ss
```

#### SSL 검증 비활성화
```bash
git config --global http.sslVerify false
```

## diff

#### unstaged와 staged의 비교
```bash
git diff
git diff --check  # 충돌 문자가 있거나 공백 에러가 있는지 확인
```
#### staged와 commited의 비교
```bash
git diff --cached
git diff --staged
```

## fetch
리모트 저장소의 데이터를 로컬 저장소로 다운로드한다. 서버의 데이터를 모두 가져오지만 머지는 생략한다.
```bash
git fetch  # origin 저장소에서 데이터 다운로드
git fetch pb  # pb 저장소에서 데이터 다운로드
git fetch --all  # 모든 리모트 저장소에서 fetch
```

#### 태그 받아오기
리모트 저장소의 태그를 모두 받아온다. 태그'만' 받는다.
```bash
git fetch --tags
```

## help
도움말 보기
```bash
git help config
git config --help
```

## init

#### 디렉토리를 git 저장소로 만들기
```bash
git init
```

#### bare repository
워킹 트리가 없는 저장소를 만든다. 이 명령은 로컬 저장소가 아닌 리모트 저장소를 생성할 때 사용한다.
```bash
git init --bare
```

## log

#### 커밋 히스토리 조회
```bash
git log
```

**options**:
- `-p` 각 커밋에 적용된 패치(patch, 반영된 변경사항)를 보여준다.
- `--stat` 각 커밋에서 수정된 파일의 통계정보를 보여준다.
- `--shortstat` --stat 옵션의 결과 중에서 수정한 파일, 추가된 줄, 삭제된 줄만 보여준다.
- `--name-only` 커밋 정보중에서 수정된 파일의 목록만 보여준다.
- `--name-status` 수정된 파일의 목록을 보여줄 뿐만 아니라 파일을 추가한 것인지, 수정한 것인지, 삭제한 것인지도 보여준다.
- `--abbrev-commit` 40자 짜리 SHA-1 체크섬을 전부 보여주는 것이 아니라 처음 몇 자만 보여준다.
- `--relative-date` 정확한 시간을 보여주는 것이 아니라 '2주 전'처럼 상대적인 형식으로 보여준다.
- `--graph` 브랜치와 머지 히스토리 정보까지 아스키 그래프로 보여준다.
- `--pretty` 지정한 형식으로 보여준다. 이 옵션에는 oneline, short, full, fuller, format이 있다. format은 원하는 형식으로 출력하고자 할 때 사용한다.
- `--walk-reflogs` 헤드가 이동한 순서대로 로그 출력

```bash
git log -p -2  # 2개의 항목과 패치내용만 보인다.
git log --pretty=oneline  # 각 커밋들의 메시지와 체크섬만 한 줄씩 출력된다.
git log -1 HEAD~3  # 헤드 기준 세번째 전의 커밋 로그 보기
git log v1.0 v2.4  # v1.0 태그에서 v2.4 태그 사이의 로그 보기
git log --oneline --decorate --graph --all  # 현재 브랜치의 모든 커밋 로그를 그래프로 보기
```

**pretty=format의 placeholder**:
- `%H` Commit hash
- `%h` Abbreviated commit hash
- `%T` Tree hash
- `%t` Abbreviated tree hash
- `%P` Parent hashes
- `%p` Abbreviated parent hashes
- `%an` Author name
- `%ae` Author e-mail
- `%ad` Author date (format respects the --date= option)
- `%ar` Author date, relative
- `%cn` Committer name
- `%ce` Committer email
- `%cd` Committer date
- `%cr` Committer date, relative
- `%s` Subject

```bash
git log --pretty=format:"%h %s" --graph
```
더 많은 내용은 [공식 도움말](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%BB%A4%EB%B0%8B-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EC%A1%B0%ED%9A%8C%ED%95%98%EA%B8%B0) 참고

**조회 범위 제한 옵션**:
- `-(n)` 최근 n 개의 커밋만 조회한다.
- `--since` | `--after` 명시한 날짜 이후의 커밋만 검색한다.
- `--until` | `--before` 명시한 날짜 이전의 커밋만 조회한다.
- `--author` 입력한 저자의 커밋만 보여준다.
- `--committer` 입력한 커미터의 커밋만 보여준다.

```bash
git log --since=2.weeks
git log --pretty="%h - %s" --author=gitster --since="2008-10-01" \ --before="2008-11-01" --no-merges -- t/
```

## merge
현재 브랜치에 다른 브랜치를 머지한다. 만약 충돌(conflict)이 발생하면 깃은 자동으로 머지를 중단하고 충돌이 발생한 파일에 각 커밋의 내용을 표시해준다.

깃의 머지는 두 개의 부모 커밋을 가리키는 특별한 커밋을 만들어 낸다. 두 개의 부모가 있는 커밋은 '한 부모의 모든 작업내역과 나머지 부모의 모든 작업, 그리고 그 두 부모의 모든 부모들의 작업내역을 포함한다'라는 의미가 있다.1
```
git merge 브랜치1 [브랜치2 브랜치3 브랜치3 ...]
```
```bash
git merge iss123  # 헤드 브랜치(현재 브랜치)에 iss123 브랜치를 머지
git merge iss123 hotfix  # 헤드 브랜치에 iss123과 hotfix를 머지
```
나열되는 브랜치들은 현재 헤드가 가리키는 브랜치에 어떤 브랜치들을 머지할 것인지를 의미한다. 그래서 다음의 경우:
```bash
git merge A B
```
아래와 같다:
```
merge A and B to CURRENT_BRANCH
```

#### 커밋하지 않고 머지
```bash
git merge --no-commit
```

#### 머지 도구 실행
3-way merge에 실패했을때 사용한다. mergetool로 지정된 앱을 실행하면서 .orig 확장자로 백업파일이 생성된다.
```bash
git mergetool
```

#### fetch 후 머지
fetch로 받아온 데이터를 현재 브랜치와 머지한다.
```bash
git merge FETCH_HEAD
```

#### 여러 커밋을 하나로 묶어서 머지
```bash
git merge --squash TARGET_BRANCH
```
머지할 브랜치의 커밋 이력을 하나로 압축한 별도의 커밋을 만들고 헤드 브랜치에 머지한다. 일반 머지와 다르게 하나의 부모커밋(헤드 브랜치 기준)만 갖는다.

## mv

#### 파일명 수정
```bash
git mv FILE_FROM FILE_TO
```

## pull
fetch 후 자동 머지.
```
git pull [리모트저장소] [브랜치]
```
리모트 저장소에서 데이터를 받아온다는 것은 fetch와 같지만 pull은 머지까지 한다는 점이 다르다.
```bash
git pull  # origin 리모트 저장소에서 현재 브랜치로 pull
git pull origin master
```
브랜치를 명시하지 않으면 (가능한 경우) 현재 브랜치만 자동 머지한다. 리모트의 저장소명과 브랜치명 생략은 업스트림 브랜치가 설정되어 있을 경우에만 가능하다.

## push
로컬 저장소의 데이터를 리모트 저장소에 업로드한다.
```
git push [리모트저장소] [브랜치]
```
```bash
git push  # origin 리모트 저장소에 현재 브랜치를 업로드
git push origin other  # origin에 other 브랜치 업로드. 리모트에 other 브랜치가 없으면 새로 생성한다.
```

#### 업스트림 브랜치 설정 #2
로컬 저장소를 init으로 생성했거나, 로컬에서 새로 생성한 브랜치일 때 업스트림 브랜치를 설정하는 방법이다.
```bash
git push --set-upstream origin master
Branch master set up to track remote branch master from origin.
Everything up-to-date
```

#### 로컬 브랜치와 리모트 저장소의 브랜치 이름이 다를때 (혹은 리모트 저장소에 다른 브랜치를 생성할 때)
```
git push 리모트저장소 로컬브랜치:서버브랜치
```
```bash
git push origin serverfix:awesome  # 현재 로컬 브랜치가 serverfix 일때 리모트 브랜치 awesome을 생성하고 푸시
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
현재 브랜치를 다른 브랜치에 머지. merge 명령이 두 브랜치의 최종결과만을 기준으로 머지한다면 리베이스는 브랜치의 변경사항을 순서대로 다른 브랜치에 적용하며 머지한다. 저장소의 커밋 로그와 이력을 한 줄로 정리해주기 때문에 보통 완료된 브랜치를 마스터에 머지할 때 사용한다.
```bash
git rebase master  # 현재 브랜치를 master 브랜치로 리베이스
```
위의 경우 현재 브랜치(HEAD)의 델타(변경 사항)를 패치(patch)로 만들어놓고, 현재 브랜치를 master의 마지막 커밋으로 이동한 뒤, 만들어뒀던 패치를 반영하는것과 결과가 같다.

자세한 내용은 아래 링크를 참고:
- [Git브랜치 Rebase하기](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)
- [Rebase의 위험성](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0#_rebase_peril)

#### 대화형 리베이스 도구로 여러 커밋 수정
```bash
git rebase -i HEAD~3  # 헤드부터 HEAD~3까지의 커밋을 대화형으로 수정
```
[커밋 메시지를 여러 개 수정하기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EB%8B%A8%EC%9E%A5%ED%95%98%EA%B8%B0#_changing_multiple)

## reflog
log와 비슷하지만 log가 커밋 이력을 출력한다면 reflog는 헤드 이동 이력을 출력한다.
```bash
$ git reflog -5  # 마지막 다섯 번의 헤드 이동 이력을 역순으로 출력

2fbc899 HEAD@{0}: checkout: moving from master to master
2fbc899 HEAD@{1}: pull: Merge made by the 'recursive' strategy.
7107b9e HEAD@{2}: checkout: moving from d to master
53576ad HEAD@{3}: merge c: Fast-forward
2bc9237 HEAD@{4}: checkout: moving from c to d
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
```
git remote add 단축이름 주소
```
```bash
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
```
git remote rm 리모트저장소이름
```

#### 리모트 저장소에서 쓸모가 없어진 브랜치 제거하기
```
git remote prune 리모트저장소
```

## reset
현재 브랜치 내에서 헤드를 이동한다.

#### staged 되돌리기(스테이징 취소)
add로 인덱스에 등록한 파일을 unstaged 상태로 바꾼다. 파일을 따로 명시하지 않으면 모든 스테이징을 취소한다.
```
git reset HEAD [파일]
```

#### 커밋 되돌리기 #1
체크섬이나 'HEAD~숫자'를 사용해서 헤드가 지난 커밋을 가리키게 할 수 있다.

**options**:
- `--soft` 헤드만 옮긴다. 스테이징 에어리어와 워킹 트리는 유지
- `--mixed` 명시하지 않을때의 기본값. 스테이징 에어리어를 헤드와 동일하게 변경한다. 워킹 트리는 유지
- `--hard` 헤드와 스테이징 에어리어, 워킹 트리를 모두 동일하게 변경한다.

```bash
git reset --soft HEAD~2  # 헤드만 2회 이전 커밋으로 이동
git reset --hard 4990ef  # 헤드를 4990ef 체크섬으로 이동하고 스테이징 에어리어, 워킹 트리를 동일하게 변경
```

## revert

#### 커밋 되돌리기 #2
1회 전의 커밋으로 되돌리되 단순히 헤드를 이동하고 끝나는게 아니라, 되돌려지는 내용을 기록한 새로운 커밋을 생성한다.
```bash
git revert HEAD  # 직전 커밋의 revert 커밋 생성
```
위의 경우 헤드 기준으로 가장 마지막 커밋의 변경사항을 되돌리는 커밋을 생성한다.
가령 직전의 커밋이 'fix-a-lot.md'라는 파일을 생성한 커밋이라면 `git revert HEAD`는 'fix-a-lot.md'를 삭제하는 커밋을 만드는 것이다.
```bash
git revert HEAD~1  # 1회 전 커밋의 리버트 커밋 생성
git revert HEAD~3  # 3회 전 커밋의 리버트 커밋 생성
git revert 3bd5055389bd059f0f781bfcfe3190bb7dfa9e5e  # 3bd505 커밋의 리버트 커밋 생성
git revert HEAD~4..HEAD  # 직전 커밋부터 3회전 커밋까지의 변경사항을 되돌린 커밋 생성
```

**options**:
- `-n` | `--no-commit` 워킹 트리와 인덱스의 상태만 되돌리고 커밋은 생성하지 않는다.
- `-m` | `--mainline` 머지 커밋을 리버트 할 때 사용하는 옵션.

#### 머지 커밋을 되돌리기
```bash
commit fc30cfe9255837f6810eb75adacaf93696828afe (HEAD -> revert-test)
Merge: 7be20ca ca433d3
# 7be20ca에 ca433d3을 머지하여 만들어진 커밋 fc30cf를 조회한 로그
```
머지 커밋을 되돌릴 때는 깃이 어느 부모가 메인라인이 되야하는지 알 수 있도록 `-m` 옵션과 함께 부모 번호(1부터 시작하는 부모 커밋을 가리키는 숫자)를 입력해줘야 한다:
```bash
git revert fc30cf -m 1
```
여기서 1이란 `git log`에서 보이는 부모 커밋 중 가장 왼쪽을 의미한다. 이 경우 7be20ca가 메인라인이 되며 ca433d3의 변경사항이 되돌려진다.

## rm

#### 파일/폴더 삭제
```bash
git rm readme.txt
```
rm 명령어는 깃이 추적중인 파일 혹은 폴더에만 사용할 수 있다.

#### 깃의 추적을 중단시키기
실제 파일은 남기고 깃의 관리대상에서만 제외한다.
```bash
git rm --cached readme.txt
```

#### file-glob 패턴으로 범위삭제
```bash
git rm log/\*.log  # log/디렉토리의 확장명이 log인 파일 모두 삭제
git rm \*.~  # ~로 끝나는 파일 모두 삭제
```

## show
커밋 정보 조회
```bash
git show HEAD  # 헤드 브랜치의 커밋 정보 조회
git show v1.1  # v1.1 태그의 커밋 정보 조회
git show 1c002dd4b536e7479fe34593e72e6c6c1819e53b  # 체크섬으로 조회
git show 1c002dd4b  # 체크섬은 중복이 없는한 앞의 일부분만 명시해도 인식한다.
```

## stash
커밋이나 스테이지가 아닌 별도의 공간에 변경사항을 임시 저장하거나 저장한 내용을 다시 불러오는 명령어.

#### 스태시 생성(임시 저장본 만들기)
추적 중인 파일의 모든 변경사항을 스태시에 저장되며 워킹 트리와 스테이징 에어리어는 헤드와 같아진다.
```bash
git stash  # 스태시 생성. stash save와 같음
git stash save
git stash -k  # --keep-index: staged 상태의 파일은 무시한다.
git stash -u  # --include-untracked: 추적중이지 않은 파일도 스태시로 저장
```

#### 스태시 확인
```bash
git stash list  # 스태시 목록 확인
git stash show  # 첫 번째 스태시 상세 확인
git stash show stash@{0}
```

#### 스태시 적용(임시 저장본 불러오기)
현재 브랜치에 스태시에 저장된 내용을 적용한다.
```bash
git stash apply  # 가장 최근의 스태시를 현재 브랜치에 적용
git stash pop  # 스태시를 적용하고 스택에서 삭제
```
apply는 스태시를 적용하되 저장한 스태시는 그대로 유지한다. 반면 pop은 적용한 스태시를 스택에서 삭제한다. 스태시는 기본적으로 modified 상태로 적용된다. `--index` 옵션을 사용하면 스태시를 저장할 때 staged 상태였던 파일을 다시 staged 상태로 만들어준다.
```bash
git stash pop --index
```

#### 스태시 삭제
```bash
git stash drop  # 첫 번째 스태시 삭제
git stash drop stash@{3}  # 네 번째 스태시 삭제
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

## svn

####SVN 저장소 복제
```
git svn clone SVN저장소주소
```

#### 표준 레이아웃 SVN 저장소 복제
표준 레이아웃(trunk와 branches, tags 폴더가 같은 위치에 있는 구조)을 사용하는 SVN 저장소를 복제할 때 사용한다.
```
git svn clone -s SVN저장소주소
```

#### 비표준 레이아웃 SVN 저장소 복제
trunk와 branches, tags 폴더의 위치를 각각 지정하는 방식이다.
```
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
```
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
```
git svn blame 파일
```

## tag
태그 조회
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

#### 지나간 커밋에 태그 만들기
체크섬을 알고 있다면 예전 커밋에도 태그할 수 있다.
```bash
git tag v0.8 9fceb02
```

#### 태그에 서명
GPG(GNU Privacy Guard) 개인키로 태그에 서명
```
git tag -s 태그명 [-m "태그메시지"]
```
```bash
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
