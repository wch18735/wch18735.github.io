---
title: "[Oracle] Operator (산술, 문자열, 비교, 관계, 패턴)"
excerpt: "select by using operator"
date: 2021-11-17
category:
    - oracle
tag:
    - database
    - oracle
    - sql
    - oracle operator
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# 연산을 통한 데이터 조회

Oracle 에는 다양한 연산이 존재한다.

## 산술 연산자

공지사항의 조회수를 조회할 때, 기존 값에 1을 더해 조회하는 조회문은 다음과 같다.

이때, 컬럼명은 다음과 같이 HIT을 주어 표현할 수 있다. 

```sql
INSERT INTO NOTICE(ID, HIT) VALUES(1, 1);
SELECT HIT+1 HIT FROM NOTICE;
```

## 문자열 연산자

그렇다면 이러한 작업은 어떻게 동작할까? DUMMY 테이블 DUAL 에 아래 SQL 문을 동작시켜 결과를 확인하면 다음과 같다.

```sql
SELECT 1+'3' FROM DUAL;
-- 결과: 4
```

ORACLE 에서는 문자열을 더하는 특별한 연산자 `||` 가 있기 때문에 `+`는 항상 숫자로 변환되어 계산된다.

따라서 결과는 다음과 같이 나타난다.

```sql
SELECT 1 || '3' FROM DUAL;
-- 결과: 13
```

### 예시 문제

위 연산자들을 이용해 조회 시 새로운 자료를 만들어 제공할 수 있다.

예시 문제를 풀어보자. `모든 회원의 이름을 조회하고, 형식은 "이름(ID)" 로 지정한다.`

```sql
SELECT NAME || '(' || ID || ')' NAME FROM MEMBER;
```

## 비교 연산자

데이터를 필터링 할 때 사용하는 연산자.

`WHERE`절과 함께 사용한다.

- `=` : 같다
- `!=` , `^=`, `<>` : 같지 않다
- `>`, `<` : 대소 비교
- `>=`, `<=` : 대소 비교 (같음 포함)
- `IS NULL`, `IS NOT NULL` : NULL 비교

비교 연산자 예시를 몇 가지 확인해보자.

1) 게시글 중에서 작성자가 `'A'` 인 게시글만 조회하십시오.

```sql
SELECT * FROM NOTICE WHERE WRITER_ID = 'A';
```

2) 게시글 중에서 조회수가 3이 넘는 글만 조회하십시오.

```sql
SELECT * FROM NOTICE WHERE HIT > 3;
```

3) 게시글 중에서 내용을 입력하지 않은 게시글을 조회하시오.

```sql
SELECT * FROM NOTICE WHERE CONTENTS IS NULL;
```

## 관계 연산자

관계 연산자를 이용해 조건절에서의 관계를 표현할 수 있다.

- NOT
- AND
- OR
- BETWEEN
- IN

관계 연산자 예시를 보면 활용 방안을 쉽게 이해할 수 있다.

1) 조회수가 0, 1, 2 인 게시글을 조회하시오.

```sql
SELECT * FROM NOTICE WHERE HIT=0 OR HIT=1 OR HIT=2;
```

2) 조회수가 0, 1, 2, ... , 9 인 게시글을 조회하시오.

```sql
SELECT * FROM NOTICE WHERE 0 <= HIT AND HIT < 10; 
```

3) 조회수가 0 부터 100 까지 있는 게시글을 조회하시오.

```sql
SELECT * FROM NOTICE WHERE HIT BETWEEN 0 AND 100;
-- 0 <= HIT <= 100
```

4) 조회수가 0, 2, 7 인 게시글을 조회하시오.

```sql
SELECT * FROM NOTICE WHERE HIT IN (0, 2, 7);
```

5) 조회수가 0, 2, 7 이 아닌 게시글을 조회하시오.

```sql
SELECT * FROM NOTICE WHERE HIT NOT IN (0, 2, 7);
```

## 패턴 연산자

문자열 패턴을 비교하는 연산자.

ORACLE 이 기본적으로 제공한다.

- `LIKE` : `WHERE` 절의 조건으로 사용되는 예약어
- `%` : 모든 경우
- `_` : 한 글자 모두 포함

예시를 보면 이해할 수 있다.

1) 회원 중에서 `'박'`씨 성을 가진 사람을 조회하라.

```sql
SELECT * FROM MEMBER WHERE NAME LIKE '박%';
```

2) 회원 중에서 `'박'` 씨 성이고 이름이 외자인 사람을 조회하라.

```sql
SELECT * FROM MEMBER WHERE NAME LIKE '박_';
```

3) 회원 이름 중에서 `'도'` 자가 들어간 회원을 조회하시오.

```sql
SELECT * FROM MEMBER WHERE NAME LIKE '%도%';
```

### 정규식을 이용한 패턴 비교

영어로는 **Regular Expression** 이라 한다.

[https://regexlib.com/CheatSheet.aspx](https://regexlib.com/CheatSheet.aspx) 페이지에서 활용 방법을 확인할 수 있다. 기억이 안 날 때 참고해보자.

간단한 예시들은 다음과 같다.

1) 대한민국 전화번호 패턴

```re
^01[016-9]-\d{3, 4}-\d{4}$
```

주의할 점이 한 가지 있다.

앞의 `^` 이 붙으면 무조건 전화번호로 시작되는 행만 찾을 수 있다.

마찬가지로 `$` 이 붙으면 무조건 전화번호로 끝나는 행만 찾을 수 있다.

번호가 중간에 있으면 찾을 수 없다. 내용 중간에 있는 핸드폰 번호를 찾으려면 시작, 끝 기호를 떼주자.

이를 SQL 문에 사용하는 방법은 다음과 같다.

```sql
SELECT * FROM NOTICE WHERE REGEXP_LIKE(TITLE, '01[016-9]-\d{3, 4}-\d{4}');
```

2) 유효한 EMAIL 패턴

`한 개 이상의 숫자로 시작하지 않는 문자열` + `@` + `도메인 명` + `.` + `com / org / net` 을 EMAIL 표준이라 나타낼 시 아래 형태를 따른다.

```
\D*@\D*.(com|org|net)
```

Alternation 을 이용해 단어를 표현해준다.