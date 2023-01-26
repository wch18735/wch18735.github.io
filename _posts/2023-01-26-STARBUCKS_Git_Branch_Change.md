---
title: "[Starbucks] Git Branch master 에서 main 으로 변경하기"
excerpt: "how to change git branch name master to main"
date: 2023-01-26
layout: single
classes: wide
category:
    - starbucks
tag:
    - html
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Git Branch "master" to "main"

1. Github Repository Settings로 이동
2. Branches 클릭
3. master 브랜치 이름을 main으로 수정

여기까지 진행하고 다시 레포지토리로 돌아가면 아래와 같은 알림을 준다.

<img src="/_img/2023-01-26/git branch change notice.png">

4. 로컬 레포지토리에서 아래 명령어 순차적 실행

```sh
git branch -m master main
git fetch origin
git branch -u origin/main main
git remote set-head origin -a
```