---
title: "[Maven] Maven 설치 및 등록"
excerpt: "maven install"
date: 2021-12-08
layout: single
classes: wide
category:
    - maven
tag:
    - maven install
toc: true
toc_label: "Contents"
author: 1FeS
comments: true
---

# Maven 이란?

> Apache Maven is a software project management and comprehension tool. Based on the concept of a project object model (POM), Maven can manage a project's build, reporting and documentation from a central piece of information.

공식 사이트 내용은 위와 같다. 즉, 메이븐이란 프로젝트를 관리 또는 응집하는 소프트웨어 도구이다. 메이븐은 프로젝트를 빌드하는 기능 외에도 리포트하거나 정보를 얻어온다.

## 설치 방법

메이븐을 설치하는 방법은 아래와 같다.

<span style="font-weight: bold; font-size: 1.3em;">1. 설치 파일 받아오기</span><br/>

[이곳](https://maven.apache.org/download.cgi)으로 들어가면 다운로드 할 수 있는 항목들이 보인다. *bin 파일* 을 설치하자.

<img src="/_img/2021-12-08/maven_bin.jpg" style="display: block; margin: auto auto; height: 10em"/>
<br/>

<span style="font-weight: bold; font-size: 1.3em;">2. 압축 풀기</span><br/>

원하는 폴더에 압축을 풀어준다. 이 때, 경로에 한글이 들어가면 오류가 발생할 수 있으니 되도록 알파벳으로만 이루어진 경로에 압축을 풀어주도록 한다.

<span style="font-weight: bold; font-size: 1.3em;">3. 환경변수 편집</span>

`내 PC > 속성 > 고급 시스템 설정 > 환경 변수` 창으로 들어간다. `시스템 변수`와 `시스템 변수 Path` 에 Maven 실행 파일의 위치를 추가할 것이다. 먼저 시스템 변수를 아래와 같이 추가한다. *bin (binary)* 상위 폴더까지의 경로를 적어준다.

<br/>
<img src="/_img/2021-12-08/maven_home.jpg" style="display: block; margin: auto auto; height: 25em"/>
<br/>

다음으로 만들어 준 시스템 변수를 이용해 실행파일 위치를 `Path`에 등록한다. `%MAVEN_HOME%` 아래 `bin` 폴더이기 때문에 `%MAVEN_HOME%` + `\` + `bin` 을 적어준다.

<br/>
<img src="/_img/2021-12-08/maven_path.jpg" style="display: block; margin: auto auto; height: 25em"/>
<br/>

제대로 설정되었다면 콘솔 창에서 `mvn --version` 명령어로 설치된 버전을 확인할 수 있다.

## 사용법

메이븐은 빌드 도구로 자바 프로젝트 생성 기능도 가지고 있다. 프로젝트를 생성하려는 위치에서 콘솔창을 열고 아래 명령어를 입력해보자.

```sh
mvn archetype:generate -DgroupId=com.example -DartifactId=javaproj -DarchetypeArtifactId=maven-archetype-quickstart
```

위에서 `archetypeArtifactId` 값은 누군가 이전에 만들어 놓은 프로젝트 기본 구조가 된다. 만약 내가 기본적인 내용을 만들어서 배포하면 `-DartifactId` 에 기재한 아이디를 이용해 다른 사람이 내려받아 사용하는 것도 가능하다. 

생성 중간에 나오는 *SNAPSHOT* 은 `현재 개발 중` 이라는 뜻이라고 한다. 생성은 아래와 같은 화면으로 마무리 된다.

<br/>
<img src="/_img/2021-12-08/create_proj.jpg" style="display: block; margin: auto auto; height: 15em"/>
<br/>