---
title: "[Git] 브랜치 이해하고 활용하기"
excerpt: "basic git funcionality"
date: 2022-07-18
layout: single
classes: wide
category:
    - git
tag:
    - github
    - git
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 브랜치 이해하고 활용하기

기본적으로는 하나의 main 브랜치에서 모든 작업이 수행된다. 그러나 프로젝트 규모가 커지고, 버전 관리가 복잡해지며 곧 여러 브랜치 운영이 필요한 순간이 다가온다. Git 에서 브랜치는 매우 중요한 부분이며, 아래 내용을 기본으로 익혀야 기업이 요구하는 다양한 브랜치 전략을 수행할 수 있다.

- 새로운 브랜치 생성
- 브랜치 간의 변경 사항 합치기
- 충돌 다루기
- 브랜치 삭제하기
- 브랜치명 변경하기
- 릴리즈 브랜치 다루기

## 브랜치란?

Git 에서 브랜치는 작업의 이력의 갈래이다. Git 은 모든 것을 브랜치로 관리하며, 직관적으로는 **방향성 비순환 그래프(Directed Acyclic Graph, DAG)** 라고 설명할 수 있다. 

한 개 브랜치로만 형상관리를 한다면? 이 같은 단방향 이력 관리만으로는 기능 추가, 리팩터링, 버그 수정과 같은 다양한 요구 사항을 만족시킬 수 없다. 그래서 브랜치는 필수적이라 할 수 있다.

자세한 내용은 `git branch -h` 옵션을 사용하면 모두 확인할 수 있다.

```bash
$ git branch -h
usage: git branch [<options>] [-r | -a] [--merged | --no-merged]
   or: git branch [<options>] [-l] [-f] <branch-name> [<start-point>]
   or: git branch [<options>] [-r] (-d | -D) <branch-name>...
   or: git branch [<options>] (-m | -M) [<old-branch>] <new-branch>
   or: git branch [<options>] (-c | -C) [<old-branch>] <new-branch>
   or: git branch [<options>] [-r | -a] [--points-at]
   or: git branch [<options>] [-r | -a] [--format]

Generic options
    -v, --verbose         show hash and subject, give twice for upstream branch
    -q, --quiet           suppress informational messages
    -t, --track           set up tracking mode (see git-pull(1))
    -u, --set-upstream-to <upstream>
                          change the upstream info
    --unset-upstream      Unset the upstream info
    --color[=<when>]      use colored output
    -r, --remotes         act on remote-tracking branches
    --contains <commit>   print only branches that contain the commit
    --no-contains <commit>
                          print only branches that don't contain the commit
    --abbrev[=<n>]        use <n> digits to display SHA-1s

Specific git-branch actions:
    -a, --all             list both remote-tracking and local branches
    -d, --delete          delete fully merged branch
    -D                    delete branch (even if not merged)
    -m, --move            move/rename a branch and its reflog
    -M                    move/rename a branch, even if target exists
    -c, --copy            copy a branch and its reflog
    -C                    copy a branch, even if target exists
    -l, --list            list branch names
    --create-reflog       create the branch's reflog
    --edit-description    edit the description for the branch
    -f, --force           force creation, move/rename, deletion
    --merged <commit>     print only branches that are merged
    --no-merged <commit>  print only branches that are not merged
    --column[=<style>]    list branches in columns
    --sort <key>          field name to sort on
    --points-at <object>  print only branches of the object
    -i, --ignore-case     sorting and filtering are case insensitive
    --format <format>     format to use for the output
```

## 새로운 브랜치 생성하기

`git branch` 명령을 사용하면 현재 작업 중인 브랜치 앞에 * 가 표시되며 녹색으로 나타난다. `git branch [새로운 브랜치 명]` 을 통해 새로운 브랜치를 만들 수 있고, `git branch -m [브랜치 명] [변경할 브랜치 명]` 으로 이름을 바꿀 수도 있다.

또한, `git checkout [변경할 브랜치]` 를 사용해 작업할 브랜치를 변경할 수 있다. 

