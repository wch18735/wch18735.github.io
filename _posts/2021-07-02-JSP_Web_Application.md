---
title: "[JSP] 웹 기반 클라이언트/서버 프로그램"
excerpt: "웹 기반 클라이언트/서버 프로그램 개요"
date: 2021-07-02
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - client
    - server
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 웹 기반 클라이언트/서버 프로그램

웹 브라우저의 요청에 따른 페이지 생성 과정을 나타내보면 아래와 같습니다.

![web/server diagram](/_img/2021-07-02/web_client_model.jpg)

브라우저로부터 요청을 받으면 서버는 요청에 따른 동작을 수행합니다. 먼저 정적 웹 페이지를 부르는 동작은 단순히 저장된 웹 페이지를 찾아 보내주게 됩니다. 이를 순서대로 나타내보면 **① → ② → ⑤** 로 나타낼 수 있습니다.

그렇다면 동적인 웹 페이지는 어떻게 만들어질까요? **④번** 에 표시된 **Optional**은 동적인 웹 페이지를 생성할 때 추가됩니다. 브라우저의 요청이 데이터베이스에 저장된 값에 따라 달라지기에 이러한 동작이 명시된 코드가 작성되어있습니다. 이러한 코드를 해석하고, 실행해주는 것을 **WAS(Web Application Server)** 라 합니다.

위에서 동적으로 문서를 만드는 코드를 **server application** 이라 합니다. 즉, 실행 환경은 운영체제와 저장공간을 제공하는 **Web Server** 자체와 클라이언트 요청을 동적 처리하는 **WAS**, 그리고 동작들이 명세되어있는 코드 문서 **Server application**로 구성된다고 할 수 있습니다.

