---
title: "[JDBC] JDBC 기본 개념 및 설정 방법, 연결 예제"
excerpt: "basic jdbc examples and history"
date: 2021-10-19
layout: single
classes: wide
category:
    - jdbc
tag:
    - java
    - jdbc
    - jdbc install
    - jdbc connection
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

# JDBC란 무엇인가?

JDBC는 Java 개발자라면, 특정 프로그램을 만들겠다고 하면 언제나 한 번쯤 들어보는 단어다. 애석하게도 나는 일을 시작하면서 그 개념을 처음 익혀가는 중.

우리는 데이터베이스를 이용하면서 아래 작업을 수행한다.

- 연결/인증
- 문장실행
- 결과패치

그 중 문장실행은 SQL을 말한다. **너 이거 해!** 라고 명령하는 것.

> **Oracle**, **MS SQL**과 같은 DBMS에 접속해 정보를 저장하고, 불러오기 위해서 우리는 SQL을 사용한다.

그런데 문제가 있다. 똑같은 기능을 하더라도 Oracle에 먹히는 SQL문과 MS SQL에 먹히는 SQL은 다르다. 아주 다르다. 

SQL이 같아도 서로 다른 회사의 API 이름은 다를 수밖에 없다.

**DBC(Database Connectivity)**. 즉, DBMS와 연결 또는 접속하기 위한 방법이 모두 다르다는 뜻이다. 기간이 만료되어 다른 DBMS를 사용하게 된다면 이는 분명 문제가 된다. 

그래서 등장한 것이 JDBC이다.

DBMS를 구동하기 위한 코드는 각각 따로 개발되어있다. 이렇게 DBMS를 제어하는 것을 **드라이버**라고 한다. 

Java는 각각의 드라이버를 제어하는 JDBC를 지원한다. 그래서! JDBC로 아래 기능을 구현해놓으면?

1. 드라이버 로드하기
2. 연결 생성하기
3. 문장 실행하기
4. 결과집합 사용하기

예를들어, Oracle에서 MS SQL로 DBMS를 바꿔도 이전처럼 모든 코드를 수정할 필요가 없다. 개발자가 드라이버를 직접 제어할 필요없이, 각 DBMS가 제공하는 드라이버만 로드해주면 되는 것!

## DBMS와 JDBC Driver 준비하기

DBMS는 Oracle을 설치하려고 한다. 같이 공부하는 많은 개발자들도 회사에서 Oracle을 쓴다고 한다.

### Oracle Database 설치

Oracle 21c Express를 다운 받아서 사용하려고 한다. [이곳](https://www.oracle.com/database/technologies/xe-downloads.html)에서 다운받을 수 있다.

오라클 데이터베이스는 세 가지 종류가 있다고 한다.

- Oracle Enterprise Edition (큰 기업용)
- Oracle Standard Edition (작은 기업용)
- Oracle Express Edition (무료, 학생용)

**Express Edition 18c**는 다음과 같은 제약 사항이 있었다.

- 2 CPU threads
- 2 GB of RAM
- 12 GB of user data

다운받아서 압축풀고 **관리자 권한으로 설치**하자.

설치가 끝나면 `1) 어떤 포트로 접속할 수 있는지`, `2) 웹 기반 관리 어플리케이션 접속 주소` 가 나타난다. 잘 정리해두자.

### Oracle JDBC 다운로드

[여기](https://www.oracle.com/database/technologies/appdev/jdbc-ucp-21-3-downloads.html)에서 자기 버전에 맞는 JDBC를 설치할 수 있다.

다운받은 jar 파일을 본인이 원하는 경로에 둔다. 나는 eclipse 폴더에 따로 jdbc 폴더를 만들어 넣었다.

그 다음 Java 프로젝트를 생성하기 위해 먼저 **Perspective**를 Java로 바꿔준다.

<br/>
<img src="/_img/2021-10-21/perspective.jpg" style="margin: auto auto; display: block;"/>
<br/>

프로젝트를 생성해준 뒤, 이 프로젝트에 방금 전 다운받은 JDBC의 Build Path를 연결한다.

<br/>
<img src="/_img/2021-10-21/jdbc_build_path.jpg" style="margin: auto auto; display: block;"/>
<br/>

들어가서 External Library 연결하면 끝!

## SQL Developer 설치하기

[여기](https://www.oracle.com/tools/downloads/sqldev-downloads.html)에서 다운받을 수 있다. 한 400MB 정도 된다. JDK 포함 안 된 것으로 받아도 된다.

그냥 접속해서 사용하는 것보다 **PDB (Plugable Database)** 을 사용한다. 가상화된 데이터베이스로 자원을 공유하면서 독립된 환경을 만들 수 있다.

<br/>
<img src="/_img/2021-10-21/pdb.jpg" style="margin: auto auto; display: block;"/>
<br/>

이걸 보면 **PDB$SEED** 라는 원본이 있고, 그 아래 **XEPDB1** 이라는 가상화된 DB가 있다. 접속할 때, 서비스 이름으로 따로 접속하면 연결 가능하다.

<br/>
<img src="/_img/2021-10-21/local_access.jpg" style="margin: auto auto; display: block;"/>
<br/>

이렇게 해서 **local access** 접근만 허용하는 것을 풀어줄 수 있다.

## 학습용 데이터베이스 생성

- **SCHEMA:** 파일, 오브젝트 등으로 구성된 데이터베이스 자체를 스키마라고 함

이제 아래 작업들을 해주면 된다.

- TABLESPACE 만들기
- LOGSPACE 만들기
- 새로운 계정 만들기

이렇게 해서 새로운 Oracle 접속을 생성한다.

나머지는 DB에 SQL 작업들을 몇 가지 걸어줘야 하는데, DB 챕터를 따로 만들어야겠다.

## JDBC를 이용하는 Java Code

JDBC를 이용하는 자바 코드 기본 과정은 아래 네 단계로 요약 가능하다. 아래 기본 코드를 통해 앞에서 생성한 데이터베이스를 조작할 수 있는 연결을 만들 수 있다.

1. **Class.forName("oracle.jdbc.driver.OracleDriver");**
먼저 드라이버를 로드하는 과정. `new` 생성자를 사용하지 않고 메모리 상에 올린다.

2. **Connection con = DriverManager.getConnection(...);**
DBMS에 연결할 수 있는 연결 객체를 얻는다. JDBC 객체 집합 - 드라이버 - DBMS 연결 생성.

3. **Statement st = con.createStatement();**
실행도구를 생성한다. Statement를 실행도구라 한다.

4. **ResultSet rs = st.executeQuery(sql);**
SQL문을 실행하고, 결과를 **받아 올 수 있는** 객체로 만든다.

이 과정은 순서를 꼭 지켜야 한다. 앞 단계가 완료되지 않으면 순차적 진행이 불가능하다. 연결도 되지 않았는데 실행도구를 생성할 수 없다.

## JDBC ResultSet

여기서 중요한 점! ResultSet은 결과가 아니라 결과를 저장할 수 있는 객체라는 것! 처음 받아온 객체는 커서가 **BOF (Before of File)** 를 가리키고 있는 상태다.

**rs.next()** 를 호출하면 **커서(Cursor)** 가 한 칸씩 다음 행을 참조하며 데이터를 가져온다. 그러다가 **EOF (End of File)** 을 만나면 끝이 난다.

## References
- [newlec: 자바 JDBC 프로그래밍](https://www.youtube.com/watch?v=c0s7g7iVtwc&list=PLq8wAnVUcTFWxwoc41CqmwnO-ZyRDL0og)
- [newlec](https://www.newlecture.com/index)