```bash
wch18735@DESKTOP MINGW64 ~/Desktop/WCH/Github/[REPO] GitFirst (master)
$ git branch
  RB_1.0.1
* master

wch18735@DESKTOP MINGW64 ~/Desktop/WCH/Github/[REPO] GitFirst (master)
$ git checkout RB_1.0.1
Switched to branch 'RB_1.0.1'

wch18735@DESKTOP MINGW64 ~/Desktop/WCH/Github/[REPO] GitFirst (RB_1.0.1)
$ git branch
* RB_1.0.1
  master
```

 현재 브랜치가 여러 개 있는 상황이라면? `git checkout -b [생성할 브랜치] [기존 브랜치]` 명령으로 특정 브랜치에서 분기시킬 수 있다. 

 ## 브랜치 간의 변경 사항 합치기

 각 브랜치는 저자오를 구성하는 데 매우 중요한 요소이나, 브랜치 간의 변경 사항을 공유하고 합칠 필요도 있다. 이런 경우를 브랜치 병합이라 한다. 병합 방법은 대표적으로 아래 3가지가 있다.

 - **바로 합치기(Straight Merge):** 하나의 브랜치와 다른 브랜치의 변경 이력 전체를 합치는 방법
 - **커밋 합치기(Squashed Commit):** 한 브랜치의 이력을 압축하여 다른 브랜치의 최신 커밋 하나로 만드는 방법
 - **선택하여 합치기(Cherry-picking):** 다른 브랜치에서 하나의 커밋을 가져와 현재 브랜치에 적용하는 방법

### 바로 합치기

먼저 바로 합치는 경우부터 확인해보자. 아래와 같이 두 브랜치가 있다.

```bash
$ git branch
  alternate
* master
```

현재 마스터 브랜치로 alternate 브랜치를 합치고 싶다면 **현재 브랜치**에서 다음과 같이 `git merge [병합 대상 브랜치]` 명령어를 입력한다. 

```bash
$ git merge alternate
Updating c57865d..cf1fc94
Fast-forward
 alternate.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 alternate.txt
```

위 명령어를 입력하면 alternate 브랜치 변경 내용이 master 브랜치에 합쳐졌다.

### 커밋 합치기

개발할 기능이나 수정할 변경 사항에 따라 브랜치를 생성하는 것도 Git 에서 브랜치를 사용하는 방법 중 하나다. 새로운 기능이나 버그를 수정했다면 변경 사항을 다시 합칠 수 있도록 커밋 합치기를 사용한다.

이 때, 브랜치 하나의 변경 이력을 아축해 다른 브랜치에 하나의 커밋으로 만들어 적용한다. 이러한 커밋 합치기를 할 때는 압축 대상인 모든 커밋 이력이 독자적으로 유지되어야 한다는 것이다. 브랜치에ㅓ 변경한 내용들이 합쳐질 내용과 충돌하는 부분 없이 하나의 변경 사항으로 볼 수 있다면, 커밋 합치기를 사용할 수 있는 후보다.

특히, 이것저것 실험한 후 마지막 결과만을 적용하고 싶을 때 유용하게 사용할 수 있다. 마스터 브랜치로 이동한 뒤 `git merge --squash [대상 브랜치]` 명령을 입력하면 변경 내역이 합쳐진다. 하지만 단순 merge 와 다르게 commit 은 되지 않고, 변경 내역만 스테이징 영역에 남는다.

```bash
$ git merge --squash contact
Updating cf1fc94..34184f6
Fast-forward
Squash commit -- not updating HEAD
 checkout.txt | 2 ++
 1 file changed, 2 insertions(+)
 create mode 100644 checkout.txt
```

변경 영역만 스테이징된 것을 아래와 같이 확인할 수 있다. 이제 모든 변경 사항의 결과만을 가져왔다. 마지막은 `git commit` 을 통해 한 개의 commit 으로 기록하는 것이다.

```bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   checkout.txt
```

### 선택하여 합치기

지금까지는 변경된 모든 내용 전체를 반영하는 방법을 알아보았다. 그러나 때로는 브랜치 간에 전체를 합치는 대신 오직 하나 또는 몇 개의 커밋만 합칠 필요가 있다. 대표적으로 **브랜치들 사이에 수정된 버그나 새로 추가한 클래스를 공유하는 경우**가 있다.

