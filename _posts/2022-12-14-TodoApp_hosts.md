---
title: "[Todo App] hosts 파일"
excerpt: "hosts file for web development"
date: 2022-12-14
layout: single
classes: wide
category:
    - todo app
tag:
    - hosts
    - web development
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# hosts 파일

처음 hosts 파일을 접한건 사내에서 프로젝트를 수행할 때였다. 당시는 아직 개발계 자원도 할당받지 않은 상태이고 사내 DNS에 주소도 등록되지 않은 상황이었다. localhost 뿐만 아니라 URL로 테스트를 진행하기 위해 hosts 파일을 세팅했었던 기억이 있다. 뿐만 아니라 타 시스템의 개발계에 접속할 때도 hosts 파일에 어떤 작업을 해주었던 것 같다. 이런 hosts 파일은 `C:\Windows\System32\drivers\etc`에 위치해 있다.

## Windows 운영 체제에서 hosts 파일

hosts 파일은 Windows 운영 체제에서 사용하는 파일이다. 그렇기 때문에 맥북으로 작업을 수행하는 사용자는 생소할 수 있다. 이 파일에는 호스트 이름과 이에 대응하는 IP 주소가 저장되어 있으며, **인터넷 접속 시 컴퓨터가 가장 먼저 확인하는 파일**이다. 

전화번호 기억도 어려운데, 거기에 접속하려는 웹의 IP 주소를 기억하고 식별하기는 더욱 쉽지 않을 것이다. 그렇기 때문에 서버에 wch18735.github.io과 같은 이름(호스트 이름)을 부여하고 그 이름을 사용한다. 그런데, 현대에 컴퓨터를 사용한 우리들에겐 도메인 이름 체계(FQDN)가 매우 익숙하나 사실 생긴지는 얼마되지 않았다고 한다. 인터넷이 보편적으로 사용되기 이전 시대에는 hosts라는 텍스트 파일에 컴퓨터의 IP 주소와 이름을 기록해두고, 사용자가 컴퓨터 이름을 사용해 통신 연결을 요청하면 운영 체제가 hosts 파일에서 컴퓨터 이름에 대응하는 IP 주소와 통신을 수행하는 방식을 사용했다고 한다.

## DNS

인터넷 사용이 폭발적으로 증가하면서, 컴퓨터 관리자들이 컴퓨터의 hosts 파일을 일일이 고치고 변경하는 일은 대단히 번거로운 일이 되었고, 자연스럽게 한 곳에서 IP 주소와 이름을 연결해주는 주소록 서비스인 DNS(Domain Name Service)가 생겼다. 이제는 어떠한 보내고자 하는 타겟 위치를 hosts에서 찾을 필요 없이 DNS 서버 IP 주소로 전달하면 서비스가 알아서 타겟을 연결해주게 된다. DNS가 보편화되면서 hosts 파일은 기업 네트워크 환경에서 특별한 경우에만 사용하고, 가정용 PC에서는 잊혀지게 되었다.

## hosts 파일과 파밍

위에서 얘기했던 것처럼 DNS의 등장에도 불구하고 hosts 파일은 지금도 사용되고 있다. 컴퓨터는 요청하고자 하는 대상 IP를 찾을 때, 먼저 hosts 파일에서 컴퓨터 이름을 찾고, hosts 파일에서 찾지 못하면 DNS 서버에게 물어본다. 이런 절차를 이용해 개발에서의 편리함을 취하는 것이다. 

파밍은 사용자가 자신의 웹 브라우저에 정확한 웹 사이트의 주소를 입력해도 가짜 웹 페이지에 접속하도록 함으로써 사용자의 개인 정보를 탈취하는 사기 행위를 말한다. 이때 자주 사용되는 해킹이 hosts 파일을 변조함으써 실제 웹 페이지의 IP 주소가 아닌 가짜 페이지의 IP 주소로 연결하도록 하는 것이다.

## Reference

- [AnLab - hosts 파일 역사와 파밍](https://help.ahnlab.com/astx/1.0/ko_kr/start.htm#view_hosts_file.htm)