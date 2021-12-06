---
title: "[JSP] Eclipse Marketplace 에러 해결"
excerpt: "eclipse marketplace error"
date: 2021-12-06
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - servlet
    - eclipse
toc: true
toc_label: "Contents"
author: 1FeS
comments: true
---

# Eclipse Marketplace error 해결

Eclipse 설치 폴더 내부의 `eclipse.ini` 파일을 아래와 같이 수정한다.

```sh
...
-vmargs
-Djavax.net.ssl.trustStoreType=Windows-ROOT
-Djavax.net.ssl.trustStore=NUL
```

여기서 각 옵션은 다음과 같다고 한다.

- Windows-ROOT : 윈도우 PC 에서 루트인증서 업데이트
- vmargs : virtual machine arguments
- NUL : NULL

아래 사이트에서 Eclipse 를 받고 `eclipse.ini` 를 받으면 프록시 설정을 따로 해주지 않아도 21-09 버전에서 Spring 개발 환경을 설정할 수 있다.

- [Eclipse 21-09 Version](https://www.eclipse.org/downloads/download.php?file=/technology/epp/downloads/release/2021-09/R/eclipse-jee-2021-09-R-win32-x86_64.zip)