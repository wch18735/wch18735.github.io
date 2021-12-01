---
title: "[Oracle] Constraints 예시와 사용법 (제약 조건)"
excerpt: "oracle constraints examples"
date: 2021-11-20
category:
    - oracle
tag:
    - database
    - oracle
    - sql
    - constraints
    - example
    - not null
    - default
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# Constraints (제약 조건)

Oracle 의 다양한 제약 조건을 설명한다. 제약 조건은 데이터의 무결성을 지키기 위해 생성된다. 테이블이나 속성에 부적절한 값이 종속되지 않도록 미연에 방지하는 것을 목적으로 한다.

## Domain Constraints

속성에 도메인이 아닌 값이 올 수 없도록 하는 제약 조건을 도메인 제약 조건이라 한다. 도메인이란 특정한 영역을 말한다. 정수, 문자열과 같은 데이터 타입을 포함해 정수의 범위, 문자열의 길이 등이 정해진 포괄적 개념이다.

제약 조건은 아래와 같다.

- **NOT NULL :** 입력하지 않으면 에러 

```sql
-- CREATE TABLE (테이블 생성) 상황에서 DEFAULT 제약 조건
CREATE TABLE NOT_NULL_TABLE
(
    ID      VARCHAR2(50)    NOT NULL,
    EMAIL   VARCHAR2(100)   NULL,
    PHONE   CHAR(13)        NULL
)

-- ALTER TABLE + MODIFY (테이블 수정) 상황에서 DEFAULT 제약 조건
ALTER TABLE NOT_NULL_TABLE MODIFY EMAIL VARCHAR2(100) NOT NULL;
```

- **DEFAULT:** 입력하지 않으면 DEFAULT 값 부여

```sql
CREATE TABLE DEFAULT_TABLE
(
    ID      VARCHAR2(50)    NOT NULL,
    EMAIL   VARCHAR2(100)   NULL,
    PHONE   CHAR(13)        NULL,
    PWD     VARCHAR(15)     DEFAULT '111'
)

-- ALTER TABLE + MODIFY (테이블 수정) 상황에서 NOT NULL 제약 조건
ALTER TABLE DEFAULT_TABLE MODIFY EMAIL VARCHAR2(100) DEFAULT '111';
```

- **CHECK:** 입력 시 사용자가 지정한 조건 검사

```sql
-- CREATE TABLE (테이블 생성) 상황에서 CHECK 제약 조건
CREATE TABLE CHECK_TABLE
(
    ID      VARCHAR2(50)    NULL,
    PHONE   CHAR(13)        CHECK(PHONE LIKE '010-____-____')  NOT NULL,
    EMAIL   VARCHAR(500)    NULL
)

-- ALTER TABLE + ADD (제약 조건 추가) 상황에서 제약 조건
ALTER TABLE CHECK_TABLE CONSTRAINT CK_PHONE CHECK(PHONE LIKE '010-____-____')
```

**CHECK 제약 조건**에서 조금 더 정밀한 검사를 수행하기 위해서 정규식(Regular Expression)을 사용할 수 있다. 실제로 위 테이블에는 `010-aaaa-bbbb` 라는 문자열도 입력이 가능하다.

이를 알맞게 수정하면 아래와 같다.

```sql
-- 기존 제약 조건 삭제
ALTER TABLE CHECK_TABLE
DROP CONSTRAINT CK_PHONE;

-- 정규식을 이용한 제약 조건
ALTER TABLE CHECK_TABLE CONSTRAINT CK_PHONE CHECK(PHONE REGEXP_LIKE(PHONE, '^01[01]-\d{3,4}-\d{4}$'))
```

이런 제약 조건들은 `SELECT * FROM USER_CONSTRAINTS` 테이블 뷰에서 확인 가능하고 `WHERE` 절에 테이블 이름으로 조회 가능하다.

## Entity Constraints (엔티티 제약 조건)

테이블 개체 범위에서의 제약 조건 설정.

- **PRIMARY KEY:** 레코드를 구분하는 절대적인 식별자
  - 중복 발생 불가
  - NULL 불가
- **UNIQUE:** 중복을 허용하지 않는 컬럼
  - 중복 발생 불가
  - NULL 가능

```sql
CREATE TABLE NOTICE
(
    ID          NUMBER          CONSTRAINT NOTICE_ID_PK PRIMARY KEY,
    TITLE       VARCHAR2(300)   NOT NULL,
    WRITER_ID   VARCHAR2(50)    NOT NULL UNIQUE
)
```

CONSTRAINT 제약 조건 설정으로 쿼리가 길어질 때, 아래와 같은 리팩토링이 가능하다.

```sql
CREATE TABLE NOTICE
(
    ID          NUMBER,
    TITLE       VARCHAR2(300)   NOT NULL,
    WRITER_ID   VARCHAR2(50)    NOT NULL

    CONSTRAINT NOTICE_ID_PK PRIMARY KEY(ID),
    CONSTRAINT NOTICE_WRITER_ID UNIQUE(WRITER_ID)
)
```

생성 시가 아니라 수정 시에도 Entity Constraints 를 수정할 수 있다.

## SEQUENCE (시퀀스)

자동으로 순차적으로 증가하는 순번을 반환하는 데이터베이스 객체. 게시판에 글이 추가될 때마다 글 번호가 증가되도록 설정하는 경우가 가장 보편적인 예시다.

```sql
-- 생성 방법
CREATE SEQUENCE [시퀀스명]
INCREMENT BY [증감숫자]     -- 증가: 양수 / 감소: 음수 / DEFAULT: 1
START WITH [시작숫자]       -- 증가: MINVALUE / 감소: MAXVALUE
NOMINVALUE OR MINVALUE [최솟값]     -- NOMINVALUE : 최소값에 DEFAULT 값 부여( 증가: 1 / 감소: -1028) 
                                    -- MINVALUE : 최소값 설정 (최소값 <= 시작숫자 && 최소값 < MAXVALUE)
NOMAXVALUE OR MAXVALUE [최대값]     -- NOMAXVALUE : 최대값에 DEFAULT 값 부여( 증가: 1027 / 감소: -1) 
                                    -- MAXVALUE : 최대값 설정 (최대값 >= 시작숫자 && 최대값 > MINXVALUE)
CYCLE OR NOCYCLE --CYCLE 설정시 최대값에 도달하면 최소값부터 다시 시작 NOCYCLE 설정시 최대값 생성 시 시퀀스 생성중지
CACHE OR NOCACHE --CACHE 설정시 메모리에 시퀀스 값을 미리 할당하고 NOCACHE 설정시 시퀀스값을 메로리에 할당하지 않음
```

사용 예시는 아래와 같다.

```sql
-- SEQUENCE.NEXTVAL 이용해 다음 ID 적용 가능
INSERT INTO NOTICE(ID, TITLE, WRITER_ID)
VALUES (NOTICE_ID_SEQ.NEXTVAL, 'SEQUENCE TEST', '1FES')
```

이때, **ID**를 입력하지 않고도 행을 증가시킬 수 있도록 하려면 **Trigger**를 사용한다. 트리거는 `데이터베이스 시스템에서 데이터의 입력, 갱신, 삭제 등의 이벤트가 발생할 때 마다 자동적으로 수행되는 사용자 정의 프로시저` 이다. SQL 제약조건만으로 명시할 수 없는 무결성 제약조건을 구현하는 방법이다.

이건 나중에...