---
title: "[Oracle] SQL 활용 예제"
excerpt: "CREATE, ALTER, SELECT, INSERT, UPDATE, DELETE"
date: 2022-03-06
category:
    - oracle
tag:
    - database
    - oracle
    - sql
    - ddl
    - dml
    - dcl
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# SQL 구분

SQL은 성격에 따라 다음과 같이 구분할 수 있으며 각각에 사용되는 키워드를 살펴보면 개념을 쉽게 떠올릴 수 있을 것이다.

- **DDL :** CREATE, ALTER, DROP 
- **DML :** INSERT, SELECT, UPDATE, DELETE - CRUD
- **DCL :** GRANT, REVOKE

테이블 자체를 생성, 변경, 누락하는 명령어를 DDL이라 한다. Table을 조작하는 DML과 조금 차이가 있다. 즉, 변경 대상이 다르다고 할 수 있다. DCL 은 권한과 관련된 쿼리라고 생각하자.

아래는 자주 사용되는 키워드별 사용법을 정리했다.

## CREATE

`CREATE` 문은 `CREATE TABLE [TABLE명] (COLUMN NAME TYPE, ...)` 형식으로 수행할 수 있다. 각각의 컬럼은 콤마(,)를 이용해 구분한다.

```sql
CREATE TABLE MEMBER (
    ID  VARCHAR2(50),
    PWD VARCHAR2(50),
    NAME VARCHAR2(50),
    GENDER VARCHAR2(50),
    AGE NUMBER,
    BIRTHDAY VARCHAR2(50),
    PHONE VARCHAR2(50),
    REGDATE DATE
);
```

SQL을 배우게 되면 가장 먼저 실행해보는 문장이 `CREATE` 가 아닐까 생각한다. 개념을 익혀보기 위해 조금 복잡한 예시를 준비했다. 아래 `CREATE` 쿼리는 로그 스페이스를 생성하는 쿼리다.

```sql
CREATE TEMPORARY TABLESPACE WCH18735_LOGSPACE 
    TEMPFILE 
        'C:\ORACLE\PDB1\LOG_FILE' SIZE 524288000 AUTOEXTEND ON NEXT 524288000 MAXSIZE 1073741824;

CREATE SMALLFILE TABLESPACE WCH18735_TABLESPACE 
    DATAFILE 
        'C:\ORACLE\PDB1\DATA_FILE1' SIZE 524288000 AUTOEXTEND ON NEXT 524288000 MAXSIZE 1073741824 
    BLOCKSIZE 8192 
    DEFAULT NOCOMPRESS NO INMEMORY 
    ONLINE 
    SEGMENT SPACE MANAGEMENT AUTO 
    EXTENT MANAGEMENT LOCAL AUTOALLOCATE;
```

위에서 설명한 생성 키워드 `CREATE` 를 이용해 아래 그림과 같은 테이블 구조를 생성할 수 있다. 

<img src="/_img/2021-11-16/table_structure.jpg" style="margin: atuo auto; display: block;">

이를 위한 테이블 생성 쿼리는 아래와 같다. 여기서 `"COMMENT"` 로 나타낸 이유는 COMMENT 가 **예약어**이기 때문이다. 예약어를 테이블 명으로 사용하기 위해서 문자열임을 나타내기 위해 큰따옴표를 사용한다.

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

## ALTER

`ALTER`는 테이블을 변화시킬 때 사용하는 키워드이다. `ALTER` 문의 예시로 이전에 지정된 `DEFAULT TABLESPACE`와 `TEMPORARY TABLESPACE`를 바꾸는 쿼리를 준비했다. 기존에 Default 로 지정된 테이블 스페이스를 사용자가 생성한 테이블 스페이스로 지정하는 아래 예시를 보자.

```sql
-- USER SQL
ALTER USER "WCH18735"
DEFAULT TABLESPACE "WCH18735_TABLESPACE"
TEMPORARY TABLESPACE "WCH18735_LOGSPACE"
ACCOUNT UNLOCK ;
```

