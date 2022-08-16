---
title: "[Pangtudy-web] Web Deployment to EC2"
excerpt: "web deployment on ubuntu server "
date: 2021-10-11
layout: single
classes: wide
category:
    - pangtudy-web
tag:
    - web development
    - ubuntu
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# EC2 인스턴스에 웹 배포

## 환경 세팅 과정

자바 설치
```bash
java -version
> Command 'java' not found, but can be installed with:
```

```bash
sudo apt install default-jre
```

깃 레포지토리 설치
```bash
sudo mkdir app
sudo mkdir app/git
```

깃 계정 설정
```bash
git config --global user.name "wch18735"
git config --global user.email "wch18735@naver.com"
```

깃 레포지토리 Clone (관리자 권한으로)
```bash
sudo git clone repo-url
```

npm 설치
```bash
sudo install npm
```

Vue 설치
```bash
sudo npm install vue
```

Vue CLI 설치 (3.x Version)
```bash
npm install @vue/cli -g
```

## Tips

- root login
- note references
- do not use backslash for Linux system

## Questions

- Jenkins 랑 Nginx 랑 같이 돌아갈 수 있나?
A: Maybe...

# References

- [AWS에 NodeJS 환경 구축](https://developer88.tistory.com/296)
- [Amazon EC2 인스턴스에서 Node.js 설정](https://docs.aws.amazon.com/ko_kr/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html)
- [보안 규정 세팅](https://dev-taem.tistory.com/3)
- [Vue 배포](https://dev-taem.tistory.com/3)