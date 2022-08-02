---
title: "[Git] Git 이력 관리"
excerpt: "git log management"
date: 2022-07-25
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

# Git 이력 이용하기

모든 버전 관리 시스템에서 가장 중요한 부분은 시스템이 가지고 있는 이력이다. 새로 추가한 파일이나 파일을 변경한 내용이 시스템 이력의 커밋을 구성하는 단위가 된다. 이전까지는 이 **커밋**을 합치고 정리하는 방법을 배웠다.

이번 포스트에서는 아래 내용을 정리한다.

- git log 를 이용해 저장소 이력을 확인하는 방법
- 검색하기 쉽도록 커밋 범위를 지정하는 방법
- 커밋간의 차이를 보는 방법
- 파일에 줄마다 이력 표시하는 방법
- 이리저리 옮긴 내용 추적하는 방법
- 변경한 내용을 취소하는 방법
- 저장소 이력을 고쳐쓰는 방법

알고 싶은 내용이 많다면 Git 은 파일의 모든 줄에 누가 언제 수정했는지를 표시해 보여줄 수 있다. 더 파고들면 파일 두 리비전의 차이점까지 확인할 수 있다. 이 능력은 다양한 사람들과 협업할 때, 나아가 프로젝트를 관리ㆍ운영할 때 귀중한 자산이 되어줄 것이다.

## Git Log 살펴보기

저장소에 어떤 변화가 있는지 알고싶다면 `git pull` 을 당겨온 뒤 `git log` 명령을 통해 로그를 확인한다. Git 의 출력 결과는 less 명령어를 이용해서 출력되므로 화면에 보여줄 수 있는 양보다 내용이 많다면 로그를 스크롤해서 볼 수 있다.

```bash
commit e9d64c1a4ff52c9c81ef86fdc8c4b68e919bc722 (HEAD -> master, origin/master)
Merge: da6bd44 a70d63e
Author: wch18735 <wch18735@naver.com>
Date:   Tue Jul 19 01:01:51 2022 +0900

    merged

commit a70d63e9376e8a89e45d316b77711eba4a0baafd (contact)
Author: wch18735 <wch18735@naver.com>
Date:   Tue Jul 19 00:50:04 2022 +0900

    modified in contact branch

commit da6bd4430f55a78365814a8dd5c636120d42045e
Author: wch18735 <wch18735@naver.com>
Date:   Tue Jul 19 00:49:11 2022 +0900

    modified in master

commit 54e5956e700a8686a5b7fd6b3b9e212d3d2d39a4
Author: wch18735 <wch18735@naver.com>
Date:   Tue Jul 19 00:31:53 2022 +0900

:
```

아래처럼 화면 하단에 콜론(:)이 있다면 출력 결과를 더 남아 있음을 의미한다. 방향키 상하좌우 키를 이용해 내용을 조금 더 자세히 확인 가능하다. 종료는 q 를 누르면 가능하다.

가장 많이 사용하는 기능은 N개의 로그를 보는 것이다. `git log -N` 의 N 자리에 원하는 숫자를 넣으면 된다. 아래는 해당 명령어를 사용하는 예시다.

```bash
$ git log -5 --oneline
e9d64c1 (HEAD -> master, origin/master) merged
a70d63e (contact) modified in contact branch
da6bd44 modified in master
54e5956 cherry-pick
88f062a cherry-pick
```

## 특정 날짜나 시간 범위를 이용한 기록 조회

끝으로 앞의 7자리 커밋명을 이용하면 해당 커밋의 상세 내역을 확인할 수 있다. 또는 리비전 범위를 지정해 얻어낼 수 있다. 예를 들어 지난 다섯 시간 동안의 커밋만 보려면 아래와 같이 사용할 수 있다. 5시간 동안 한 개도 커밋하지 않아서 기록이 없다.

```bash
$ git log --since="5 hours"
```

반대로 5시간 동안의 커밋을 제외한 것들을 보려면 아래와 같이 수행할 수 있다.

```bash
$ git log --before="5 hours"
commit e9d64c1a4ff52c9c81ef86fdc8c4b68e919bc722 (HEAD -> master, origin/master)
Merge: da6bd44 a70d63e
Author: wch18735 <wch18735@naver.com>
Date:   Tue Jul 19 01:01:51 2022 +0900

    merged

commit a70d63e9376e8a89e45d316b77711eba4a0baafd (contact)
Author: wch18735 <wch18735@naver.com>
Date:   Tue Jul 19 00:50:04 2022 +0900

    modified in contact branch

commit da6bd4430f55a78365814a8dd5c636120d42045e
Author: wch18735 <wch18735@naver.com>
Date:   Tue Jul 19 00:49:11 2022 +0900

    modified in master

commit 54e5956e700a8686a5b7fd6b3b9e212d3d2d39a4
Author: wch18735 <wch18735@naver.com>
Date:   Tue Jul 19 00:31:53 2022 +0900

:
```