`ALTER` 키워드는 값 뿐만 아니라 특정 컬럼 이름을 변경하는 방법도 가능하다. 이를 통해 테이블 컬럼에 오타가 있어도 쉽게 수정할 수 있으나, 기존에 예약된 값으로는 변경 불가능하다.

```sql
/* column 명 변경 */
ALTER TABLE TEST_TABLE RENAME COLUMN COLLOR TO COLOR;
```

## INSERT

Oracle 에서 사용하는 INSERT 명령 규칙은 `INSERT INTO [TABLE NAME] VALUES [값 목록]` 과 같다. 이 때는 테이블을 생성 할 때 기재했던 순서대로 값 목록을 작성해야 한다. 

만약 원하는 필드만 원하는 순서대로 입력하고 싶을 때는 `INSERT INTO [TABLE NAME](COLUMN_A, COLUMN_B, ...) VALUES [값 목록] (VALUE_A, VALUE_B, ...)` 형태로 사용한다. 순서를 임의로 지정할 수 있으며, SQL에 대응되는 값들이 나타나있어 파악이 빠르다.

INSERT 구문 실행 시 DEFAULT 가 정해지지 않은 컬럼에 NULL 값이 입력된다. 이렇게 한 번 데이터를 삽입한 다음 `SELECT * FROM MEMBER` 를 통해 배운 내용을 확인해보자.

```sql
INSERT INTO MEMBER(ID, PWD) VALUES('wch18735', '111');
INSERT INTO MEMBER(ID, PWD) VALUES('dragon', '111');
```

위 구문을 실행한 뒤 결과를 확인해보면 아래와 같이 삽입이 정상적으로 이뤄진 것을 볼 수 있다.

<br/>
<img src="/_img/2021-11-16/member_select_all.jpg" style="margin: atuo auto; display: block;">
<br/>

## SELECT

데이터 조회는 `1) 특정 컬럼을 지정` 하고 `2) 컬럼 명을 바꾸어` 조회할 수 있다. 명령 규칙은 기본적으로 `SELECT [COLUMN NAME], ... FROM [TABLE NAME]` 이다.

```sql
SELECT ID "USER ID", NAME, PWD "password" FROM MEMBER;
```

이 때, 컬럼 이름 옆에 `쌍따옴표`를 이용해 별칭을 지정하기만 하면 테이블이 출력될 때 사용될 컬럼 명을 지정할 수 있다. `AS` 를 사용하지 않아도 별칭을 지정할 수 있는 것이다. 이로써 별칭에 공백을 포함하는 방법은 큰 따옴표로 감싸는 것이다. 아래는 활용 예시다.

<br/>
<img src="/_img/2021-11-16/member_select_col.jpg" style="margin: atuo auto; display: block;">
<br/>

## UPDATE

기존 값, 컬럼의 데이터 타입 등 **변화**를 적용하기 위해서 사용되는 키워드가 `UPDATE` 이다. 명령 규칙은`UPDATE <TABLE> SET <COLOUMN>=VALUE WHERE CONDITION` 형태로 나타낼 수 있다. 값을 바꾸는 예시로 아이디가 `wch18735` 인 사람의 비밀번호를 `222`로 바꿔보자.

```sql
UPDATE MEMBER SET PWD='222' WHERE ID='wch18735';
```

이 때, 만약 여러 COLUMN 을 여러 개 변경하는 방법은 아래와 같이 콤마를 사용해 동시에 적용 가능하다. 

```sql
UPDATE MEMBER SET NAME='BLUE EYE', PWD='111' WHERE ID='dragon';
```

## DELETE

특정 테이블의 행을 삭제하고 싶다면, 아래와 같이 `WHERE` 절을 삽입해 조건에 맞는 행을 골라내 삭제 할 수 있다. 이와 `DROP` 명령어와 `TRUCATE` 명령어와 헷갈리는 경우가 종종 있는데, 둘은 행 정보 변경보다 큰 범위인 테이블 형태를 결정하는 차이가 있다.

```sql
DELETE MEMBER WHERE ID='dragon';
```