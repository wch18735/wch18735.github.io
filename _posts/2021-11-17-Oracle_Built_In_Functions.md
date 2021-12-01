---
title: "[Oracle] Built-in functions"
excerpt: "character, numeric, date, null built-in functions"
date: 2021-11-17
category:
    - oracle
tag:
    - database
    - oracle
    - sql
    - built-in
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# Oracle Built-in functions

이번 포스트에서는 오라클 데이터베이스에서 지원하는 다양한 내장 함수를 알아보려 한다. 총 정리까지는 아니지만 SQLD 공부할 때, 꽤 도움이 될 수도...?

비단 자격증을 위해서가 아니라! 적절한 함수를 사용해 원하는 값으로 데이터를 조회하는 능력이 곧 데이터베이스 활용 능력이라 봐도 무방하다고 생각한다.

## 문자열 함수

이번 포스트에서 제일 많은 양을 차지하는 부분이 문자열 함수다. 그만큼 중요하다. 조회 수준을 고도화 할 수록 클라이언트 파트에서 처리할 일을 배로 줄일 수 있다.

### SUBSTR(문자열, 시작위치, 길이)

문자열을 자르는 함수이다. 아래와 같은 방식으로 활용된다. 주의할 점은 한글 한 글자는 3 BYTE 이기 때문에 `SUBSTRB()` 함수에서 원하는대로 동작하지 않을 수 있다는 것이다.

```sql
SELECT SUBSTR('HELLO', 1, 3) FROM DUAL;
-- HEL (1부터 3까지)
SELECT SUBSTR('HELLO', 3) FROM DUAL;
-- LLO (3번째 부터)
SELECT SUBSTRB('HELLO', 3) FROM DUAL;
-- LLO
SELECT SUBSTRB('안녕하세요', 3) FROM DUAL;
-- 녕하세요
```

**예시)** 모든 학생의 이름과 출생 월을 조회하시오.

```sql
SELECT NAME, SUBSTR(BIRTHDAY, 6, 2) MONTH FROM MEMBER;
```

또한 다음과 같이 조건절에서도 활용 가능하다.

**예시)** 학생 중 전화번호가 011로 시작하는 회원의 모든 정보를 출력하시오.

```sql
SELECT * FROM MEMBER WHERE SUBSTR(PHONE, 1, 3) = '011';
-- OR
SELECT * FROM MEMBER WHERE PHONE LIKE '011%';
```

사실 두 번째 방법이 RECORD 가 많을 때, 조금 더 효율적이라 할 수 있다.

### CONCAT('문자열', '문자열')

문자열 덧셈 연산자 `||`와 비슷하다.

성능 면에서 `||` 이 빠르게 동작하는 것을 유념하자.

### LTRIM(), RTRIM(), TRIM()

공백을 없애주는 함수.

각각 왼쪽, 오른쪽, 양쪽 공백을 없애주는 함수이다.

### LOWER(), UPPER()

소문자 또는 대문자로 변환하는 문자열 내장 함수.

### REPLACE(문자열, 찾는 문자열, 대치할 문자열)

전체 문자열에서 특정 문자열을 다른 문자열로 대체하는 함수.

`TRANSLATE(문자열, N개 문자열, N개 대체 문자열)` 는 문자를 통째로 바꾸는 것이 아니라 대응되는 문자가 모두 바뀌는 것이 있다.

```sql
SELECT REPLACE('WHERE ARE YOU?', 'YOU', 'WE') FROM DUAL;
-- WHERE ARE WE?
```

### LPAD(), RPAD()

지정한 문자로 채우는 함수.

```sql
SELECT LPAD('HELLO', 10, '*') FROM DUAL;
-- *****HELLO
SELECT RPAD('HELLO', 10, '*') FROM DUAL;
-- HELLO*****
```

### INITCAP(문자열)

첫 글자를 대문자로 바꿔주는 함수.

모든 단어를 대문자로 바꿔준다.

```sql
SELECT INITCAP('the most important thing in the world is love.') FROM DUAL;
-- The Most Important Thing In The World Is Love.
```

### INSTR(문자열, 검색문자열, 찾기 시작하는 위치, N번째 찾을 개수)

문자열 검색 함수. 특정 문자열에서 특징을 뽑아낸다.

```sql
SELECT INSTR('ALL WE NEED TO DO IS JUST TO TRY.', 'TO') FROM DUAL;
-- 13

SELECT INSTR('ALL WE NEED TO DO IS JUST TO TRY.', 'TO', 1, 2) FROM DUAL;
-- 27
```

이를 이용해서 전화번호 대시 문자(-) 사이에 몇 개가 있는지 알아낼 수 있다.

```sql
SELECT INSTR('010-0000-0000', '-', 1, 2) - INSTR('010-0000-0000', '-', 1, 1) - 1 FROM DUAL;
```

이외에도 아스키 코드 값을 숫자로, 숫자를 아스키로 바꾸는 함수 등도 존재한다.

## 숫자 함수

문자열 함수와 마찬가지로 숫자형 데이터 형식에 활용되는 함수를 사용해본다.

- 절대 값을 구하는 함수
```sql
SELECT ABS(35), ABS(-35) FROM DUAL;
-- 35, 35
```

