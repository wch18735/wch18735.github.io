---
title: "[JSP] 웹 어플리케이션 폴더 구조"
excerpt: "directory structure of eclipse and maven"
date: 2021-11-29
layout: single
classes: wide
category:
    - jsp
tag:
    - maven
    - eclipse
    - directory structure
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# 웹 어플리케이션 디렉토리 구조 (Eclipse vs Maven)

웹 어플리케이션 디렉토리 구조는 비슷하면서도 다르다. 이번 포스트에서는 Eclipse 와 Maven 디렉토리 구조를 살펴보고, 각 디렉토리 역할을 정리한다.

그 전에 자료 조사하며 알게 된 흥미로운 사실 한 가지 먼저 적는다. 실무에서는 Eclipse 보다 Maven 구조를 많이 사용한다는 글을 읽었다. 이유는 Eclipse IDE 디렉토리 구조에 종속되어 개발자끼리 대화가 통하지 않는 경우가 있다고 하는데... 아직 경험하지 못 했으니 판단은 보류.

## Eclipse Dynamic web project structure

```java
$workspace/ProjName
                /Java Resources
                    /src
                    /Libraries
                /build
                    /classes
                /src
                /WebContent
                    /META-INF
                    /WEB-INF
                      /lib
```

- **Java Resources**
  - src: Java 소스파일, 프로퍼티(.properties)파일 위치
  - libraries: WebContent/WEB-INF/lib 내부 라이브러리들을 가상으로 표현
- **build** 
  - 소스파일 컴파일 이후의 결과물 저장
  - 자바 클래스 파일(.class) 위치
  - Project Exlorer 에서는 class 파일은 보이지 않는 것이 Default 설정이므로 내용물이 보이지 않음
- **WebContent** 
  - `@WebServlet("/")` 에 해당하는 곳
  - HTML(.html), CSS(.css), JavaScript(.js), JSP, 이미지 파일 등의 웹 콘텐츠 저장
  - 웹 어플리케이션 서버 배포 과정에서 해당 폴더의 내용물 복사
  - **WebContent/WEB-INF :** 웹 어플리케이션 설정 파일들이 위치하는 디렉토리
    - 이 폴더에 있는 파일은 클라이언트에서 요청할 수 없음
  - **WebContent/WEB-INF/web.xml :** 웹 어플리케이션 Deployment Descriptor(배치 설명서, DD파일)
    - 서블릿, 필터, 리스너, 매개변수, Welcome Pages 등의 웹 어플리케이션 컴포넌트 배치 정보를 작성
    - 클라이언트 요청 처리 시 서블릿 컨테이너가 참조하는 파일 (서블릿 클래스 바인딩, 필터 실행 등)
  - **WebContent/WEB-INF/lib :** 자바 아카이브 파일(.jar)이 위치하는 디렉토리
    - jar(Java Archive)
      - zip 파일 형태의 압축파일
      - 여러 개의 자바 클래스 파일 
      - 클래스가 이용하는 관련 리소스(텍스트, 그림 등) 및 메타 데이터를 하나의 파일로 모아서 압축
      - 하나의 파일로서 자바 플랫폼에 응용 SW나 라이브러리를 배포하기 위한 파일 포맷

## Maven web application structure

```java
$workspace/ProjName
                /src
                   /main
                      /java
                      /resources
                      /webapp(=webContent)
                         /WEB-INF
                   /test
                      /java
                         /resources
                /bin
```

디렉토리 상세 내용
- **src :** 프로그램 소스 파일 위치 
  - **main/java :** 자바 소스 파일(*.java파일) 위치
  - **main/resources :** 프로그램 설정 파일(*.properties, *.xml 등) 위치
    - **main/webapp :** 정적 웹 자원 위치 (HTML, CSS, JavaScript, JSP)
      - **main/webapp/WEB-INF :** 웹 애플리케이션 정보 파일이 위치하며, 외부로부터 접근 차단
- **test :** 코드를 테스트하는 소스 파일 위치
  - **test/java :** 단위 테스트 관련 자바 소스 파일 위치
- **bin :** 소스코드가 컴파일된 *.class 파일, *.properties파일, *.xml파일 등 위치