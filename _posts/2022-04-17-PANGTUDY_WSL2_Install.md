---
title: "[Pangtudy-web] WSL2 설치"
excerpt: "wsl2 install"
date: 2022-04-17
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

# WSL2 설치

WSL 은 Windows Subsystem for Linux 를 말한다. 리눅스용 윈도우 하위 시스템은 윈도우10에서 네이티브로 리눅스 실행 파일을 실행하기 위한 호환성 계층이다.

설치하는 방법은 [공식 문서](https://docs.microsoft.com/ko-kr/windows/wsl/install)에 잘 설명되어있다. 이를 짧게 요약하면 아래와 같다. 필수 버전은 무조건 충족했다고 가정한다.

1. Windows PowerShell 관리자 권한으로 실행
2. DISM(배포 이미지 서비스 및 관리) 명령어로 Microsoft-Windows-Subsystem-Linux 기능 활성화
    - `dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart`
3. DISM(배포 이미지 서비스 및 관리) 명령어로 VirtualMachinePlatform 기능 활성화
    - `dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart`
4. 재부팅
5. 자기 CPU에 맞는 WSL2 Linux 커널 업데이트
    - [공식 문서](https://docs.microsoft.com/ko-kr/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package) 의 `x64 머신용 최신 WSL2 Linux 커널 업데이트 패키지` 를 다운받아 실행
6. Windows PowerShell 에서 `wsl --set-default-version 2` 명령어로 WSL 기본 버전을 2로 변경
7. `wsl --list --online` 을 통해 설치할 수 있는 배포 목록 확인
8. `wsl --install -d <배포 이름>` 을 통해 설치
    - ex) `wsl --install -d Ubuntu-20.04`

이게 정상적으로 마무리되면 사용자 이름과 패스워드를 입력하라는 메시지가 나온다. 그런데... 설치 후 과정에서 `Error: 0x80370102` 가 떴다.

StackOverflow 에서 2가지 해결책을 찾았다. 한 가지는 Hyper-V 옵션을 키는 것. `Windows 기능 켜기/끄기` 에서 Hyper-V 옵션을 킬 수 있는데, Windows Home 버전은 불가하다.

두 번째로 해결했다. BIOS 에서 CPU 가상화 기술을 활성화하면 된다. 어쩔 수 없이 성능하락이 발생한다고 하는데, 아직까지 크게 느껴지는 면은 없다.

컴퓨터를 키며 DEL 키 연속으로 눌러주면 BIOS 접속 가능하다.

- Intel CPU
    - Advanced CPU Configuration > Intel Virtualization Techonlogy > Enable
- AMD CPU
    - OC > Advanced CPU Configuration > SVM Mode > Enable

이후 Windows Terminal 을 설치함면 `cmd > wt` 치고 들어간 뒤 간단하게 머신을 띄울 수 있다.