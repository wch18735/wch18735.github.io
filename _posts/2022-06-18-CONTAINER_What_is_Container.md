---
title: "[Container] Container 란?"
excerpt: "What is container"
date: 2022-06-07
layout: single
classes: wide
category:
    - container
tag:
    - container
    - docker
author: 1FeS
comments: true
header:
    teaser: "/_img/thumb-nail/docker-container.png"
---

# Container 등장 배경

소프트웨어 기술 발전은 역사를 바라보면 그 이해가 쉽다고 한다. 과거에는 베어메탈(Bare metal) 위에 하나의 OS를 설치하고 여러 WAS를 올려 운영했다. 장비를 구매하고 배송이 완료되면 운영체제(OS)를 설치하고, DB를 설치하거나 WAS 등을 구축한 다음에야 용도에 맞게 활용할 수 있었다고 한다. 방화벽과 보안 설정은 덤. 

아직도 장비를 투자하고, 각 분야 엔지니어들의 작업이 끝난 후에야 사내망에 연결되어 활용하는 방식으로 일하는 회사들이 있다고는 한다. 이 단계들을 모두 거치기까지는 상당한 시간과 전문적인 서버 지식을 갖춘 엔지니어의 손길을 거쳐야 했다고 들었다.

이 같은 방식은 점차 서버 성능이 발전함에 따라 서비스가 소모하는 하드웨어 자원의 활용률 감소로 이어졌다. 같은 어플리케이션이라도 이전에는 서버 자원의 80%를 사용했다면 20%도 채 사용하지 못하는 경우가 빈번해졌다. 즉, 그 무심무시한 자원 저활용이 발생하기 시작했다고 한다. 유휴자원 발생은 곧 불필요한 지출을 뜻했기에 이를 해결하기 위해 많은 엔지니어들이 머리를 맞댔다고 전해진다.

그리고 가상화(Virtualization) 기술이 등장하며 자원 활용률 문제 해소가 가능해졌다. 하나의 시스템을 논리적으로 여러 개의 시스템처럼 활용할 수 있게 됐다. 단일 서버에 가상화된 여러 OS를 만들고, 그 위에 여러 어플리케이션을 올려 서비스하는 것이 가능해져 사용률을 높일 수 있엇다. 

그러나 이러한 가상화가 하드웨어 기반의 자원 격리를 통해 자원 활용률을 높이는데 기여한 것은 맞지만 여전히 그 과정은 변하지 않았다. 가상화 환경에서 애플리케이션 서비스를 제공하려면 서버에 하이퍼바이저(Hypervisor)와 게스트(Guest) OS를 설치하고, WAS를 올려 애플리케이션을 구동하는 과정은 변하지 않았다. 또한 이기종 가상머신(VM) 간 호환성 문제와 어플리케이션 서비스 확장성(Scalability) 측면에서 유연한 스케일 업/다운이 어려웠다는 단점이 존재했다.

이후 등장한 것이 컨테이너 기술이다. 컨테이너는 가상화와 마찬가지로 자원 격리를 통해 효율적인 자원 활용을 가능하게 하며, 가상화 이미지 대비 한층 경량화된 용량으로 인해 빠르게 서비스를 올리고 내리는 것도 가능해졌다. 이로 인해 민첩하고 유연함이 강조되는 클라우드 환경에서 컨테이너 기술이 빠르게 성장하게 된 것이다.

<img src="/_img/2022-06-18/container_history.jpg">

# Why Linux ?

그럼 컨테이너는 왜 리눅스에서 운영할까? 그 이유는 리눅스 커널 기능을 사용하기 위해서이다. 컨테이너를 만들기 위해서는 아래 세 가지 커널 기능을 사용한다.

- chroot: 독립된 공간을 만들어주는 기능
- namespace: 독립된 공간 안에 6가지 Isolate (분리 또는 고립) 를 지원하는 기능
- cgroup: 필요한 만큼 HW를 지원해주는 기능

# Container vs General Application

아래와 같은 간단한 app.js 가 있다고 가정하자.

```js
// app.js
const http = require('http');
const os = require('os');
cnosole.log("Test server starting...");
var handler = function(req, res) {
    res.writeHead(200);
    res.end("Container Hostname: " + os.hostname() + "\n");
}
var www = http.createServer(handler);
www.listen(8080);
```

보통은 app.js 가 실행될 수 있도록 VM에 OS를 올리고, 구동에 필요한 라이브러리들을 추가해줘야 한다. 그런데 컨테이너라면? 앞에서 봤던 그림처럼 엔진 위에 가벼운 컨테이너들을 올려 사용할 수 있게 되었다. 여기서 많은 엔진들이 존재하는데, 그 중 하나가 Docker 다.

그치만 컨테이너 기반 배포의 가장 큰 특징은 `Loosly coupled system`, `무중단 배포`, `간단한 Scale Out` 이라고 생각한다. 개발자가 만든 그대로 어디서든 돌아가고, 개발과 동시에 고객에게 제공될 수 있으며, 유동적인 트래픽에도 유연하게 동작할 수 있는 형태를 모두 갖춘 새로운 서비스 제공 문화로 자리잡을 수 있는 기반 기술이 바로 컨테이너다.

컨테이너는 이제, 현대 웹 개발자들에겐 필수적으로 경험해봐야하는 요소가 되었다고 생각한다.

# Reference 

- 데이터넷 [http://www.datanet.co.kr](http://www.datanet.co.kr)
- 따배도: 따라 배우는 도커 [1.컨테이너와 도커의 이해](https://www.youtube.com/watch?v=3HId-tpYaZs&list=PLApuRlvrZKogb78kKq1wRvrjg1VMwYrvi&index=2)