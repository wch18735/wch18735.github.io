---
title: "[Oracle] Troubleshoot for executing sqldeveloper"
excerpt: "troubleshooting for executing sqldeveloper"
date: 2021-11-05
category:
    - oracle
tag:
    - database
    - oracle
    - sql
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# SQL Developer 실행하기

**sqldeveloper...** 개복치같은 녀석이다. vscode, visual studio 실행하면서 이게 중간에 뻗으면 어떡하지? 응답없음 뜨면 우짜지? 이런 생각한 적 있는가.

물론 visual stdio까지는 가끔 그러기도 함. 근데 얘는 connection 할 때마다 가슴이 두근두근한다.

하루 종일 연결 안 되는 이 놈을 붙잡고 원일을 찾아본 결과, Listener가 실행되고 있지 않아서였다. 그래서 서비스 재실행하려고 온갖 노력을 다했다.

이놈.

<img src="/_img/2021-11-05/listener_service.jpg" style="margin: auto auto; display: block; border: 1px black;"/>
<br/>

결국 Oracle 21c 삭제하고 다시 깔았다.

아마도 언젠가는... 한 번 더 이런 일이 생기겠지. 
그 때를 위해 남겨놓는 작은 기록...

## Oracle 21c 설치

먼저 Setup.exe file이 생기는데 이건 관리자 권한으로 실행하자.

가끔 `이 시스템에는 이미 Oracle Database 21c Express Edition 서비스가 있습니다.` 라는 알림이 오는데, 아래 명령어 치고 다시 해보자.

```
$ sc delete OracleServiceXE
$ shutdown -r -t 0
```

아마 sc는 system call이고, reboot을 0초 뒤에 실행하세요 이런 의미인 것 같다.

## sqldeveloper

`일부 모듈을 설치하지 못 했습니다`라는 오류가 나올 수 있다. 응답없음 상태에서 종료하고 나면 생긴다.

User > Roaming > SQL Developer 에 있는 파일 다 삭제하면 해결된다. 대신 이전에 만들어뒀던 작업 환경들도 다 날아간다.

외부에서 접근 가능하게 설정 바꾸려면

```
$ sqlplus / as sysdba
$ exec dbms_xdb.setlistenerlocalaccess(false);
```

이렇게 **listener**가 **local access만 허용하는 것을 false로** 바꾸면 된다.
아, 이렇게 했는데 막히면 포트가 안 열린 걸 의심해보면 된다.

방화벽 작업은 시스템에서 열 수 있고, **1521번은 기본**으로 하고 나머지는 프로젝트에 따라 열면 되겠다.

### 작업환경 구축

아래 순서로 진행하자.

- PDB 접속
- DBA 창 열어 사용자 지정
  - 테이블 스페이스에 Data-file, Log-file 생성
  - 보안 탭에서 계정 생성하고 만들어 준 테이블과 연결

