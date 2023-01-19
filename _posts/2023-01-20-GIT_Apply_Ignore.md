---
title: "[Git] .gitignore가 적용되지 않을 때"
excerpt: "how to apply .gitignore"
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

# .gitignore

가끔 .gitignore에 원격 저장소에 올리지 않을 파일이나 폴더를 지정하고 push를 수행해도 업로드되는 경우가 있다. 이런 경우 `git rm -r --cached .` 명령어를 통해 git 캐시를 삭제하고 커밋하면 정상적으로 .gitignore 파일에 설정한 값이 반영되는 것을 확인 할 수 있다.

```sh
git rm -r --cached .
git add .
git commit -m "clear git cache for .gitignore"
git push -u origin master
```