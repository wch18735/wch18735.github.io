---
title: "[JSP] no main manifest attribute 에러 해결"
excerpt: "no main manifest attribute solve"
date: 2021-11-29
layout: single
classes: wide
category:
    - jsp
tag:
    - java
    - jsp
    - maven
    - maven error
toc: true
toc_label: "Contents"
author: 1FeS
comments: true
---

## no main manifest attribute

간단한 maven 프로젝트를 만들고 build 후 target 폴더에 생성된 jar 파일을 실행해봤다. 그랬더니 `no main manifest attribute` 에러가 생성됐다.

`jar -xvf JAR File Name` 을 통해 생성된 파일을 풀고, `cat META-INF/MENIFEST.MF` 파일 내용을 살펴봤다. 아래와 같이 아무 내용이 없었다.

```sh
Manifest-Version: 1.0
Created-By: Apache Maven 3.8.4
Built-By: My Account Name
Build-Jdk: 12.0.2
```

해당 에러는 main 함수를 찾을 수 없을 때 나타난다고 한다. 여러 방법을 시도하다가 아래 플러그인 활용법으로 에러를 해결했다.

$1.$ 프로젝트 root `pom.xml` 열기

$2.$ 아래처럼 `<plugin>` 이라 적힌 부분 찾기 

```xml
<!-- ... -->
<!-- ... -->
<!-- ... -->

        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
          <!-- HERE -->
        </plugin>

<!-- ... -->
<!-- ... -->
<!-- ... -->
```

$3.$ `HERE` 에 아래 플러그인을 삽입. 이 때, `<mainClass>` 임을 주의.

```xml
<!-- manifest plugin -->
          <configuration>
                    <archive>
                        <manifest>
                            <mainClass>Your.Package.And.Class.Name</mainClass>
                        </manifest>
                    </archive>
          </configuration>
<!-- manifest plugin -->
```

완성된 pom.xml 파일은 다음과 같은 형태다. 메인 클래스 태그 내부에는 패키지 이름과 클래스 이름을 적는데, `main 함수` 가 있는 패키지까지의 경로와 클래스 이름을 적으면 된다. maven 에서 제공하는 프로젝트 `archetype` 을 기반으로 아무 설정 없이 프로젝트를 만들었다면 `com.example.App` 이 된다.

```xml
<!-- ... -->
<!-- ... -->
<!-- ... -->

        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
          <configuration>
                    <archive>
                        <manifest>
                            <mainClass>Your.Package.And.Class.Name</mainClass>
                        </manifest>
                    </archive>
          </configuration>
        </plugin>

<!-- ... -->
<!-- ... -->
<!-- ... -->
```

## Plug-in 이란?

>플러그인(plugin) 또는 추가 기능(애드인;add-in, 애드온;add-on)은 호스트 응용 프로그램과 서로 응답하는 컴퓨터 프로그램이며, 특정한 "주문식" 기능을 제공한다.
<br/> 출처: 위키백과

보통의 응용프로그램에 없는 기능을 사용할 수 있도록 돕는 것. 웹 브라우저에서 음악을 듣거나 PDF를 열어보는 행위가 가능하게 하는 것이 Plug-in 이다.