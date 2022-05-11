---
title: "[Oracle] Oracle HR Schema 생성"
excerpt: "how to create oracle hr schema"
date: 2022-05-11
category:
    - oracle
tag:
    - database
    - oracle
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# HR Schema 생성 방법

1. sqlplus 접속
   - user : system / password : 초기 패스워드
2. HR 스키마 확인 : `$ORACLE_HOME/demo/schema/human_resources`
3. HR 계정 기본 정보 삭제
```sql
create user hr identified hr;
drop user hr cascade;
```
4. `@?/demo/schema/human_resources/hr_main.sql`
5. 아래 네 가지 입력
   - HR 스키마 사용자 패스워드 : hr
   - 기본 테이블 스페이스 : USERS
   - 임시 테이블스페이스  : TEMP
   - 로그 경로 : `OracleDB/demo/schema/log`

※ 3번의 HR 계정 기본 정보 삭제를 하지 않거나, 비밀번호에 특수문자가 포함되면 아래와 같은 에러가 나온다

`ORA-20000: Schema "HR" does not exist or insufficient privileges`