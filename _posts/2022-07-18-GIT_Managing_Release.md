---
title: "[Git] Git Release 관리"
excerpt: "Git Release 관리"
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

# Git Release 관리

Git 을 통해 버전 관리를 수행하는 전통적인 방법을 정리한다.

## commit 

1. `git commit -m "message1" -m "message2" ...`
`-m "message"` 는 하나의 문단이 된다. `git log` 를 이용해 확인해보면 실제로 두 문단으로 나타나는 것을 볼 수 있다.

2. `git commit -a` or `git commit --amend`
해당 명령어는 마지막 커밋 메시지를 수정한다. 

## Tag 삽입

태그는 `git tag [tag 명] [branch 명]` 으로 나타낼 수 있다. 예를 들어, `git tag 1.0 develop` 이라 하면 develop 브랜치에 1.0 태그를 삽입하겠다는 뜻이다.

## Rebase

git 에서 한 브랜치를 다른 브랜치로 합치는 두 가지 방법 중 하나. 하나는 Merge 이다. Rebase 명령으로 변경된 사항을 다른 브랜치에 적용할 수 있다.

`git rebase [branch 명]` 로 사용 가능하다. `git checkout [branch 명]` 으로 이동한 브랜치에서 Rebase 할 브랜치 명을 나타낸다.

```bash
git checkout main
git rebase BR_1.0
```
실제로 일어나는 일은 아래와 같다. 
- 두 브랜치가 나뉘기 전 공통 커밋으로 이동
- 공통 커밋으로부터 checkout 한 브랜치가 가리키는 커밋까지 diff 를 차례로 만들어 임시 저장
- Rebase 할 브랜치(main 브랜치)가 합칠 브랜치(BR_1.0 브랜치)가 가리키는 커밋을 가리키게 함
- 임시 저장했던 변경사항을 차례대로 적용

이렇게 하면 BR_1.0 에 기록된 변경 사항들이 모두 main 브랜치에 적용된다. 끝으로 `git branch -d BR_1.0` 명령어를 통해 삭제하면 main 브랜치에 변경 내용을 모두 기록할 수 있으며, 이전에 적용한 태그도 사용할 수 있다.

## tag 로부터 branch 하기

현재 main 브랜치에서 1.0.x 에 대한 패치를 적용하려면 태그에서 브랜치를 만들어야 한다. 이를 위해 `git branch [분기 branch 명] [tag 명]` 형태로 명령어를 사용할 수 있다.

실제로 변경한 뒤 `git log --pretty=oneline` 을 통해 commit 메시지들을 확인해보자. 태그 삽입 전 메시지들까지만 나타나는 것을 확인할 수 있다.

## Archive

릴리스에 대한 압축 파일 생성 역시 중요하다. 릴리스할 때 프로젝트 이력을 함께 배포하는 일은 드물다. 대개는 태그를 붙여둔 내용만을 tar 나 zip 파일 형태로 제공하는 것으로 충분하다.

gzip 으로 압축된 mysite 의 tar 파일을 생성하는 명령어는 다음과 같다.

`git archive --format=[압축 포맷] --prefix=[경로] [태그] | gzip > [압축 파일명].gz`

실제 사용 예시는 다음과 같이 `git archive --format=tar --prefix=mysite-1.0/ 1.0 | gzip > mysite-1.0.tar.gz` 로 사용할 수 있다.

## 원격 저장소 복사

원격 저장소 복사는 clone 명령어를 사용한다. 보통은 `git clone [저장소 위치] [디렉터리 명]` 형태로 사용하거나 저장소 위치 매개변수만을 사용한다. 여기서 디렉터리 명을 지정해주면 저장소를 복사하며, 복사되는 디렉토리 이름을 동시에 지정할 수 있다.