- 음수/양수를 알려주는 함수
```sql
SELECT SIGN(35), SIGN(-35) FROM DUAL;
-- 1, -1
```

- 숫자의 반올림 값을 알려주는 함수
```sql
SELECT ROUND(34.456789), ROUND(34.56789) FROM DUAL;
-- 34, 35

SELECT ROUND(34.456789, 2) FROM DUAL;
-- 34.46
```

- 숫자의 몫과 나머지 값을 반환하는 함수
```sql
SELECT TRUNC(17/5), MOD(17, 5) FROM DUAL;
-- 3, 2 (몫, 나머지)
```

- 숫자의 제곱을 구하는 함수와 제곱근을 구하는 함수
```sql
SELECT POWER(5, 2), SQRT(25) FROM DUAL;
-- 25, 5

SELECT ROUND(SQRT(2), 2) FROM DUAL;
-- 1.41
```

## 날짜 함수

날짜와 관련된 함수.

- 현재 시간을 얻는 함수
  - SYSDATE : 오라클 서버에서 제공하는 시간
  - CURRENT_DATE : 접속자의 시간 (세션 시간 변경을 통해 설정 가능)
  - 시, 분, 초 정보를 얻으려면 TIMESTAMP 활용
```sql
SELECT SYSDATE, CURRENT_DATE, SYSTIMESTAMP, CURRENT_TIMESTAMP FROM DUAL;
-- 21/11/17	21/11/17	21/11/17 14:30:19.539000000 +09:00	21/11/17 14:30:19.539000000 ASIA/SEOUL
```

- 세션 시간과 포맷 변경
  - NLS : National Language Support
```sql
ALTER SESSION SET TIME_ZONE='09:00';
-- 그리니치 천문대 기준 +09:00

ALTER SESSION SET NLS_DATE_FORMAT = 'YYYY-MM-DD HH24:MI:SS';
-- (AFTER SESSION CHANGE) SYSDATE: 2021-11-17 14:35:11
```

- 날짜 추출 함수
```sql
SELECT EXTRACT(YEAR FROM SYSDATE) FROM DUAL;
SELECT EXTRACT(MONTH FROM SYSDATE) FROM DUAL;
-- ...

-- 예제: 가입 인원 중 비수기에 가입한 회원 정보
SELECT * FROM MEMBER WHERE EXTRACT(MONTH FROM REGDATE) IN (2, 3, 11, 12);
```

- 날짜 누적 함수
```sql
SELECT ADD_MONTHS(SYSDATE, 1) FROM DUAL;
-- 2021-12-17 14:41:55

-- 예제) 현재 날짜에서 가입 일자가 6개월 미만인 회원 정보
SELECT * FROM MEMBER WHERE ADD_MONTHS(SYSDATE, -6) < REGDATE;
```

- 날짜 차이를 알아내는 함수
```sql
-- 현재 날짜와 2013-12-25 까지 개월 차이
-- 결과: 94.76176784647550776583034647550776583035
SELECT MONTHS_BETWEEN(SYSDATE, TO_DATE('2013-12-25')) FROM DUAL;
```

- 다음 날짜를 알아내는 함수
```sql
-- 오늘 날짜와 가장 가까운 요일에 해당하는 날짜
SELECT NEXT_DAY(SYSDATE, '월') FROM DUAL;
```

- 월의 마지막 날짜를 알려주는 함수
```sql
SELECT LAST_DAY(SYSDATE) FROM DUAL;
-- 2021-11-30 14:49:15

SELECT LAST_DAY(TO_DATE('2021-02-01')) FROM DUAL;
-- 2021-02-28 00:00:00
```

이 외에도 많은 날짜 관련 함수들이 존재한다.

모두 암기할 수 없으니 참조 문서를 사용하여 기능 개발에 주력할 수 있도록 한다.

## 변환 함수

`TO_CHAR()`, `TO_DATE()`, `TO_NUMBER()`, `TO_CHAR()` 등의 변환 함수가 존재한다.

각 함수들은 특정 포맷을 만족시키며 자료들을 변환할 수 있다.

```sql
SELECT TO_CHAR(123456789, '$9,999,999,999') FROM DUAL;
--    $123,456,789
```

이때, 지정해준 패턴만큼 공백이 발생하기 때문에 TRIM 을 사용해 공백을 제거하는 방법을 추천한다.

## NULL 함수

- 반환 값이 NULL 일경우에 대체 값을 제공하는 함수
```sql
SELECT NVL(AGE, 0) FROM MEMBER;
```

- NOTNULL 일 경우 대체 값까지 지정해 반환하는 함수
```sql
-- NOTNULL: 1 / NULL: 0
SELECT NVL2(AGE, 1, 0) FROM MEMBER;
```

- 두 값이 같은 경우 NULL 그렇지 않은 경우 첫 번째 값 반환
```sql
SELECT NULLIF(AGE, 19) FROM MEMBER;
```

- 조건에 따른 값 선택하기
```sql
-- DECODE(조건, TURE, THEN THIS, TRUE, THEN THIS, ... DEFAULT);
SELECT DECODE(GENDER, '남성', 1, 2) FROM MEMBER;
SELECT DECODE(SUBSTR(PHONE, 1, 3),  '011', 'SK', 
                                    '016', 'KT', 
                                    '기타');
```