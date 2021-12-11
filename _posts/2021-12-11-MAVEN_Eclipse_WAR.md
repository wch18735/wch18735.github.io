---
title: "[Maven] pom.xml <packaging>war</packaging> 디렉토리 구조 변경 문제"
excerpt: "web archive directory changing issue"
date: 2021-12-11
layout: single
classes: wide
category:
    - maven
tag:
    - maven
    - maven error
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

Eclipse 에서는 `<packaging>jar</packaging>` 에서 `<packaging>war</packaging>` 로 패키징 방식을 변경하면 폴더 구조가 자동으로 변경된다.

이렇게 동작하지 않는 경우 Eclipse Enterprise Edition IDE 로 설치해야 한다.

- Help > Eclipse Marketplace > Eclipse Java EE Developer Tools 검색 > Install > 재시작

과정을 거친 뒤 `Alt + F5` 로 메이븐 프로젝트를 업데이트 해주면 디렉토리 구조 변경과 함께 `web.xml` 이 없다는 에러가 나타나는 것을 확인할 수 있다.

이렇게 프로젝트가 만들어지면 `mvn compile`, `mvn test` 등을 수행할 수 있다. 각각의 *Build Lifecycle 단계* 에 해당하는 명령어를 입력하면 이전 단계들은 자동으로 수행되는 것이 메이븐의 장점이라 할 수 있다.

<span style="font-size: 1.5em; font-weight: bold;">Build Lifecycle</span>

- validate - validate the project is correct and all necessary information is available
- compile - compile the source code of the project
- test - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
- package - take the compiled code and package it in its distributable format, such as a JAR.
- verify - run any checks on results of integration tests to ensure quality criteria are met
- install - install the package into the local repository, for use as a dependency in other projects locally
- deploy - done in the build environment, copies the final package to the remote repository for sharing with other developers and projects.

이를 더 자세히 나타내면 아래와 같이 `Default LifeCycle`로 나타낼 수 있다.

<html>
<body>
<!--StartFragment-->

validate | validate the project is correct and all necessary information is available.
-- | --
initialize | initialize build state, e.g. set properties or create directories.
generate-sources | generate any source code for inclusion in compilation.
process-sources | process the source code, for example to filter any values.
generate-resources | generate resources for inclusion in the package.
process-resources | copy and process the resources into the destination directory, ready for packaging.
compile | compile the source code of the project.
process-classes | post-process the generated files from compilation, for example to do bytecode enhancement on Java classes.
generate-test-sources | generate any test source code for inclusion in compilation.
process-test-sources | process the test source code, for example to filter any values.
generate-test-resources | create resources for testing.
process-test-resources | copy and process the resources into the test destination directory.
test-compile | compile the test source code into the test destination directory
process-test-classes | post-process the generated files from test compilation, for example to do bytecode enhancement on Java classes.
test | run tests using a suitable unit testing framework. These tests should not require the code be packaged or deployed.
prepare-package | perform any operations necessary to prepare a package before the actual packaging. This often results in an unpacked, processed version of the package.
package | take the compiled code and package it in its distributable format, such as a JAR.
pre-integration-test | perform actions required before integration tests are executed. This may involve things such as setting up the required environment.
integration-test | process and deploy the package if necessary into an environment where integration tests can be run.
post-integration-test | perform actions required after integration tests have been executed. This may including cleaning up the environment.
verify | run any checks to verify the package is valid and meets quality criteria.
install | install the package into the local repository, for use as a dependency in other projects locally.
deploy | done in an integration or release environment, copies the final package to the remote repository for sharing with other developers and projects.

<!--EndFragment-->
</body>
</html>