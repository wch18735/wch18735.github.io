---
title: "[Git] Git branch 생성 및 삭제"
excerpt: "명령어를 이용한 git branch 관리"
date: 2021-06-28
layout: single
classes: wide
category:
    - git
tag:
    - github
    - git branch
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Git Branch 생성 및 삭제

[Git 브랜치](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)는 모든 버전 관리 시스템이 그러하듯, Git에서 지원하는 브랜치를 뜻합니다. 개발 과정에서는 동일한 코드에서 몇 가지 다양한 변화를 주거나 독립적인 개발을 필요로 할 때 이 브랜치가 사용됩니다.

Git의 브랜치는 매우 가볍고, 생성 및 삭제가 쉽습니다. 이러한 브랜치는 추후 Merge 되어 Main 브랜치에 합쳐질 수 있습니다. 이러한 방법이 익숙해지면 안정적인 개발 프로세스를 기반으로 한 개발, 수정, 배포가 가능해집니다.

아래는 **Modern Java in Action Study**를 기획하며 Organization 안에 생성한 레포지토리의 로그입니다. 샘플 브랜치를 만들고, 지우는 과정을 보여줍니다. 새로운 브랜치를 만들고, 지우는 과정은 다음과 같이 요약될 수 있습니다.

1. 로컬에서 브랜치 생성
2. 원격 저장소에 로컬에서 생성된 브랜치 올리기
3. 원격 저장소의 브랜치 삭제
4. 로컬에서 브랜치 삭제

## 정리

- `git branch` : 현재 어떤 branch 에 작업하고 있는지 확인
- `git branch -r` : 해당 Repository 에 어떤 branch 들이 있는지 원격으로 확인
- `git checkout [branch 이름]` : 특정 branch 로 전환 (생성되지 않은 branch 로는 이동 불가)
- `git checkout -b [branch 이름]` : 특정 branch 를 생성하며 전환 **→ Local 저장소에 branch 생성**
  - `git push -u origin [branch 이름]`  **→ Remote 저장소에 branch 생성**
  - `git branch --set-upstream-to [remote 이름]/[branch 이름]` 으로 push 설정 가능

## Log

```bash

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch
* master
  test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git checkout test_branch
Switched to branch 'test_branch'

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ touch test.txt

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ vim test.txt

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git add .
warning: LF will be replaced by CRLF in test.txt.
The file will have its original line endings in your working directory

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git push -u origin test_branch
Total 0 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'test_branch' on GitHub by visiting:
remote:      https://github.com/Sample-Study/Modern-Java-in-Action/pull/new/test_branch
remote:
To https://github.com/Sample-Study/Modern-Java-in-Action.git
 * [new branch]      test_branch -> test_branch
Branch 'test_branch' set up to track remote branch 'test_branch' from 'origin'.

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git commit -m "test text added"
[test_branch 01dd9c3] test text added
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git push -u origin master
Everything up-to-date
Branch 'master' set up to track remote branch 'master' from 'origin'.

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git push -u origin test_branch
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 287 bytes | 287.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/Sample-Study/Modern-Java-in-Action.git
   c88f4de..01dd9c3  test_branch -> test_branch
Branch 'test_branch' set up to track remote branch 'test_branch' from 'origin'.

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git push origin --delete test_branch
To https://github.com/Sample-Study/Modern-Java-in-Action.git
 - [deleted]         test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git branch
  master
* test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch
* master
  test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch -d test_branch
error: The branch 'test_branch' is not fully merged.
If you are sure you want to delete it, run 'git branch -D test_branch'.

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch -D test_branch
Deleted branch test_branch (was 01dd9c3).

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch
* master

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$
```

## Git Branch 생성

브랜치를 생성할 때는 다음과 같은 명령어를 입력합니다. 이 방법은 로컬에서 새로운 브랜치를 만드는 방법입니다.

`git branch [새로운 브랜치 이름]`

이렇게 만들어진 브랜치를 원격 저장소에 올리기 위해서는 현재 브랜치를 바꾸어야 합니다. 이를 `checkout` 명령어를 이용합니다. 이후 `git branch`로 현재 브랜치가 제대로 바뀌었는지 확인할 수 있습니다.

`git checkout [바꿀 브랜치 이름]`

끝으로 이를 원격 저장소에 push 하면 새로운 브랜치가 원격 저장소에 추가되는 것을 확인할 수 있습니다. 참고로, 다음 git 사용법은 **원격 merge**를 다뤄볼 예정입니다.

```bash
wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch
* master
  test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git checkout test_branch
Switched to branch 'test_branch'

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git push -u origin test_branch
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 287 bytes | 287.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/Sample-Study/Modern-Java-in-Action.git
   c88f4de..01dd9c3  test_branch -> test_branch
Branch 'test_branch' set up to track remote branch 'test_branch' from 'origin'.
```

## Git Branch 삭제

브랜치 삭제는 생성과 역순입니다. 먼저 원격 저장소에 있는 브랜치를 삭제합니다. 이때는 `git push --delete [삭제할 브랜치 이름]` 으로 명령을 전달합니다. 이것이 수행된 이후 로컬 브랜치를 `git branch -D [삭제할 브랜치 이름]` 를 이용해 삭제합니다.

```bash
wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git push origin --delete test_branch
To https://github.com/Sample-Study/Modern-Java-in-Action.git
 - [deleted]         test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git branch
  master
* test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch
* master
  test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch -D test_branch
Deleted branch test_branch (was 01dd9c3).

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch
* master
```

## References
- https://git-scm.com/doc