```bash
wch18735@DESKTOP-NA583NG MINGW64 ~/Desktop/WCH/GitHub/[REPO] GitFirst (contact)
$ git commit -m "cherry-pick"
[contact 88f062a] cherry-pick
 1 file changed, 1 insertion(+), 1 deletion(-)

wch18735@DESKTOP-NA583NG MINGW64 ~/Desktop/WCH/GitHub/[REPO] GitFirst (contact)
$ git checkout master
Switched to branch 'master'

wch18735@DESKTOP-NA583NG MINGW64 ~/Desktop/WCH/GitHub/[REPO] GitFirst (master)
$ git cherry-pick 88f062a
[master 54e5956] cherry-pick
 Date: Tue Jul 19 00:31:53 2022 +0900
 1 file changed, 1 insertion(+), 1 deletion(-)
```

위 예시처럼 변경사항이 저장된 `88f062a` 커밋을 master 브랜치에서 불러온다. 지금은 한 개에서만 수행했지만 여러 커밋들을 동시에 가져올 수 있으며 방법은 `git cherry-pick [commit hash] ... [commit hash]` 와 같이 커밋 해시를 나열해주면 된다.

`git cherry-pick` 은 선택한 커밋의 변경 사항을 가지고 새로운 커밋을 생성하는데, 선택하려는 커밋이 여러 개인 경우 **변경 사항만 가져와 반영하는 것도 가능하다.** 이는 `git cherry-pick -n` 을 사용해 커밋하지 않게 할 수 있다. 이 매개변수를 이용하면 여러 커밋들을 가져온 후 한 번에 반영할 수 있다. 

끝으로 cherry-pick 도중 에러가 발생했을 때에는 두 가지 옵션이 있다.

- `git cherry-pick --continue` : conflict 가 발생한 파일을 수정한 후 다시 진행
- `git cherry-pick --abort` : cherry-pick 중단

## 충돌 다루기

두 개의 다른 브랜치에서 동일한 파일을 다르게 편집한 후 합치려고 할 때, Git 은 이를 충돌로 판단해 실행을 멈추고 사용자가 수정하도록 한다.

hello.txt 를 만들고 브랜치를 분기한 후 서로 다른 내용을 기입했다. 그리고 양쪽에 커밋을 수행한 뒤 master 브랜치에서 merge 를 진행하면 아래와 같은 에러를 확인할 수 있다.

```bash
$ git merge contact
Auto-merging hello.txt
CONFLICT (content): Merge conflict in hello.txt
Automatic merge failed; fix conflicts and then commit the result.
```

충돌이 난 부분을 확인하기 위해 hello.txt 를 열어보면 아래와 같은 내용을 확인할 수 있다.

```bash
$ vim hello.txt

hello.txt
<<<<<<< HEAD
modified in master
=======
modified in contact branch
>>>>>>> contact
```

`<<<<<<<` 뒤에 나오는 부분은 현재 브랜치 코드, `>>>>>>>` 뒤에 나오는 부분은 다른 브랜치의 코드이다. 합칠 내용이 간단하다면 충돌이 발생한 부분을 직접 수정한다. 복잡한 내용인 경우 코드를 비교하면서 합칠 수 있는 도구를 사용한다. `git mergetool` 명령어로 합치기 도구를 실행할 수 있다.

끝으로 변경된 사항을 add 한 뒤 commit 을 수행한다. 그러면 아래와 같이 MERGING 과정이 종료된다.

```bash
wch18735@DESKTOP-NA583NG MINGW64 ~/Desktop/WCH/GitHub/[REPO] GitFirst (master|MERGING)
$ git add .

wch18735@DESKTOP-NA583NG MINGW64 ~/Desktop/WCH/GitHub/[REPO] GitFirst (master|MERGING)
$ git commit -m "merged"
[master e9d64c1] merged

wch18735@DESKTOP-NA583NG MINGW64 ~/Desktop/WCH/GitHub/[REPO] GitFirst (master)
```