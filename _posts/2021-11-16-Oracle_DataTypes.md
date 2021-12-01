---
title: "[Oracle] Data Types"
excerpt: "oracle data types"
date: 2021-11-14
category:
    - oracle
tag:
    - database
    - oracle
    - sql
    - data type
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# Oracle Built-in Data Types

옛날에 배웠던 것들을 다시 한 번 복습하면서 느낀점.

`뭐지 나 왜 아무것도 모르지.`

심각함을 느끼고 가장 기본적인 데이터 타입들부터 정리한다.

데이터 타입 범주는 아래와 같이 나뉜다.

- Character 형식
- Numeric 형식
- Data 형식
- LOB 형식

## Character 형식

이 네 가지 중 Character 형식 먼저 알아보자.

문자를 표현하는 자료형은 네 가지가 있다.

- CHAR
- VARCHAR
- NCHAR
- NVARCHAR

`CHAR(N)` 는 N 개의 고정 길이 공간을 마련하는 것. 이때, 한 칸 당 사이즈는 **1Byte**이다.

`VARCHAR(N)`는 가변 길이 공간을 뜻한다. **VAR** 는 Variable 을 뜻한다.

그럼 모든 자료형을 VARCHAR로 표현하면 좋지 않을까? 

**Nope.**

100칸의 공간을 만들어 10칸만 사용하면 나머지를 반환하는 것이 아니라 구분자를 이용해 사용하지 않는 공간을 구분한다. 따라서 검색할 때 **구분자 수를 세야 한다.**

반대로 고정 길이에서 검색이 쉽다. 고정길이 N Byte를 지정하면 **N개 * 검색행 만큼 포인터를 옮기면 검색이 끝난다.**

`N` 은 National 을 뜻한다. 다양한 나라 언어를 모두 포함할 수 있다. 그러나 많은 공간을 사용한다. 대략 3배(3Byte) 정도.

전세계 각국 문자를 한 번에 표현하는데 활용된다고 한다.

이제 이전에 만들었던 MEMBER 테이블 자료형을 수정해보자.

```sql
CREATE TABLE MEMBER (
    ID          VARCHAR2(50),
    PWD         VARCHAR2(50),
    NAME        VARCHAR2(50),
    GENDER      CHAR(2),        -- 남성 / 여성
    AGE         NUMBER,             
    BIRTHDAY    CHAR(10),       -- 2021-11-16
    PHONE       CHAR(13),       -- 010-2639-0065
    REGDATE     DATE
);
```

여기서 한 가지 의문. **GENDER** 는 올바르게 설정됐을까? 

(다른 성소수자를 고려했느냐를 묻는 질문은 아니다... 나는... 존중합니다..)

아래 쿼리를 실행시켜 결과를 확인해보자.

```sql
SELECT LENGTH('AB') FROM DUAL;
-- 결과: 2

SELECT LENGTHB('AB') FROM DUAL;
-- 결과: 2
```

현재 GENDER 는 **2 Byte** 가 할당되어 있다.

그런데 한글은 한 글자가 몇 바이트를 차지할까?

```sql
SELECT LENGTHB('남성') FROM DUAL
-- 결과: 6
```

그렇기 때문에 한글이 들어가는 경우를 종합한 최종 테이블 생성문은 아래처럼 바뀐다.

```sql
CREATE TABLE MEMBER (
    ID          VARCHAR2(50),
    PWD         NVARCHAR2(50),
    NAME        NVARCHAR2(50),
    GENDER      NCHAR(2),       -- 남성 / 여성
    AGE         NUMBER(3),      -- 나이 999살 까지            
    BIRTHDAY    CHAR(10),       -- 2021-11-16
    PHONE       CHAR(13),       -- 010-2639-0065
    REGDATE     DATE
);
```

### LONG, CLOB, NCLOB

- **LONG:** 2GB 까지 지원하는 문자열 데이터 타입. 예전에 사용된 데이터 형식.
- **CLOB:** 대용량 텍스트 데이터 타입 (최대 4GB)
- **NCLOB:** 대용량 텍스트 유니코드 데이터 타입 (최대 4GB)

## Numeric 형식

- **NUMBER:** 숫자를 표현하는 데이터 타입
  - NUMBER(4) : 최대 4자로 이루어진 숫자
  - NUMBER(6, 2) : 소수점 2자리를 포함하는 최대 6자리 (소수점 둘째 자리에서 반올림)
  - NUMBER(6, -2) : 소수점 -2자리에서 반올림하는 최대 6자리 숫자
  - NUMBER : NUMBER(38, *) 과 같음. 최대 자리수가 38자리인 숫자

## Date 형식

4712 BC ~ 9999 AD 까지를 **년, 월, 일**로 표현함.

여기 **시, 분, 초**를 표현하고자 한다면 `TIMESTAMP` 사용.

## 테이블 수정

EMAIL을 추가하고, ID와 AGE 데이터 타입을 바꿔야 하기 때문에 아래 SQL문을 수행한다.

만약 AGE COLUMN을 지우려면 DROP으로 날린다. 

```sql
ALTER TABLE MEMBER MODIFY ID NVARCHAR2(20);
ALTER TABLE MEMBER MODIFY AGE NUMBER(3);
-- ALTER TABLE MEMBER DROP COLUMN AGE; : 없앨 때
ALTER TABLE MEMBER ADD EMAIL VARCHAR2(100);
```

보통 DBMS를 많이 사용하는 사람들은 보통 SQLDEVELOPER를 사용한다.

하... 근데 개인적으로 SQLDEVELOPER는 너무 개복치라는 느낌을 지울 수 없다...