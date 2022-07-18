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

[Git 브랜치](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EB%B8%8C%EB%9E%9C%EC%B9%98%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)는 모든 버전 관리 시스템이 그러하듯, Git에서 지원하는 브랜치를 뜻한다. 개발 과정에서는 동일한 코드에서 몇 가지 다양한 변화를 주거나 독립적인 개발을 필요로 할 때 이 브랜치가 사용된다.

Git의 브랜치는 매우 가볍고, 생성 및 삭제가 쉽다. 이러한 브랜치는 추후 Merge 되어 Main 브랜치에 합쳐질 수 있다. 이러한 방법이 익숙해지면 안정적인 개발 프로세스를 기반으로 한 개발, 수정, 배포가 가능해진다.

아래는 **Modern Java in Action Study**를 기획하며 Organization 안에 생성한 레포지토리에 샘플 브랜치를 만들고, 지우는 과정을 보여준다. 새로운 브랜치를 만들고, 지우는 과정은 아래와 같이 요약할 수 있다.

1. 로컬에서 브랜치 생성
2. 원격 저장소에 로컬에서 생성된 브랜치 올리기
3. 원격 저장소의 브랜치 삭제
4. 로컬에서 브랜치 삭제

## 활용법

- `git branch` : 현재 어떤 branch 에 작업하고 있는지 확인
- `git branch -r` : 해당 Repository 에 어떤 branch 들이 있는지 원격으로 확인
- `git checkout [브랜치 이름]` : 특정 branch 로 전환 (생성되지 않은 branch 로는 이동 불가)
- `git checkout -b [브랜치 이름]` : 특정 branch 를 생성하며 전환 **→ Local 저장소에 branch 생성**
  - `git push -u origin [브랜치 이름]`  **→ Remote 저장소에 branch 생성**
  - `git branch --set-upstream-to [원격 이름]/[브랜치 이름]` 으로 push 설정 가능
- `git branch -d [브랜치 이름]` : 로컬에서 branch 를 삭제
- `git push [원격 이름] --delete [브랜치 이름]` : 원격 저장소의 branch 를 삭제

아래는 해당 명령어와 예시를 확인한 로그들이다.

### 브랜치 생성 : git branch [생성할 브랜치명]

```bash

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch test_branch

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git branch
* master
  test_branch
```

### 로컬 작업 브랜치 변경 : git checkout [변경할 브랜치명]

```bash
wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (master)
$ git checkout test_branch
Switched to branch 'test_branch'
```

### 새로운 branch 로 commit and push

```bash
wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ touch test.txt

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ vim test.txt

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git add .
warning: LF will be replaced by CRLF in test.txt.
The file will have its original line endings in your working directory

wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git commit -m "test text added"
[test_branch 01dd9c3] test text added
 1 file changed, 1 insertion(+)
 create mode 100644 test.txt

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
```

### 원격 브랜치 삭제 : git push [remote 명] --delete [브랜치 명]

```bash
wch18@DESKTOP-AV7344M MINGW64 /d/github/Moden Java in Action (test_branch)
$ git push origin --delete test_branch
To https://github.com/Sample-Study/Modern-Java-in-Action.git
 - [deleted]         test_branch

```

### 로컬 브랜치 삭제 : git branch -d [삭제할 브랜치 명]

```bash
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
```

## References
- https://git-scm.com/doc