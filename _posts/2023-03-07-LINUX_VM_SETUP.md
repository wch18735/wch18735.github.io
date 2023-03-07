---
title: "[Linux] VM Argument Setup"
excerpt: "vm args setting"
date: 2023-03-07
layout: single
classes: wide
category:
    - linux
tag:
    - linux
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# VM Args 설정

개발을 하다 보면 최대한 서버 설정과 맞추기 위해 특정 arguments를 전달하여 실행하는 경우가 있다. Eclipse, Intellij 같은 IDE들에서는 모두 이 기능을 지원한다. 가령, Run configuration에서 아래와 같은 설정 값을 vm args에 작성하고 코드를 실행하는 경우를 보자.

- Dspring.profiles.active=dev
- Djasypt.encryptor.password=password

이건 사실 실제 서버의 `tomcat > bin > catalina.sh` 아래 `JAVA_OPTS="$JAVA_OPTS -Dspring.profiles.active=tobe-dev"`를 설정하는 것과 같은 것이다. 실제 서버에 접속해 해당 셸에서 JAVA를 검색해보면 앞서 적어 놓은 설정들을 볼 수 있다. 

참고로, `catalina.sh`을 건드리면 설정일 꼬일 위험도 있기 때문에, `setenv.sh` 을 만들어 별도로 관리하도록 권장한다. 가령 jasypt 적용을 위해 `export JAVA_OPTS="-Djasypt.encryptor.password=cheolhwangwon"` 과 같이 값을 설정할 때도 `$CATALINA_HOME/bin/` 아래에 `setenv.sh` 을 만들어 삽입해주는 것을 추천한다. 실제로 `catalina.sh`을 보면 현재 디렉토리 안에 `setenv.sh`이 있으면 해당 스크립트를 실행하는 부분이 있다.

## Error를 잘 읽자

개발자가 미처 챙기지 못하는 부분은 PM이 챙겨야 한다. 이전에는 완료QA를 위해 github 이름을 변경하고, jenkins에서 거침없이 Build now를 눌렀는데, 갑자기 사이트 접속이 끊어졌다. 부랴부랴 서버에 접속해 로그를 살펴보니 아래와 같은 에러가 있었다.

```sh
Caused by: java.lang.IllegalArgumentException: The main resource set specified [/server/apache-tomcat-8.5.27/publish/WRONG_SYSTEM_NAME-1.0.0-RELEASE] is not valid
        at org.apache.catalina.webresources.StandardRoot.createMainResourceSet(StandardRoot.java:749)
        at org.apache.catalina.webresources.StandardRoot.startInternal(StandardRoot.java:706)
        at org.apache.catalina.util.LifecycleBase.start(LifecycleBase.java:150)
        ... 9 more
```

과거에는 에러 읽기가 너무 귀찮아 읽기를 포기하고 직관 리팩토링을 즐기곤 했다. 사실, 에러를 잘 읽어보면 무엇이 문제인지 알 수 있다. 문제에 답이 있는데, 보기만 읽으면서 답을 찍었던 것 같다. 위에서 설명하는 에러는 `[/server/apache-tomcat-8.5.27/publish/WRONG_SYSTEM_NAME-1.0.0-RELEASE]` 라는 resource가 유효하지 않다는 뜻이다.

생각해보니 github 이름을 변경하면 릴리즈 war 파일 이름이 바뀌고, 폴더명은 자연스럽게 바뀌는데 docBase 설정은 그대로 남아있었기 때문에 발생하는 것이었다. `config/server.xml` 내부 `<Host>` 태그 내용에서 docBase를 바꿔주고 다시 실행했더니 정상적으로 동작했다.

## Defining the context path from the server.xml

server.xml 설정하며 많은 어려움을 겪었다. 다행히 [How to Deploy a WAR File to Tomcat](https://www.baeldung.com/tomcat-deploy-war)라는 페이지를 찾아 하나하나 공부해가니 가닥이 잡혔다. 아래는 공부한 내용 중 일부를 짤막하게 정리한 내용인다.

`<Context>` 요소를 `<Host>` 안에 사용하면 WAR 파일, 배포 폴더 경로를 변경해 배포하는 것이 가능하다. 아래와 같은 태그가 있다고 가정하자.

```xml
<Host name="localhost"  appBase="webapps"
      unpackWARs="false" autoDeploy="false" deployOnStartup="false">
      <Context path="/mydemo/version1" docBase="demo#v1.war"/>
      ...
</Host>
```

`docBase`는 WAR 파일이나 압축해제된 배포 폴더 경로를 나타내는데, tomcat 내부 webapps 디렉토리로부터 상대 경로나 서버 내 절대 경로로 표현된다. 일반적으로 `docBase`는 tomcat 내부에 application을 설치하지 않거나 `path`를 설정하기 위해 사용한다.

어플리케이션 이름이 ExampleApp이라면 context path는 `http://localhost:8080/ExampleApp`로 설정된다. 이걸 막기 위해 Context 요소의 path 속성을 `path="/"`으로 지정하게 되면 context path를 `http://localhost:8080/`로 설정할 수 있게 된다. 이 때, 중복으로 배포하는 것을 막기 위해 반드시 `autoDeploy="false"`, `deployOnStartup="false"` 설정을 해줘야 한다. 반대로, 만약 `autoDeploy="true"` 인 경우라면, 보통의 경우 WAR 파일을 `appBase="webapps"`로 명시된 tomcat 내부 webapps 폴더에 위치시켜 자동으로 배포가 수행된다.

결론적으로는 Tomcat 5 버전 위부터는 이런 context 설정이 배포 담당자에게 혼란을 주며, server.xml 파일 수정에 따른 tomcat restart를 피할 수 없게 만들어 추천되지 않는 방법이라고 한다. 대신 `$CATALINA_HOME/conf/Catalina/localhost` 폴더에 `ROOT.xml`을 만들고, `<Context docBase="../deploy/ExampleApp.war">` 와 같이 기재하는 것을 추천한다고 한다.

