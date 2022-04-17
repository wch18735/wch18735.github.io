---
title: "[Pangtudy-web] 외부에서 WSL2 서버 연결"
excerpt: "build remote connection to wsl2 server"
date: 2022-04-18
category:
    - pangtudy-web
tag:
    - web development
    - spring boot
    - jenkins
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

Spring Boot, Node.js, Django 를 WSL2에서 처음 구동하면 로컬만 접속되고, 외부에서는 접속이 안 될 것이다. 아마 ping 도 날려보고, tcping 도 날려보고, telnet 도 해보고 여러가지 고생을 해가며 왜 안 될까를 고민했을 것이다.

문제는 사실 간단했는데, `외부 → 로컬` 로의 포트 포워딩이 된 것 처럼 `로컬 → WSL2` 로 포트포워딩을 해줘야 한다는 것이다.

- 로컬의 내부IP(192.168.0.17) -> WSL2 어댑터의 주소(172.20.17.129)

이 어댑터 주소는 cmd 에 ipconfig 를 쳐보면 알 수 있다. 그런데 매번 부팅마다 이 작업을 해주어야 할까, 라는 생각이 들던 찰나에 아래 스크립트를 보게 되었다.

```sh
$remoteport = bash.exe -c "ifconfig eth0 | grep 'inet '"
$found = $remoteport -match '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}';

if( $found ){
  $remoteport = $matches[0];
} else{
  echo "The Script Exited, the ip address of WSL 2 cannot be found";
  exit;
}

#[Ports]

#All the ports you want to forward separated by coma
$ports=@(80,443,10000,3000,5000,9090);


#[Static ip]
#You can change the addr to your ip config to listen to a specific address
$addr='0.0.0.0';
$ports_a = $ports -join ",";


#Remove Firewall Exception Rules
iex "Remove-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' ";

#adding Exception Rules for inbound and outbound Rules
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Outbound -LocalPort $ports_a -Action Allow -Protocol TCP";
iex "New-NetFireWallRule -DisplayName 'WSL 2 Firewall Unlock' -Direction Inbound -LocalPort $ports_a -Action Allow -Protocol TCP";

for( $i = 0; $i -lt $ports.length; $i++ ){
  $port = $ports[$i];
  iex "netsh interface portproxy delete v4tov4 listenport=$port listenaddress=$addr";
  iex "netsh interface portproxy add v4tov4 listenport=$port listenaddress=$addr connectport=$port connectaddress=$remoteport";
}
```

이를 윈도우 아무 폴더에 `.txt` 로 만들어 붙인 다음 `.ps1` 으로 변경해 실행하면 된다. 실행은 관리자 권한으로 실행한 PowerShell 에서 `.\[filename].ps1` 을 입력하면 된다. 끝으로 작업 스케줄러에서 로그온 할 때마다 3분의 지연 시간을 가지고 수행하면 끝!

이를 수행하기 이전에 아래 작업들이 완료되어야 한다. 

- Windows 방화벽에 해당 포트 Inbound, Outbound 설정
- WSL2 서버의 ifconfig 동작 가능
- WSL2 서버 방화벽 끄기 (내부 MySQL 서버와 Connection 이 맺어지는 과정에서 에러가 발생해 방화벽 Off 결정)