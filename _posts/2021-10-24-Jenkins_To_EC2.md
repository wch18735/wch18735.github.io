---
title: "[Pangtudy-web] Install Jenkins to EC2"
excerpt: "Jenkins to EC2"
date: 2021-10-24
category:
    - pangtudy-web
tag:
    - web development
    - ubuntu
    - jenkins
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# AWS Ubuntu EC2 위에 Jenkins 설치 및 Github 연동

고난의 연속이다. 생각보다 많이 복잡하다.

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

**Settings** → **Developer settings** → **Personal access token** 을 발행한다. 설정은 [이곳](https://bcho.tistory.com/1237)을 참고했다.

## Project Item 생성

[여기](https://kitty-geno.tistory.com/88)

## Add webhook

이렇게 웹훅을 등록해 놓으면 Push 할 때, 자동으로 빌드되는 것을 확인할 수 있다.

<img src="/_img/2021-10-24/adjust_webhook.jpg" style="margin: auto auto; display: block;"/>

[여기](https://medium.com/hgmin/jenkins-github-webhook-3dc13efd2437)

