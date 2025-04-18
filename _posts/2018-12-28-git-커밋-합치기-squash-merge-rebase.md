---
layout: post
date: 2018-12-28 22:00:00 +0900
title: '[Git] 커밋 합치기'
categories:
  - git
tags:
  - git
  - rebase
  - merge
  - squash
---

* Kramdown table of contents
{:toc .toc}

#### 참고 문서

- [https://stackoverflow.com/questions/5189560/squash-my-last-x-commits-together-using-git/5201642](https://stackoverflow.com/questions/5189560/squash-my-last-x-commits-together-using-git/5201642)
- [커밋 메시지를 여러 개 수정하기](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-%ED%9E%88%EC%8A%A4%ED%86%A0%EB%A6%AC-%EB%8B%A8%EC%9E%A5%ED%95%98%EA%B8%B0#_changing_multiple)


## merge를 이용한 방법

아직 push 하지 않은 커밋이 두 개가 있다고 했을 때:

```bash
touch 1
git add .
git commit -s -m 'add 1'
touch 2
git add .
git commit -s -m 'add 2'
```

우선 스쿼시할 커밋들의 직전으로 헤드를 이동한다:

```bash
git reset --hard HEAD~2
```

이후 커밋들을 스쿼시 머지한다:

```bash
# HEAD@{1} is where the branch was just before the previous command.
# HEAD@{1} 표현은 커밋 아이디나 태그 등으로 대체할 수 있음
git merge --squash HEAD@{1}
```

헤드로부터 지정한 커밋까지의 변경 사항이 인덱스에 반영되었을 것이다:

```bash
$ git status
On branch master
Your branch is ahead of 'origin/master' by 5 commits.
  (use "git push" to publish your local commits)

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   1
        new file:   2
```

아직 커밋은 생성되지 않았으므로 커밋까지 완료해야 스쿼시가 끝난다.

아래는 자동 생성되는 커밋 메시지:

```bash
Squashed commit of the following:

commit accdca6b50a8df8a5d6be4eba9722c5006cf531b
Author: noritersand <who@where.com>
Date:   Fri Dec 28 16:05:38 2018 +0900

    add 2

    Signed-off-by: noritersand <who@where.com>

commit f65276e23ad3498dc7f469591fc07d6057dedac3
Author: noritersand <who@where.com>
Date:   Fri Dec 28 16:05:37 2018 +0900

    add 1

    Signed-off-by: noritersand <who@where.com>

# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# Your branch is ahead of 'origin/master' by 3 commits.
#   (use "git push" to publish your local commits)
#
# Changes to be committed:
# new file:   1
# new file:   2
#

```


## rebase를 이용한 방법

아래처럼 5개의 커밋이 있는 상태에서:

```bash
PS C:\dev\git\git-test> git log -5
commit 810000fe555478c43583b12d7142f9ba08d7c016 (HEAD -> main, origin/main, origin/HEAD)
Author: noritersand <noritersand@example.com>
Date:   Thu Feb 18 22:29:59 2021 +0900

    I added file too.

commit 4def5aa0f82dcaa0be6a17b46372070d939caa3c
Author: noritersand <noritersand@example.com>
Date:   Thu Feb 18 22:28:17 2021 +0900

    Update 3, 4, 5 lines.

commit d5ecc08e9cf6ed76fd136ab5669af6546162aa8a
Author: noritersand <noritersand@example.com>
Date:   Thu Feb 18 22:34:21 2021 +0900

    Hello!

commit f29b0b3a7a0b53b5ffe5380e0ed295fa375f99e0
Author: noritersand <noritersand@example.com>
Date:   Thu Feb 18 22:26:39 2021 +0900

    Create HELLO.md

commit 4991dad214779ae01ee498d1909d95adf0d7c00a
Author: noritersand <noritersand@example.com>
Date:   Wed Feb 10 00:04:22 2021 +0900

    update
```

마지막 커밋 세 개(810000, 4def5a, d5ecc0)를 스쿼시 해보자.

우선 인터렉티브 모드로 `rebase` 명령을 실행하면서 스쿼시할 범위를 지정한다(합칠 브랜치보다 1회 더 전으로 지정해야 함):

```bash
git rebase -i HEAD~3 # 헤드부터 3회 전 커밋까지 rebase
```

```bash
pick d5ecc08 Hello!
pick 4def5aa Update 3, 4, 5 lines.
pick 810000f I added file too.

# Rebase f29b0b3..810000f onto f29b0b3 (3 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# ...
```

과거의 커밋 순으로 나오는데, 다음처럼 맨 위만 빼고 `squash`\*로 바꿔준다:

```bash
pick d5ecc08 Hello!
squash 4def5aa Update 3, 4, 5 lines.
squash 810000f I added file too.
```

\* 기본 에디터인 Vim에선 <kbd>ctrl + a</kbd>와 <kbd>ctrl + x</kbd>로 rebase 옵션을 변경할 수 있다.

이 후 저장하고 커밋 메시지까지 입력하면 아래처럼:

```bash
PS C:\dev\git\git-test> git log -3
commit 87f06d9f47b55abd5220a53170a884e3248759c1 (HEAD -> main)
Author: noritersand <noritersand@example.com>
Date:   Thu Feb 18 22:34:21 2021 +0900

    Hello!

    Update 3, 4, 5 lines.

    I added file too.

commit f29b0b3a7a0b53b5ffe5380e0ed295fa375f99e0
Author: noritersand <noritersand@example.com>
Date:   Thu Feb 18 22:26:39 2021 +0900

    Create HELLO.md

commit 4991dad214779ae01ee498d1909d95adf0d7c00a
Author: noritersand <noritersand@example.com>
Date:   Wed Feb 10 00:04:22 2021 +0900

    update
```

커밋 세 개가 하나로 합쳐진다.

끗.