`--since` 와 `--before` 를 동시에 사용하거나 `[from]..[to]` 와 같은 형태로 커밋 이력을 확인할 수 있다. 주의할 점으로는 항상 오래된 로그 형식을 먼저 지정해줘야 동작한다는 것이다.

```bash
$ git log --oneline
e9d64c1 (HEAD -> master, origin/master) merged
a70d63e (contact) modified in contact branch
da6bd44 modified in master
54e5956 cherry-pick
88f062a cherry-pick
ff78ab2 cherry-pick
34184f6 add email
fafb1e2 add contact file
cf1fc94 alternate
c57865d about.txt hello.txt

$ git log 34184f6..da6bd44 --oneline
da6bd44 modified in master
54e5956 cherry-pick
ff78ab2 cherry-pick
```

이 방식으로 특정 커밋부터 지금까지 커밋을 모두 확인할 수 있다. `[to]` 부분을 비워두거나 `HEAD` 로 나타내면 현재 커밋 헤드를 기준으로 지정된 커밋까지의 기록을 보여준다.

```bash
$ git log 34184f6..HEAD --oneline
e9d64c1 (HEAD -> master, origin/master) merged
a70d63e (contact) modified in contact branch
da6bd44 modified in master
54e5956 cherry-pick
88f062a cherry-pick
ff78ab2 cherry-pick
```

끝으로 `~N` 또는 `^` 을 사용하여 가장 최근 커밋부터 5번째 이전 커밋까지를 확인하는 에시로 마무리해보자.

```bash
$ git log HEAD~5..HEAD --oneline
e9d64c1 (HEAD -> master, origin/master) merged
a70d63e (contact) modified in contact branch
da6bd44 modified in master
54e5956 cherry-pick
88f062a cherry-pick
ff78ab2 cherry-pick
34184f6 add email
```

## 커밋에서 달라진 저 확인하기

특정 커밋 ID를 알고있으면 `diff` 명령어를 사용해 현재 상태로부터 해당 커밋까지 어떤 것이 바뀌었는지 확인할 수 있다. 여기서 `--stat` 을 추가하면 각 파일들에 대한 변경점들을 통계내어 출력해준다.

```bash
$ git diff e9d64c1
diff --git a/about.txt b/about.txt
index 240e963..8497055 100644
--- a/about.txt
+++ b/about.txt
@@ -1 +1,2 @@
 about.txt
+change here
```

## 누구 책임인지 찾기

동사 blame 은 `~을 탓하다` 또는 `~를 비난하다` 등의 뜻을 가지고 있다. 실제로 우리는 누가 커밋을 했는지, 언제 했는지 등을 찾아야 할 때가 있다. 물론 누군가에게 책임을 전가하기 위해서가 아니라 담당자에게 수정 또는 조언을 구하기 위함이다. 예를 들어, hello.html 파일에 대한 커밋 이력을 확인하려면 `git blame hello.html` 명령을 사용한다.

```bash
$ git blame hello.txt
^c57865d (wch18735 2022-07-18 23:45:06 +0900 1) hello.txt
e9d64c1a (wch18735 2022-07-19 01:01:51 +0900 2) <<<<<<< HEAD
da6bd443 (wch18735 2022-07-19 00:49:11 +0900 3) modified in master
e9d64c1a (wch18735 2022-07-19 01:01:51 +0900 4) =======
a70d63e9 (wch18735 2022-07-19 00:50:04 +0900 5) modified in contact branch
e9d64c1a (wch18735 2022-07-19 01:01:51 +0900 6) >>>>>>> contact
```

`blame` 을 이용하면 파일 내용에 대한 추적도 가능하다. 실제로 같은 내용을 반복시킨 파일에서 `git blame -M [filename]` 명령을 수행하면 가장 처음 원본 내용을 작성한 커밋ID로 모두 바뀌는 것을 확인할 수 있다. 또한, 파일 내용뿐만 아니라 파일 복사를 확인하는 방법으로는 `git blame -C -C [filename]` 을 사용할 수 있다.