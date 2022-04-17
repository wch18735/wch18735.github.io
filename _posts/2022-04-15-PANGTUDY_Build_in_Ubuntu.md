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

nohup (no hang up) 을 통한 백그라운드 실행을 해보자.

백그라운드에서 실행시킬 때, `nohup` 명령을 위해서는 실행 파일의 권한은 755 이상이어야 한다. 

```sh
sudo chmod 755 [builded jar file]
```

그리고 아래 명령어로 동작시켜본다.

```sh
cd target/
nohup java -jar [builded jar file] &
```

`nohup: ignoring input and appending output to nohup out` 이라는 메시지가 나타날 것이다.

```sh
cat nohup.out
```

위 명령어로 메시지를 확인해보자. 이들이 백그라운드에서 잘 돌아가고 있는지 확인하려면 아래 명령어로 확인 가능하다.

```sh
bg
jobs
ps -ef | grep [builded file name]
```

<span style="font-weight: bold; font-size: 1.2em">6. 프로세스 종료</span>

끝으로 프로세스 종료는 위 `ps -ef | grep [builded file name]` 명령어로 알아낸 프로세스 번호를 통해 수행한다.

```sh
kill -9 [pid]
```

<span style="font-weight: bold; font-size: 1.2em">7. 포트 개방</span>

모든 세팅이 완료된 뒤 postman 으로 요청을 보냈을 때, 닿지 않는 경우가 있다.

이는 방화벽을 확인해봐야 한다. 포트를 개방해줘야 한다.

```sh
sudo apt install net-tools
```

먼저 `net-tools` 를 설치한다. 그리고 열려있는 포트들을 확인해보자.

```sh
netstat -n -a -p
```

예를들어 spring boot application 을 8080 포트로 받기로 했는데 열려있지 않은 경우에 `ERROR: connect ETIMEOUT IP` 가 나타날 수 있다.

AWS 인스턴스는 콘솔에서 보안 그룹을 편집하는 작업을 직접 수행한다고 생각하면 되겠다.

```sh
# 추가
iptables -I INPUT 1 -p tcp --dport 9090 -j ACCEPT
iptables -I INPUT 1 -p udp --dport 9090 -j ACCEPT

# 삭제
iptables -D INPUT 1 -p tcp --dport 9090 -j ACCEPT
iptables -D INPUT 1 -p tcp --dport 9090 -j ACCEPT
```

이렇게 추가한 방화벽은 아래 명령어로 확인 가능하다.

```sh
# 방화벽 정책 확인
iptables -nL
```

마지막으로 wsl2 에서 실행하고 있다면 아무리 머신 방화벽을 열어줘도 Windows 설정이 막혀있다면 접속이 불가능하다. 먼저 안전한 포트로 바꿔준다.

`application.properties` 에 `server.port=9090` 등을 잡아준다. 그리고 윈도우 방화벽에서 해당 포트를 열어준다.