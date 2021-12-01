---
title: "[JSP] 톰캣 startup.bat 실행오류 해결 및 활용 예제"
excerpt: "톰캣 설치 및 startup.bat 실행오류 해결, 예제"
date: 2021-07-03
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - tomcat
    - tomcat install
    - startup.bat
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# Tomcat 설치 및 정상 실행 확인

Tomcat 은 WAS 역할을 수행합니다. 처음 톰캣을 설치할 때, 실행 오류 원인들이라고 생각했던 몇 가지를 기록합니다.

## 설치

[여기](https://tomcat.apache.org/)에서 다운받을 수 있습니다. 본인의 버전에 맞는 Tomcat을 설치해주면 됩니다. 서비스를 목적으로 한다면 **Service Instraller**, 개발을 연습하기 위한 간단한 용도로는 **zip** 파일을 받아주면 됩니다.

## 실행 확인

다운로드 후 **apache-tomcat-9 폴더**의 **bin** 안에 들어갑니다. 그리고 **startup.bat** 파일을 더블 클릭하여 실행하고, console 창이 꺼지지 않고 켜지는지 확인합니다. 이렇게 console 창에 나타납니다.

![tomcat console](/_img/2021-07-05/tomcat_console.jpg){: .align-center}

이런 창이 나타나지 않는다면 몇 가지 이유를 생각해볼 수 있습니다.

- java 환경 변수 설정
- 다른 프로그램의 port 8080 점유 (예: vscode)
- tomcat 과 java version 차이가 많이 나는 경우

저도 처음 배치 파일을 실행시켰을 때, 깜빡하고 꺼지는 현상이 생겼습니다. 컴퓨터를 다시 시작하고, **Java JDK 12** 를 다운받아 해당 폴더로 환경변수를 바꾸었습니다. 다행히 배치 파일은 정상 동작했습니다.

이렇게 창이 나타나면 인터넷 브라우저에 `localhost:8080`을 입력해 다음과 같이 tomcat 기본 페이지가 나타나는지 확인합니다.

![tomcat console](/_img/2021-07-05/tomcat_page.jpg){: .align-center}

이 페이지까지 확인하셨다면 톰캣이 잘 설치된 것입니다. 그럼 간단하게 톰캣을 어떻게 활용할 수 있는지 보겠습니다.

## 웹 문서 요청 및 응답 확인

톰캣은 **WAS** 기능을 담당한다고 했습니다. 문서에 적힌 코드를 실행하여 동적인 웹 페이지를 제공하는 역할입니다. 그렇다면 정적인 웹 페이지는 제공할 수 없을까요? 가능합니다. 아래와 같은 과정을 통해 알아볼 수 있습니다.

먼저 **startup.bat** 을 실행해 톰캣을 켜줍니다. 그리고 간단한 텍스트 문서를 작성해 톰캣이 문서를 보관하는 *Home Directory* 에 저장해줍니다. 저장 위치는 `~\apache-tomcat-9.0.48\webapps\ROOT` 입니다.

어떤 글을 적을지 생각 안 나면 간단하게 `hello.txt` 를 만들고 내용을 **Hello Tomcat!** 이라고 작성해줍니다.

```txt
Hello Tomcat!
```

잘 저장해줬다면 웹 브라우저에서 해당 파일을 요청해봅니다.

![hello tomcat](/_img/2021-07-06/hello_tomcat.jpg){: .align-center}

웹 브라우저에서 다음과 같이 `localhost:8080/hello.txt`를 입력해서 확인해볼 수 있습니다. 아래 화면과 같이 브라우저가 요청한 문서를 표시해주면 정상적으로 동작하는 겁니다.

![hello tomcat](/_img/2021-07-06/hello_tomcat2.jpg){: .align-center}

만약 본인이 공인IP를 사용하고 있다면 이제 세계 어디에서든 본인의 아이피와 포트번호, 파일 명을 통해 이 문서에 접근할 수 있습니다. 물론 방화벽을 비롯한 여러 보안 문제들이 존재합니다. 

## context site

**Context site**는 규모가 있는 웹을 개발할 때, 개발자들에게 편리함을 제공하기 위한 방법입니다. 브라우저는 다음과 같은 형태로 요청을 보내도록 합니다.

`http://IP:Port/context/resource`

이때, *context* 란 일종의 가상화 방법이라 생각해주시면 이해가 쉬울 것 같습니다. 물리적인 위치를 *ROOT* 디렉토리 내의 상대값으로 변환하여 나타내줍니다. 그 과정을 살펴보겠습니다.

먼저 webapps 폴더에 `~/ITWEB/news.txt`로 폴더와 텍스트를 만들어줍니다. 그리고 해당 파일에 원하는 내요을 적어주겠습니다. 저는 실습을 진행하는 날짜를 적었습니다.

상대경로 지정이 끝나지 않은 상태에서 `http://localhost:8080/it/news.txt`로 요청을 보내면 다음과 같은 화면이 나타납니다.

![hello tomcat](/_img/2021-07-06/before_context.jpg){: .align-center}

왜냐하면 디렉토리 구조가 보낸 요청과 일치하지 않기 때문입니다. `ROOT` 내부에는 `it` 폴더와 `news.txt` 파일이 존재하지 않습니다. 이때, Context를 지정해주어 ROOT 폴더 외부 그 어떠한 곳에 있는 파일이라도 찾아갈 수 있도록 만들어줄 수 있습니다.

![context process](/_img/2021-07-06/context_process.jpg){: .align-center}

그림에서 보는 것 처럼 **실제 구조**를 상대경로 설정을 통해 **가상화된 형태**로 나타낼 수 있습니다. 상대경로는 어떻게 지정할 수 있을까요?

`aparche-tomcat-9.0.48/conf/server.xml`의 *xml* 파일에 원하는 **문맥(context)** 을 추가함으로써 동작시킬 수 있습니다. `docBase`는 실제 위치이고, `path`는 *ROOT* 폴더 내부에서 가상화된 위치* 를 표현합니다.

![hello tomcat](/_img/2021-07-06/context_add.jpg){: .align-center}

이 작업을 마무리해주신 뒤 서버를 재시작 해줍니다. 그 다음 기존 요청을 다시 보내보면 정상적인 응답을 확인할 수 있습니다.

![hello tomcat](/_img/2021-07-06/after_context.jpg){: .align-center}

이는 매우 유용하게 사용됩니다. 대규모 웹 개발 과정에서 ROOT 폴더 밖에서도 서로 다른 부서가 개발을 진행할 수 있는 편리함을 제공합니다. 또한 각각의 하위 폴더를 하나의 ROOT 처럼 다룰 수 있기 때문에 수정 및 배포에 간편함을 기대할 수 있습니다.

## References
- [뉴렉처](https://www.youtube.com/channel/UC5-ixpj8DioZqmrasj6Ihpw)
