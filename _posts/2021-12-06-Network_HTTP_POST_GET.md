---
title: "[Network] POST 와 GET 명령어 차이점"
excerpt: "difference between post and get"
date: 2021-12-06
layout: single
classes: wide
category:
    - network
tag:
    - http
    - post
    - get
toc: true
toc_label: "Contents"
author: 1FeS
comments: true
---

간단하게 정리해보는 `HTTP` 개념과 `GET`과 `POST` 요청의 차이! IT 서비스 개발 면접 질문으로 무조건 나오는 질문. 안 나오면 이 정도는 무조건 안다고 가정하고 넘어가는 경우다. 그럼 안심해도 될까? 아니요. 왜냐면 더 어려운 질문이 나올테니까.

# HTTP

- *HTTP (Hyper Text Transfer Protocol)* 은 *W3 (Wolrd Wide Web)* 상에서 정보를 주고받을 수 있는 프로토콜이다. 
- *HTML 문서* 를 주고받는 데에 쓰이며, *TCP*, *UDP* 등의 *전송 프로토콜* 을 사용한다.
- HTTP는 `클라이언트`와 `서버` 사이에 이루어지는 `요청/응답 (Request/Response) 프로토콜`이다.
  - 예시) 클라이언트인 `웹 브라우저`가 HTTP를 통해 웹페이지나 그림 `정보를 요청`하면 `서버`는 `응답`으로 필요한 정보를 해당 사용자에게 전달한다.

## GET 요청

- 특정 정보를 조회하기 위해 사용되는 메소드
- URL에 변수(데이터)를 포함시켜 요청하는데 이를 `쿼리스트링(Query-string)` 이라 한다
- 데이터를 Header(헤더)에 포함하여 전송한다
- 캐싱할 수 있다

> 캐싱이란?
주어진 리소스 복사본을 젖아하고 있다가 요청 시에 이를 제공하는 기술. 원래 저장소로부터 리소스를 다시 다운받지 않도록 하여 웹 사이트, 어플리케이션 성능을 현저하게 향상시킬 수 있는 기술.

웹 캐시는 크게 *Priavte cache* 와 *Shared cache* 로 나뉜다. `사설 캐시`는 한 명만이 사용할 수 있는 캐시로 뒤로가기, 저장, 소스로 보기 등의 작업을 지원한다. `공유 캐시`는 한 명 이상의 사용자에 의해 응답을 저장하는 캐시로 조회가 많은 리소스들은 재사용을 제공하여 트래픽과 레이턴시를 줄이는 방향으로 사용된다.

더 자세한 내용은 [여기](https://developer.mozilla.org/ko/docs/Web/HTTP/Caching)에서 확인해보자.

## POST 요청

- 캐시되지 않음
- 브라우저 기록에 남지 않음
- 요청하는 데이터 길이에 제한이 없음
- POST 요청은 *Non-Idempotent* 함

> Idempotent
멱등성. 어떤 작업을 여러 번 수행했을 때, 항상 같은 값이 나타나는 것을 보장하는 성질이다. POST 메소드는 리소스를 생성할 수 있기 때문에 서버 상태를 변화시킬 가능성이 있다.

즉, HTTP 메소드는 다음과 같은 역할에 활용될 수 있다.

- **GET :** 조회
- **POST :** 생성
- **PUT :** 리소스 전체 수정
- **PATCH :** 리소스 일부 수정
- **DELETE :** 리소스 삭제