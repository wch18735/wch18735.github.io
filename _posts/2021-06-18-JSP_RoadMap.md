---
title: "[JSP] 웹 개발자를 위한 학습 로드맵"
excerpt: "웹 개발 로드맵 정리"
date: 2021-06-19
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - web development
    - roadmap
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 웹 개발자를 위한 학습 로드맵

이 문서는 여러 인터넷 정보들을 나름대로 정리한 포스트입니다. 따라서 틀린 내용이 있을 수 있습니다. 많은 분들의 피드백 대환영입니다! 

웹 개발자는 다음과 같이 세 부분으로 나눠질 수 있습니다.

- **Web 퍼블리셔:** 웹 문서를 만드는 역할
- **Front-end 개발자:** 웹 문서를 동적으로 만드는 역할
- **Back-end 개발자:** 웹 문서를 동적으로 만드는 역할

**Front-end**와 **Back-end** 모두 웹 문서를 동적으로 만드는 역할을 합니다. 그럼 둘의 차이는 무엇일까요. 바로 `어디에서 문서를 동적으로 만드는가` 입니다.

![client server](/_img/2021-06-18/client_server.jpg){: .align-center}

위의 그림에서 **클라이언트**는 특정 페이지를 요청합니다. 웹 서버는 이 요청에 해당하는 페이지를 찾고, 이를 돌려주게 됩니다. 먼저 웹 서버에 미리 존재하는 특정 페이지들을 만드는 일을 **웹 퍼블리셔**가 담당합니다. 

그런데 이러한 페이지들은 웹 서버에서 동적으로 제작되어 클라이언트에 보내질 수 있습니다. 예를들어 게시판의 첫 번째 작성된 게시글에 따라 달라질 것입니다. 이렇게 동적으로 웹 페이지를 관리 및 제작하는 서버를 제작하는 개발자를 **백엔드 개발자**라고 합니다.

반대로 **프론트 엔드 개발자**는 클라이언트 위치에서 웹 문서를 관리합니다. 프론트 엔드 개발자가 하는 일들의 예시로 웹 페이지의 다크 모드 개발, 모바일 전용 화면 설계, 컴포넌트 응답 및 반응 제작과 같은 작업을 수행합니다.

## 프론트 엔드 개발자

웹 퍼블리셔는 웹 문서를 만들어주는 기술자입니다. 이들은 클라이언트 측에서 개발을 진행하기 때문에 **Web browser**에 대한 공부를 해야합니다. 기본적으로 공부해야 할 것들은 다음과 같습니다.

- **HTML**
- **CSS**
    - Bootstrap / Material
    - SASS / LESS

Bootstrap, SASS 등은 개발자가 처음부터 하나하나 CSS를 제작하지 않아도 되는 편리함을 제공합니다. HTML 문서와 CSS 문서를 이용해 기본적인 웹 페이지를 제젝할 수 있습니다. 

이렇게 제작된 문서에서 HTML 태그로 나타나는 하나의 부분은 매칭되는 CSS 요소와 결합된 하나의 객체처럼 다뤄질 수 있습니다. 그리고 이러한 객체를 관리하는 것을 **DOM**이라 합니다. 여기에 **Javascript**를 더하면 클라이언트 측에서 동적인 웹을 제작하는 것이 가능해집니다.

- **DOM**
- **javascript**
    - ES6
    - ES7
    - ES8
    - Typescript

**DOM**은 브라우저마다 종료와 버전을 체크해야하는 어려움이 있었으나, **jQuery**를 이용해 이를 해결했습니다. 그치만 이 역시 아주 옛날의 이야기이며 지금은 DOM을 이용해 객체를 직접 조작하지 않으며 다음과 같은 기술을 사용합니다.

- **HTML5 API**
- **jQuery**
- **Angular / React.js / Vue.js**

## 백엔드 개발자

백엔드 개발자의 스택은 아래 표와 같이 나눠질 수 있습니다.

|**LAMP**|**WISA**|**Java**|**MEAN**|
|---|---|---|---|
|**L**inux|**W**indows|**Java**|**M**ongoDB|
|**A**pache|**I**IS|WAS|**E**xpress|
|**M**ySQL|MS-**S**QL|Oracle|**A**ngular|
|**P**HP|**A**SP.net|JSP|**N**odeJS|

아직까지는 Java 스택이 가장 많이 사용되고 있습니다. 취업 과정에서 가장 많이 요구되는 스택이기 때문에 이 부분이 중요할 것이라 생각되어 이를 정리했습니다.

- **JVM**
    - JDK
- **WAS**
    - Servlet
    - JSP
- **DBMS**
    - JDBC

Java 언어를 실행할 수 있는 환경을 **JVM**이라 하고, JDK 내부 모듈과 클래스들을 이용해 연산을 수행할 수 있습니다. **WAS**는 동적인 웹을 제작하는 역할을 하며 Servlet과 JSP를 이용해 개발합니다. DBMS는 데이터베이스를 관리하는 것으로 Java는 **JDBC**를 사용합니다. 이러한 Java WEB API들과 Java 언어를 이용해 웹 서버를 개발할 수 있습니다.

여기에서 Spring, ORM 등을 사용하면 조금 더 간결한 코드로 개발이 가능해집니다. 이와 관련된 내용들은 다음주부터 진행되는 6개월짜리 스터디와 프로젝트를 통해 공부하고 정리할 예정입니다. 위메프, CJ 현직 개발자와 함께하는 스터디라 많이 기대가 됩니다.