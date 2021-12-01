---
title: "[Oracle] 테이블 생성 및 데이터 조작 예제 (CREATE, INSERT, SELECT, UPDATE, DELETE)"
excerpt: "Query oriented structure and DML practice"
date: 2021-11-16
category:
    - oracle
tag:
    - database
    - oracle
    - sql
    - dml
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# 테이블 구조

구성할 테이블 구조는 아래 그림과 같다.

<br/>
<img src="/_img/2021-11-16/table_structure.jpg" style="margin: atuo auto; display: block;">
<br/>

이를 위한 테이블 생성 쿼리는 아래와 같다.

```sql
CREATE TABLE NOTICE
(
     ID		    NUMBER,
     TITLE 		NVARCHAR2(100),
     WRITER_ID	NVARCHAR2(50),
     CONTENT	CLOB,
     REGDATE  	TIMESTAMP,
     HIT		NUMBER,
     FILES		NVARCHAR2(1000)
);

CREATE TABLE "COMMENT"
(
     ID		    NUMBER,
     CONTENT	NVARCHAR2(2000),
     REGDATE  	TIMESTAMP,
     WRITER_ID	NVARCHAR2(50),
     NOTICE_ID	NUMBER
);

CREATE TABLE ROLE
(
     ID		        VARCHAR2(50),
     DISCRIPTION 	NVARCHAR2(500)
);

CREATE TABLE MEMBER_ROLE
(
     MEMBER_ID	NVARCHAR2(50),
     ROLE_ID		VARCHAR2(50)
);
```

여기서 `"COMMENT"` 로 나타낸 이유는 COMMENT 가 **예약어**이기 때문이다. 예약어를 테이블 명으로 사용하기 위해서 문자열임을 나타내기 위해 큰따옴표를 사용한다.

# 데이터 조작

데이터를 조작하는 언어를 DML 이라고 한다.

데이터베이스를 사용할 때 가장 많이 활용되는 부분으로 확실하게 익혀둬야 나중에 일일이 찾을 필요 없다.

## 데이터 삽입 - INSERT

INSERT 명령 규칙은 `INSERT INTO <TABLE NAME> VALUES <값 목록>` 과 같다.

이때, 원하는 필드만 원하는 순서대로 입력하고 싶을 때는 `INSERT INTO <TABLE NAME>(COLUMN_A, COLUMN_B) VALUES <값 목록>(VALUE_A, VALUE_B)` 와 같은 형태를 사용한다.

이때, 지정되지 않은 값은 DEFAULT 가 정해지지 않았다면 NULL 값이 입력된다.

이렇게 한 번 데이터를 삽입한 다음 `SELECT * FROM MEMBER` 를 통해 배운 내용을 확인해보자.

```sql
INSERT INTO MEMBER(ID, PWD) VALUES('wch18735', '111');
INSERT INTO MEMBER(ID, PWD) VALUES('dragon', '111');
```

결과는 이렇게 보인다.

<br/>
<img src="/_img/2021-11-16/member_select_all.jpg" style="margin: atuo auto; display: block;">
<br/>

## 데이터 조회 - SELECT

데이터 조회는 `1) 특정 컬럼을 지정` 하고 `2) 컬럼 명을 바꾸어` 조회할 수 있다.

```sql
SELECT ID "USER ID", NAME, PWD "password" FROM MEMBER;
```

이렇게 `AS` 를 사용하지 않아도 별칭을 지정할 수 있으며, 별칭에 공백을 포함하는 방법은 큰 따옴표로 감싸는 것이다.

그럼 아래와 같은 형태로 나타난다.

<br/>
<img src="/_img/2021-11-16/member_select_col.jpg" style="margin: atuo auto; display: block;">
<br/>

## 데이터 변경 - UPDATE

기존 값들을 바꾸기 위해서 UPDATE 명령어를 사용할 수 있다.

아이디가 `wch18735` 인 사람의 비밀번호를 `222`로 바꿔보자.

```sql
UPDATE MEMBER SET PWD='222' WHERE ID='wch18735';
```

즉, 명령어 규약은 다음과 같이 `UPDATE <TABLE> SET <COLOUMN>=VALUE WHERE CONDITION` 형태로 나타낼 수 있다.

여러 COLUMN 을 여러 개 변경하는 방법은 아래와 같이 콤마를 사용하면 된다.

```sql
UPDATE MEMBER SET NAME='BLUE EYE', PWD='111' WHERE ID='dragon';
```

## 데이터 삭제 - DELETE

특정 테이블의 행을 삭제하고 싶다면 다음과 같다.

주의할 점으로 `WHERE` 절을 삽입하여 조건을 확실히 정해줘야 본인이 원하는 데이터만 삭제할 수 있다.

```sql
DELETE MEMBER WHERE ID='dragon';
```