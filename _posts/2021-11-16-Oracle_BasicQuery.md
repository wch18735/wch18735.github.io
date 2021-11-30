---
title: "[Oracle] DDL, DML, DCL 개념과 CREATE, ALTER"
excerpt: "ddl, dml, dcl and create query"
date: 2021-11-16
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

# SQL 구분

SQL은 성격에 따라 다음과 같이 구분할 수 있으며 각각에 사용되는 키워드를 살펴보면 개념을 쉽게 떠올릴 수 있을 것이다.

- **DDL :** CREATE, ALTER, DROP 
- **DML :** INSERT, SELECT, UPDATE, DELETE - CRUD
- **DCL :** GRANT, REVOKE

테이블 자체를 생성, 변경, 누락하는 명령어를 DDL이라 한다. Table을 조작하는 DML과 조금 차이가 있다. 즉, 변경 대상이 다르다고 할 수 있다. DCL 은 권한과 관련된 쿼리라고 생각하자.

# 테이블 생성하기 - CREATE

먼저 아래와 같은 테이블 생성문을 작성한다. 테이블을 많이 만들어 본 관리자는 자료형이 적절하지 않다고 느낄 수 있다. 

맞다. 좀 이상하다. 넘어가자.

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

## CREATE 활용 예시

SQL을 배우게 되면 가장 먼저 실행해보는 문장이 CREATE 가 아닐까 생각한다. 개념을 익혀보기 위해 조금 복잡한 예시를 준비했다. 아래 CREATE 쿼리는 로그 스페이스를 생성하는 쿼리다.

```sql
CREATE TEMPORARY TABLESPACE WCH18735_LOGSPACE 
    TEMPFILE 
        'C:\ORACLE\PDB1\LOG_FILE' SIZE 524288000 AUTOEXTEND ON NEXT 524288000 MAXSIZE 1073741824;
```

또 다른 예시로 실제 데이타 파일을 생성하는 쿼리를 준비했다. 사이즈를 지정하고 여러 설정값을 지정하는 모습을 볼 수 있다.

```sql
CREATE SMALLFILE TABLESPACE WCH18735_TABLESPACE 
    DATAFILE 
        'C:\ORACLE\PDB1\DATA_FILE1' SIZE 524288000 AUTOEXTEND ON NEXT 524288000 MAXSIZE 1073741824 
    BLOCKSIZE 8192 
    DEFAULT NOCOMPRESS NO INMEMORY 
    ONLINE 
    SEGMENT SPACE MANAGEMENT AUTO 
    EXTENT MANAGEMENT LOCAL AUTOALLOCATE;
```

CREATE가 단순하게 테이블 생성에만 쓰이는 것은 아니란 것을 기록하고, 함께 알기 위해 이런 예시를 준비했다. 이건 *sqldeveloper* 에서 새로운 접속을 생성할 때, SQL 탭을 눌러 확인해 볼 수 있다.

## ALTER 활용 예시

ALTER는 테이블을 변화시킬 때 사용하는 키워드이다. ALTER 문의 예시로는 DEFAULT TABLESPACE와 TEMPORARY TABLESPACE를 이전에 생성한 데이터베이스 테이블 스페이스로 지정하는 아래 예시를 보자.

```sql
-- USER SQL
ALTER USER "WCH18735"
DEFAULT TABLESPACE "WCH18735_TABLESPACE"
TEMPORARY TABLESPACE "WCH18735_LOGSPACE"
ACCOUNT UNLOCK ;
```