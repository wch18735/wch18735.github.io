---
title: "[Todo App] REST API 인증"
excerpt: "jwt: json web token"
date: 2022-12-11
layout: single
classes: wide
category:
    - todo app
tag:
    - java
    - rest api
    - jwt
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# REST API 인증 기법

기본적인 백엔드 기능 개발을 수행한 다음은 인증(Certification)과 인가(Authorization) 영역을 다루게 된다. 내가 개발한 API를 모든 사용자가 활용하는 것을 방직하는 것이다.

## BASIC 인증

기본적인 인증 방법은 HTTP 요청 헤더의 Authorization: 부분에 아이디와 비밀번호를 콜론으로 이어 붙인 후 Base64로 인코딩한 문자열을 함께 보내는 것이다. 이 HTTP 요청을 수신한 서버는 문자열을 디코딩해 아이디와 비밀번호를 찾아낸 뒤, 데이터베이스 정보와 대조해 가입된 사용자인지 확인해본다.

