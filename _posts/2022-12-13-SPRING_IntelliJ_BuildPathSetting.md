---
title: "[Spring] IntelliJ 에서 webapps 자동 생성 안 될 때"
excerpt: "mapper xml file"
date: 2022-12-13
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - aop
    - aspectj
    - jdbc
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# IntelliJ에서 webapps 자동 생성이 되지 않을 때

Build 결과물이 생성되는 경로를 개발자가 자유롭게 설정할 수 있느냐는 중요한 문제인 것 같다. 실제로 vscode로 gradle 빌드를 작업하는 개발자분들께서 특히 많은 불편을 겪고 있으며, 최근까지도 [이슈](https://github.com/redhat-developer/vscode-java/issues/634#issuecomment-1341448118)가 되고 있는 것 같다. 링크를 쭉 읽어보면 이슈가 해결되지 않았는데 왜 닫았냐, Gradle은 해당 이슈를 확인해 변경해야 한다, 매번 이 문제로 골머리를 앓고 있다 등 부정적인 반응이 많다.

IntelliJ에서는 이 작업을 비교적 쉽게 수행할 수 있다. 먼저 나는 Windows10, IntelliJ, Gradle 환경에서 작업했다. 보통 Gradle 프로젝트를 생성한 뒤 main 메소드를 실행하면 excluded 상태(주황색) build 폴더가 생성된다. 그러나 tomcat 서버를 실행하는 경우, 직접 설정하지 않는 이상 webapps/WEB-INF/classes 경로를 탐색하기 때문에 빌드 경로가 변경되어야 한다.

먼저 Gradle 프로젝트의 Build 주체를 변경해주어야 한다. Ctrl + Alt + S 단축키를 누르면 프로젝트 설정 화면을 띄울 수 있다. Build, Execution, Deployment > Build Tools > Gradle로 이동하면 Build and run using: 항목이 Gradle (default)로 선택되어 있을 것이다. 이를 IntelliJ IDEA가 수행하도록 변경한다.

<img src="/_img/2022-12-13/build setting.png">

그리고 모듈의 build path를 변경해준다. 프로젝트를 클릭하고 F4를 누르면 Project Structure로 화면을 띄울 수 있다. 이곳에서 Modules를 클릭해주고 Paths 항목을 보면 기본적으로 Inherit project compile output path로 설정되어 있을 것이다. 이를 Use module compile output path로 변경하고 main의 Output path를 webapps\WEB-INF\classes로 변경해준다.

<img src="/_img/2022-12-13/build out path.png">

## 빌드 결과

이렇게 변경하고 main 메소드를 수행해 빌드하면 자동생성되는 것을 확인할 수 있다.

<img src="/_img/2022-12-13/webapps.png">