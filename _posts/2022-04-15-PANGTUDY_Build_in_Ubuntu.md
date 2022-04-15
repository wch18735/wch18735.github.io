---
title: "[Pangtudy-web] Ubuntu 에서 Maven Build"
excerpt: "build in ubuntu server"
date: 2022-04-15
category:
    - pangtudy-web
tag:
    - web development
    - spring boot
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# Spring Boot Project 배포

<span style="font-weight: bold; font-size: 1.2em">1. 인스턴스 접속</span>

<span style="font-weight: bold; font-size: 1.2em">2. Requirement 설치 및 세팅</span>

```cmd
sudo apt-get update && sudo apt-get upgrade
sudo apt install openjdk-11-jdk
sudo apt-get install mysql-server
```

이후 mysql 계정 및 데이터베이스 설정 수행.

<span style="font-weight: bold; font-size: 1.2em">3. git clone -b [branch] [repository]</span>

<span style="font-weight: bold; font-size: 1.2em">4. 해당 폴더 내부로 이동 후 빌드</span>

```cmd
sudo chmod -x mvnw
./mvnw clean package -DskipTests
```

- `package` : 컴파일 수행 후 소스를 packaging 양식에 맞춰 jar 또는 war 파일로 지정 경로에 디렉토리 생성
- `clean` : 이전 maven build 시 생성된 모든 자원 삭제
- `-DskipTests` : 빌드할 때 테스트 케이스 건너 뜀

<span style="font-weight: bold; font-size: 1.2em">5. 빌드 지정 경로 내부로 이동 후 실행</span>

```cmd
cd target/
nohup java -jar [builded jar file] &
```