---
title: "[Git] Git Log 관리"
excerpt: "Git Log 관리"
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

# Git Log 관리

로컬 저장소에서 commit log 를 관리한다. 원격 저장소에 commit 한 경우라면 revert 명령어를 사용해 되돌릴 수 있다.

## Git 컨텐츠 관리자

- HEAD : 마지막 커밋 스냅샷, 다음 커밋의 부모 커밋
- Index : 다음에 커밋할 스냅샷
- Working Directory : 현재 작업 공간

여기서 스냅샷이란 용어에 주목해 이해하면 조금 더 쉽게 받아들일 수 있다. 어떤 작업을 수행했을 때, 변경점이 발생하는데 각 변경점들에 대한 스냅샷을 뜬다고 생각하면 된다. Index 는 예를 들어 `git add .` 를 통해 저장된 변경점들이 저장되는 곳이다. 

## git reset

아주 세부적이고 명확한 설명은 [이곳](https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-Reset-%EB%AA%85%ED%99%95%ED%9E%88-%EC%95%8C%EA%B3%A0-%EA%B0%80%EA%B8%B0) 에서 확인할 수 있다. 정리하자면 git reset 동작은 HEAD 의 이동으로 설명할 수 있다.

- `git reset --soft HEAD~N` : 특정 지점으로 HEAD 에서부터 `N만큼` 이동
- `git reset --mixed HEAD~N` : 특정 지점으로 HEAD 에서부터 `N만큼` 이동 후 Index 까지 변경
- `git reset --hard HEAD~N` : 특정 지점으로 HEAD 에서부터 `N만큼` 이동 후 Working Directory 까지 변경

## 활용 예

실제 사용하는 예시를 살펴보자.

아래와 같이 한 줄로 로그를 살펴본 뒤, 가장 아래에 있는 `test` 로그로 이동한다.

```bash
$ git log --pretty=oneline
13238d7b16417c42b4d3e4637ff1fdc6fd328f08 (HEAD -> RB_1.0.1, tag: 1.0) add description
7f0e0f6bdd38663a181e049d0ead0e7adc641b93 add <head> and <title> to index
df7db932fa72af895fb9099a6495298c23839932 (origin/master) add index.html
33cee1be070a591017096c5bdfeefa68b0275323 Merge remote-tracking branch 'origin/branch01'
cf7124149b57a20f80efcf73d7c13f12bba747d7 branch test
5fffa5b6bd56548dd851595502b2f1f7a9be7931 master test
fcb8457e86903b3eb362dac7d090d640562297ce Merge branch 'branch01' of https://github.com//GitFrist
ee9f53692dfa786449d543d31d9b39d8d872b466 test
200870ecb431c09781db510a1b472037b795a6eb test
4461b4d94ff94177e68e4a00cd6a17f3a3e39797 test
60b08de71d045bfdfc4a631799191b3462cd74d4 test
c2a8f484802b8112bf969cf2d921cb2b02a0425c first commit
```

`--mixed` 옵션을 사용해 변경점 저장이 진행되지 않은 상태로 이동한다.

```bash
$ git reset --mixed 60b08de71d045bfdfc4a631799191b3462cd74d4
Unstaged changes after reset:
D       test.txt
```

이후 변경점을 저장 후 다시 커밋하면 로그 메시지가 정리된 것을 확인할 수 있다.

```bash
wch18735@DESKTOP MINGW64 ~/Desktop/WCH/Github/[REPO] GitFirst (RB_1.0.1)
$ git log --pretty=oneline
4f26ba9a3af9e326bd199a0f72ca8773a3aa48cc (HEAD -> RB_1.0.1) organized git message
60b08de71d045bfdfc4a631799191b3462cd74d4 test
c2a8f484802b8112bf969cf2d921cb2b02a0425c first commit
```

실제로 며칠 전 기존에 push 된 commit 을 rollback 하는 일이 있었다. 영원한 선생님 구글에 검색한 뒤, 두근두근대며 진행했던 롤백은 다행히 잘 진행되었다. 그 때 진행했던 순서는 대략 아래와 같이 요약할 수 있다.

 1. 해당 repository 를 pull 받은 후 `git reset --mixed HEAD^` 로 상태를 변경한다.
 2. `git add .` 로 스테이징 저장
 3. `git commit -m "rollback"` 커밋
 4. `git push -u origin master -f` 원격 저장소에 반영