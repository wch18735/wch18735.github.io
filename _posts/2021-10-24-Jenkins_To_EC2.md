---
title: "[pangtudy web] Install Jenkins to EC2"
excerpt: "Jenkins to EC2"
date: 2021-10-24
layout: single
classes: wide
category:
    - pangtudy web
tag:
    - web development
    - ubuntu
    - jenkins
    - ci/cd
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# AWS Ubuntu EC2 위에 Jenkins 설치 및 Github 연동

고난의 연속이다. 생각보다 많이 복잡하다. 먼저 사전 작업이다.

## SSH 연결 방법

1. ssh-keygen 수행
2. 단계 별 입력 (Default: 전부 Enter 눌러서 통과)
3. **default 경로**에 id_rsa 파일을 cat 으로 열기
4. private key 를 복사(ctrl + insert)해 첨부 
5. ssh 로 접근 테스트

※ defalut 경로 : /[account]/.ssh/id_rsa
※ 안 되는 경우 : /etc/ssh/ssh_config 에서 설정 확인

## java 11 jdk 설치

```cmd
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install openjdk-11-jdk
```

## java 이전 버전 삭제

```cmd
sudo apt-get purge openjdk*
```

<span style="font-weight: bold; font-size: 1.2em">jenkins 설치</span>

[https://pkg.jenkins.io/debian-stable/](https://pkg.jenkins.io/debian-stable/) 참조

## Jenkins EC2에 설치

`$ wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -`
`$ sudo sh -c 'echo deb https://pkg.jenkins.io/debian binary/ > /etc/apt/sources.list.d/jenkins.list'`
`$ sudo apt-get update`
`$ sudo apt-get install jenkins`

[여기](https://pkg.jenkins.io/debian/)를 참고했다.

## 8080 Port 열어주기

AWS Console 에서 Security - Inbound Rule 에 **8080 포트** 열어줘야 접속 가능. Jenkins 설정 파일을 보면 (`vim /etc/default/jenkins`로 확인 가능) HTTP 기본 포트가 8080인 것 볼 수 있다.

## 패스워드 확인 후 설치

접속에 성공하면 이렇게 화면이 나타난다.

<img src="/_img/2021-10-24/jenkins_init.jpg" style="margin: auto auto; display: block;"/>

하라는대로 첫 비밀번호를 확인해보고 넣어보자.

`$ vim /var/lib/jenkins/secrets/initialAdminPassword`

그러고 나면 설치화면이 나오는데, 나는 제안해주는 것 그냥 다 설치했다.

## Github repository TOKEN 발행

**Settings** → **Developer settings** → **Personal access token** 을 발행한다. 설정은 [여기](https://bcho.tistory.com/1237)를 참고했다.

## 계정 생성

[여기](https://chati.tistory.com/23)를 참고해서 계정을 생성했다. 로그인 할 때, 모든 사용자가 들어올 수 있도록 설정 파일에 보안을 **false**로 해놓으면 큰일이니까.

## Project Item 생성

[여기](https://kitty-geno.tistory.com/88)를 참고해서 프로젝트를 생성했다. **Credential**은 생성할 때 Error 페이지가 나타나는데, 새로고침을 한 번 해주면 정상적으로 추가되는 것을 확인할 수 있다.

각 속성이 의미하는 바가 뭔지 정리할 기회가 곧 올 것 같다.

## Add webhook

이렇게 웹훅을 등록해 놓으면 Push 할 때, 자동으로 빌드되는 것을 확인할 수 있다.

<br/><img src="/_img/2021-10-24/adjust_webhook.jpg" style="margin: auto auto; display: block;"/><br/>

[여기](https://medium.com/hgmin/jenkins-github-webhook-3dc13efd2437)에서 웹훅 등록하는 법을 참고했다.

## TO DO

- [ ] Web_Front Repository Jenkins 연동
- [ ] Build script 작성하기
