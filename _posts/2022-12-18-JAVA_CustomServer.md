---
title: "[Java] HTTP와 Socket"
excerpt: "Custom Web Server and HTTP check, Socket"
date: 2022-12-18
category:
    - java
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

# Custom Server 구현

보통 웹 어플리케이션을 다룰 때 WAS(와스)라는 말을 많이 사용한다. 이런 WAS가 어떤 역할을 하는지 확인해보기 위해 직접 간단한 Custom Web Server를 만들어보자. 먼저 아래와 간단히 서버 코드를 작성한다. 직접 작성한 서버는 요청을 수신하기 위한 포트를 설정하고, 사용자 요청을 콘솔 상에 System.out.println()을 통해 표시해준다.

```java
public class CustomWebApplicationServer {
    private final int port;

    private static final Logger logger = LoggerFactory.getLogger(CustomWebApplicationServer.class);

    public CustomWebApplicationServer(int port) {
        this.port = port;
    }

    public void start() throws IOException {
        try (ServerSocket serverSocket = new ServerSocket(port)) {
            logger.info("[CustomWebApplicationServer] started {} port", port);

            Socket clientSocket;
            logger.info("[CustomWebApplicationServer] waiting for client");

            while ((clientSocket = serverSocket.accept()) != null) {
                logger.info("[CustomWebApplicationServer] client connected!");

                // [요구사항1] 사용자 요청을 메인 Thread가 처리하도록 한다
                try(InputStream in = clientSocket.getInputStream();
                    OutputStream out = clientSocket.getOutputStream()) {
                    BufferedReader br = new BufferedReader(new InputStreamReader(in, StandardCharsets.UTF_8));
                    DataOutputStream dos = new DataOutputStream(out);

                    String line;
                    while((line = br.readLine()) != "") {
                        System.out.println(line);
                    }
                }
            }
        }
    }
}
```

## Socket (소켓)

컴퓨터 공학 전공자나 네트워크 프로그래밍 강의를 수강해 본 사람에게는 Socket이란 단어가 익숙하게 들릴 것이다. Socket 통신 기반 채팅 프로그램 만들기 프로젝트는 공대생이라면 반드시 한 번은 만들어보고 졸업하지 않을까란 생각까지 든다. 위 코드의 Socket은 그 프로젝트에 사용되던 Socket과 크게 다르지 않다.

그러나 소켓을 사용해 네트워크 통신 기능을 구현하는 과정인 소켓 프로그래밍(Socket Programming)은 사실 아주 어려운 과정이다. 운영체제나 프로그래밍 언어에서 종속적으로 제공하는 소켓 API 사용법을 숙지하는 것은 기본이고, 이를 바탕으로 소켓 자체를 구현하고, 해당 소켓을 기반으로 한 데이터 통신 프로세스를 설계하는 모든 일을 수행해야 하기 때문이다. 

거기에 더해 네트워크 단절, 트래픽 증가에 따른 데이터 전송 지연, 시스템 리소스 부족에 따른 에러 등 네트워크 환경에서 발생할 수 있는 다양한 상황에 대한 예외처리도 필요하기 때문에 소켓 프로그래밍은 굉장히 어려운 영역이라 할 수 있다. 다행히 소켓 프로그래밍은 역사가 긴 편이고, 엔지니어들의 문서화 습관 덕분에 대부분의 프로그래밍 언어와 개발 플랫폼에서 소켓과 관련된 API가 제공되고 있다.

실제로 대부분 소켓 API 실행 흐름은 정형화되어 제공되고 있다. 소켓 API 실행 흐름은 아래 그림과 같이 간단하게 나타낼 수 있다. 

<img src="/_img/2022-12-18/socket execution process.png">

### 클라이언트 소켓 프로그래밍

클라이언트에서는 socket() 메소드를 이용해 소켓을 생성하고 connect()에 IP, PORT 정보를 기입해 연결 요청을 전송한다. 이 때, 해당 요청은 Block되어 응답이 전달되기 전까지 대기 상태를 가진다. connect()가 성공하면 send()와 recv()을 통해 데이터 송수신을 진행하고, 작업이 완료되면 close()를 통해 생성했던 소켓을 닫아준다. Linux에서는 이러한 소켓 정보를 파일로 관리한다는 사실은 덤으로 알아두자.

