---
title: "[Oracle] Database 공부하자"
excerpt: "why do we need to study database?"
date: 2021-11-01
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

# Database를 공부한다면?

개발자로서 필연적으로 접하게 될 Database. 이를 공부한다면 어디가서 <span style="background-color: #CACACA; font-weight: bold;">아니 이것도 몰라?</span> 소리는 안 들을 수 있다.

"우와 이런 것도 알아?" 까지는 절대 아니다. 결론... 필수로 알아야 하는 지식이라는 뜻. 대신 너무 급하게 공부하지 않고, 천천히 알고 있는 것들을 정리하는 것으로 시작해보자.

그럼에도 불구하고 더 공부하고 싶다면 어디로 가야하나.

## DBA Course

`우리는 어떻게 관리자 권한을 얻어 데이터베이스를 관리할 수 있을까?` 라는 의문이 생길 수 있다.

**Admin Accounts(관리자 계졍)** 은 아래 2개가 주어진다.

- **SYS:** An account used to perform database administration taks.
- **SYSTEM:** A default generic database administrator account for Oracle databases.

좀 더 큰 단위의 작업은 **SYS**만이 할 수 있다. SYSDBA 기능은 SYS만 가지고 있는데, 이는 DBA 과정을 더 깊게 공부하면서 알 수 있다고 한다.

또한 Oracle에서 기본적으로 주어지는 Sample Schema들이 주어진다. SQL 연습을 해보고 싶다면 사용해보는 것 추천. 이를 기반으로 제공되는 강의들이 많다고 한다.

`데이터베이스는 어떻게 파일을 관리하고 저장할까?` 라는 의문도 생길 수 있다.

Table space 라는 곳 안에 파일, Table 들이 존재한다. 이렇게 모인 데이터베이스를 보통 **Schema**라고 부른다.

한 개의 테이블은 한 개의 파일에만 존재할까? 답은 `그렇지 않다`이다.

각 테이블들은 파일의 일부에 나눠져 저장될 수 있다. 이들 구조를 설계하고, 접속 방법이나 검색 방법을 최적화 해 성능을 향상 시키는 작업을 **튜닝**이라 한다.

역시 실무에서 웹 어플리케이션 성능을 상승시키는 방법 중 하나로 **SQL 튜닝**, **Database tuning**이 있다. 필연적으로 배우게 될 영역이기 때문에 마음의 준비를 하는 편이 좋다.

더 깊게 공부하고 싶다면 역시 DBA 과정을 찾아보거나, SQLP 자격증 공부를 추천받았다.

<span style="background-color: #BFBFBF; font-weight: bold;">※ SQLP 특징으로는 취득 난이도는 적당하나 실무에 크게 활용될 것이라는 기대는 버리라고 한다.</span>