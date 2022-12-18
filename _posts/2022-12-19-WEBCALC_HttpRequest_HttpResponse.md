---
title: "[WEB Calculator] HttpRequest, HttpResponse 구현"
excerpt: "HttpRequest and HttpResponse implementation"
date: 2022-12-19
category:
    - web calculator
tag:
    - http
    - side project
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
classes: wide
comments: true
---

# HttpRequest, HttpResponse 직접 구현

HttpRequest와 HttpResponse를 직접 구현하고, 서버에서 활용하는 과정을 직접 구현해보며 실제 WAS가 무엇을 어떻게 수행해주는지 알 수 있다. 먼저 결과부터 살펴보면 Custom Web Application Server는 아래와 같이 구현 할 수 있다.

<img src="/_img/2022-12-19/code explanation.png">

실제로 서버를 구동한 다음 `GET http://localhost:8080/calculate?operand1=11&operator=*&operand2=55` 테스트를 수행해 보았다. 결과는 아래와 같이 나타난다. 응답이 정상적으로 전달되었음을 알 수 있다.

<img src="/_img/2022-12-19/request test.png">

## HttpRequest

HttpRequest는 RequestLine, Header, Blank line, Body로 구성된다. 각각을 직접 구현해보면 어떤 동작들이 필요한지 알게 되며, 구현 후 Java에서 제공되는 HttpRequest 클래스와 비교해보며 자신의 설계를 고쳐나가는 것도 큰 도움이 될 것이다. 그러나 지금은 RequestLine 처리만 다뤄보도록 한다.

RequestLine은 GET 요청을 보냈을 때, 서버에서 설정했던 BufferedReader가 가장 먼저 읽어내는 첫 번째 줄이다. 가령 위 테스트 URL로부터 전달되는 RequestLine은 `GET /calculate?operand1=11&operator=*&operand2=55` 이다. 이 RequestLine은 다시 아래와 같이 나눌 수 있다. 

- **Method:** GET
- **URL Path:** /calculate
- **Query String:** operand1=11&operator=*&operand2=55
    - operand1=11
    - operator=*
    - operand2=55

즉, HttpRequest에 RequestLine이 전달되면 자동적으로 그 아래에 있는 정보를 파싱할 수 있어야 한다. 그렇기 때문에 RequestLine을 단순 String이 아닌 객체로 선언하고, 아래와 같이 작성한다.

<img src="/_img/2022-12-19/request line class.png">

여기서 Query String은 Key, Value 한 쌍으로 전달되는 문자열이다. 그런데 이 쌍은 여러 개 전달될 수 있으며, Key에 매칭되는 Value를 뽑아내는 작업 수행을 위해 QueryStrings라는 일급 컬렉션으로 묶어 주었다. 그렇게 하여 객체 초기화 시 전체 QueryString 문자열을 전달하면 자동으로 QueryString 단위로 생성하도록 권한을 위임하였다.

<img src="/_img/2022-12-19/query strings.png">

## HttpResponse

응답은 응답상태에 따라 다양한 메시지가 존재하지만, 여기에서는 200 정상응답만을 구현했다. 이로써 알 수 있는 사실은 응답을 받는 Client 측에서도 결국은 문자열을 Parsing해 사용한다는 것이다. 그리고 이런 역할은 크롬이나 엣지와 같은 Browser가 수행해주고 있다.

<img src="/_img/2022-12-19/http response.png">

## 결론

클라이언트와 서버는 결국 문자열을 전송하고, 이를 파싱하여 요청과 응답을 주고 받는다. 이 때, 문자열을 파싱하기 위한 규칙이 있는데, 이를 프로토콜이라는 규약이라 말 할 수 있다. 여기서는 단순히 문자열 안에서 어떤 기호로 나누는지만을 설명했지만 실제로는 어떤 순서로 주고 받는지 등도 프로토콜이 다루는 영역의 한 가지가 된다. 

이러한 규칙이 정해져 있기 때문에 우리는 망설임 없이 문자열을 Parsing해 요청을 식별하고, 적절한 응답을 전달할 수 있다. 통신 공학을 전공한 엔지니어로서, 전기적 신호에서 위와 같은 데이터 흐름까지를 천천히 생각해보며 복기 할 수 있어야 겠다는 생각이 든다.