### 서버 소켓 프로그래밍

서버에서는 조금 더 복잡한 과정을 거친다. socket()을 통해 소켓을 생성하는 것은 맞지만 이후 과정이 클라이언트와는 다르다. 먼저 bind() 작업은 Client의 요청을 받는 PORT를 내부적으로 어떤 소켓과 연결할 것인지를 결정하는 과정이다. 만약 이전에 8080포트를 다른 소켓이 점유하고 있다면, 방금 만든 소켓이 bind()를 시도하는 경우 에러를 리턴한다.

그 다음 listen()은 해당 소켓으로 요청이 전달되는지 확인하는 것이다. 이제부터 클라이언트 소켓에서 connect() 요청을 보내면 요청을 잘 받았다는 Success나 에러가 발생했다는 Fail을 리턴 할 수 있다. 그치만 클라이언트 연결 요청에 대한 정보를 **수신**은 했으나 **연결**은 진행하지 않은 상태로, 연결 요청에 대한 정보는 시스템 내부 큐(Queue)에 쌓여만 있다. 연결을 완료하기 위해 서버는 accept()를 사용해야 한다.

이렇게 accept()를 수행하면 새로운 소켓에 연결 정보를 주입하고 생성한다. 클라이언트는 서버 소켓이 아닌 새롭게 생성된 소켓과 데이터를 주고 받게 되는 것이다. 서버 소켓은 계속해서 listen() 상태로 새로운 요청이 들어오는 것을 감지하는 역할만을 수행한다.

### HTTP vs Socket 통신

Socket은 연결 정보가 유지된다. 대표적인 Stateful 방식의 통신이다. 앞에서 본 것처럼 요청자의 정보를 저장한 소켓이 1:1로 다른 소켓을 담당한다. 물론 추후에 한 개 소켓에 여러 요청자 정보가 대응될 수 있지만 기본적으로는 연결 정보가 유지된다는 것이 중요하다.

그러나 HTTP 통신은 Stateless하다. 요청을 보낼 때마다 자신의 인가 정보를 전달해줘야 하고, 서버는 응답을 보내고는 전달이 정상적으로 전달되었는지 확인하지 않는다. 이 두 통신 방식 외에도 다양한 통신 프로토콜이 존재하니 되도록 다양한 방식을 익히고, 시스템 구축에 고려해봐야 할 것이다. 그것이 곧 경험을 살리는 아키텍처로 나아가는 기반이 된다.

## 서버 동작 확인

소켓의 동작 과정을 생각하며 작성한 서버가 어떻게 동작하는지 확인해보자. Main.java 파일에 CustomWebApplicationServer를 생성하고 실행하면 아래 그림처럼 서버가 정상적으로 동작되는 것을 확인 할 수 있다.

<img src="/_img/2022-12-18/server run.png">

여기서 IntelliJ에서 제공하는 http request 파일을 만들어 `GET http://localhost:8080` 으로 요청을 보내고 다시 콘솔 창을 확인하면 아래와 같이 HTTP 요청을 확인 할 수 있다.

<img src="/_img/2022-12-18/server received request.png">

요청 정보가 모두 보인다. GET Method가 사용되었고, Host 정보는 localhost, 포트는 8080번, User-Agent 정보와 인코딩까지 모두 확인 할 수 있다. 예를 들어 Tomcat이라는 WAS는 이렇게 전달 받은 요청을 Parsing하여 처리해주는 역할을 수행한다. CustomWebApplicationServer를 만드는 것은 이런 요청을 잘 Parsing하여 적절한 응답을 수행하는 객체를 만드는 것이다.

웹 계산기에서 적절한 응답이란 무엇일까? `GET http://localhost:8080/calculate?operand1=11&operator=*&operand2=55` 요청이 들어온다고 가정해보자. 그러면 아래와 같이 사용자 요청 메시지가 첫 번째 줄에 출력된다. 이제 알 수 있다. 적절한 응답이란 해당 메시지를 잘 Parsing하여 로직에 맞게 계산 후 다시 응답하는 것이다.

<img src="/_img/2022-12-18/server received parameter.png">

## References

- [소켓 통신의 이해](https://recipes4dev.tistory.com/153)
