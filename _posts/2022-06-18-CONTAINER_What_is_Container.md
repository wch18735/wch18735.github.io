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

그럼 컨테이너는 왜 리눅스에서 운영할까? 그 이유는 리눅스 커널 기능을 사용하기 위해서이다. 이렇게 설명하면 조금 어려울 수 있다. 왜 리눅스 커널을 사용해야 할까? 그것은 바로 `독립된 공간`을 만들기 위함이다.

독립. 영어로하면 Independency. 반대의 개념으로는 종속이 있다. 어디서 많이 들어보지 않았나? 여러 예문이 있겠지만, 나는 가장 최근 아래와 같이 종속성이라는 단어가 사용되는 것을 보았다.

`이 시스템은 OO 회사의 솔루션 Dependency 가 강해서 쉽게 수정 할 수 없어요`

이렇듯 의존성, 종속성이 높은 시스템은 쉽게 수정할 수 없으며 유지 보수가 까다로워진다. 물론 이런 것들이 변명이 되어서는 안되며, 그런 변명들에 빠지는 순간 실력없는 개발자가 될 수 있으니 항상 조심하자. 하여튼 중요한 점은 독립성. Container 를 사용하면 프로세스에 독립된 자원을 할당해 고립시킬 수 있다.

간단하게 컨테이너를 만들기 위해 사용하는 세 가지 커널을 나타냈다.

- chroot: 독립된 공간을 만들어주는 기능
- namespace: 독립된 공간 안에 6가지 Isolate (분리 또는 고립) 를 지원하는 기능
- cgroup: 필요한 만큼 HW를 지원해주는 기능

이를 이용하면 어떤 장점이 있을지는 위에서 이미 작성했지만, 다시 한 번 Red Hat 에서 제공하는 `What's a linux container` 의 설명을 빌려보려고 한다. 

서로 다른 개발자들의 로컬 환경은 다를 수 있다. 그러나 회사는 특정 환경으로 구성된 자체 서버를 갖추고 있다. 개발자들이 서버에 맞춰 다시 개발할 필요도, 반대로 서버를 개발자들 환경에 맞춰 재구축하는 부가적인 작업 없이, 개발자들은 로컬에서 에뮬레이션 한 어플리케이션을 그대로 서버에 올려 실행시킬 수 있도록 하는 것. 그것이 컨테이너 기술이다.

# Container vs General Application

아래와 같은 간단한 app.js 가 있다고 가정하자. 우리의 목적은 app.js 를 동작시키는 것이다.

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

보통은 app.js 가 실행될 수 있도록 VM에 OS를 올리고, 구동에 필요한 라이브러리들을 추가해줘야 한다. 그렇다면 컨테이너는? 

<img src="/_img/2022-06-07/container_architecture.jpg">

위의 그림처럼 여러 image 들을 기반으로 Kernel 위에서 돌아갈 수 있다. 굳이 하드웨어를 에뮬레이션(특정 CPU나 Memory를 흉내내는 것)해 서로 다른 OS를 올리지 않고, Docker 와 같은 엔진 위에 가벼운 컨테이너들을 올려 사용할 수 있게 된 것이다.

첨언하자면, 나는 컨테이너 기반 배포의 가장 큰 특징은 `Loosly coupled system`, `무중단 배포`, `간단한 Scale Out` 이라고 생각한다. 개발자가 만든 그대로 어디서든 돌아가고, 개발과 동시에 고객에게 제공될 수 있으며, 유동적인 트래픽에도 유연하게 동작할 수 있는 형태를 모두 갖춘 새로운 서비스 제공 문화로 자리잡을 수 있는 기반 기술이 바로 컨테이너다.

컨테이너는 이제, 현대 웹 개발자들에겐 필수적으로 경험해봐야하는 요소가 되었다고 생각한다.

## Container? Container Image?

컨테이너란 하나의 프로세스 또는 어플리케이션이 될 수 있다. 혹자는 MSA(Micre-service Architecture) 패러다임 아래에서 기능별로 나뉘어진 단위들만이 Container 로 구성될 수 있다고 생각한다. 그러나 이것은 틀렸다. Node.js 기반의 Web Server, Spring Boot 기반의 Backend Server, MySQL DB 등 큰 단위의 어플리케이션도 컨테이너로 구성될 수 있다.

이런 컨테이너를 공부할 때, 이미지(image)라는 이야기를 많이 듣는다. 이미지는 무엇일까? 여러 강의를 들어보면 다양한 비유를 보았지만 나는 아래 설명이 가장 마음에 든다.

`An image can be thought of an architectural drawing of a house; a container is a house built based on that drawing; several houses can be built from the drawing, optionally with differences, e.g. color, roof type, additions/replacements`

이미지는 집의 설계 도면이며 Container 란 도면을 기준으로 지어진 집이다. 같은 집이라도 색, 지붕의 종류 등은 조금씩 다를 수 있다. 즉, 이미지가 Class 라면 컨테이너는 Instance 가 되는 것이다.

<img src="/_img/2022-06-07/Layer3b.png">

실제로 위 그림과 같이 컨테이너를 instantiate 하기 위해서 Docker 엔진은 이미지를 받아오고 writable layer 를 추가한 뒤, network port 나 container name, ID, 자원 등을 할당한다.

정리하자면, 우리가 컨테이너를 사용한다라고 하는 것은 가장 먼저 `이미지를 만든다` 라는 행위를 필요로 한다. 다음 포스트에서는 어떻게 이미지를 만드는지, 어떻게 이미지를 가져오는지, 어떻게 이미지를 저장하는지 등을 정리해 볼 예정이다.

# Reference 

- 데이터넷 [http://www.datanet.co.kr](http://www.datanet.co.kr)
- 따배도: 따라 배우는 도커 [1.컨테이너와 도커의 이해](https://www.youtube.com/watch?v=3HId-tpYaZs&list=PLApuRlvrZKogb78kKq1wRvrjg1VMwYrvi&index=2)
- RedHat [What's a linux container](https://www.redhat.com/ko/topics/containers/whats-a-linux-container)
- [Neo Kobo - Docker Container](https://neokobo.blogspot.com/2017/03/docker-container.html?m=0)