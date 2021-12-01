---
title: "[Oracle] Join 활용방법과 예제"
excerpt: "inner join, outter join, etc."
date: 2021-11-20
category:
    - oracle
tag:
    - database
    - oracle
    - sql
    - join
    - example
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# 조인 (JOIN)

**조인(JOIN)** 은 쉽게 이해하자면 여러 데이터베이스를 하나로 묶는 것이다. **하나의 독립된 기준(KEY)** 이용해 `1) 중복된 것들은 없애고` `2) 새로운 것들은 추가하여` 무결성을 유지하는 작업. 즉, 여러 참조된 테이블을 합치는 작업이다.

서로 다른 테이블들은 서로 관계를 가질 수 있다. 

**~~Mendix 에서도 Association 을 만들 수 있는 것처럼(?)~~** 

예를 들어, 사용자 테이블은 **부모 테이블**이 되고 게시글 테이블은 **자식 테이블**이 될 수 있다. 또한 테이블 관계는 항상 상대적이다. 게시글 테이블은 다시 댓글 테이블의 부모 테이블이 될 수 있다.

## INNER JOIN

아래는 사용자 테이블과 게시글 정보를 조합하는 쿼리문이다. 순서는 다음과 같이 `FROM` 절에 **결합 형태를 정의**해준다. 이때, `ON` 뒤에 두 테이블이 어떤 컬럼을 공유하는지를 보여준다.

```sql
SELECT * FROM MEMBER INNER JOIN NOTICE ON MEMBER.ID = NOTICE.WRITER_ID;
```

이러한 **INNER JOIN** 은 자식이 없는 행(관계가 없는 것) 은 격자로 만들지 않는다. 이는 ANSI 에서 사용하는 표준 정의이다.

## OUTER JOIN

그렇다면 관계가 없는 것들은 어떻게 합칠 수 있을까. **OUTER JOIN** 을 사용한다. **OUTER JOIN** 이란 **참조키를 기준으로 일치하지 않는 행도 포함시키는 조인**이다.

ORACLE 에서 제공하는 OUTER JOIN 방법은 아래와 같다.

- LEFT OUTER JOIN
- RIGHT OUTER JOIN
- FULL OUTER JOIN

OUTER 란 연결 관계가 없는 행을 뜻한다. 즉, 양 쪽에 한 그룹씩 OUTER 가 생길 수 있다.

JOIN 에 참여하는 두 테이블은 SQL 문에 적힌 순서대로 LEFT, RIGHT 를 부여받는다. 그리고 JOIN 시 명시된 방향에 있는 TABLE 의 OUTER 들이 테이블 형성에 참여하게 된다. 결과적으로 비어있는 컬럼을 NULL 값으로 받게 된다.

보통 INNER JOIN 이 안정적일 것 같다. 그러나 실무에서는 기준 테이블을 정하고 LEFT/RIGHT OUTTER JOIN 수행하는 것이 오히려 누락되는 값 없이 데이터를 다룰 수 있다고 한다.

## 연습

일단 여기까지 이해하면 [이 문제](https://programmers.co.kr/learn/courses/30/lessons/59045)는 5분 정도 걸리면 풀 수 있다. 자세한 내용은 YouTube 에 `NEWLEC` 검색해서 꼭 들어보길. 진짜 나만 알고싶은 강의다.

내 풀이는 아래와 같다.

```sql
-- MySQL 정답
SELECT I.ANIMAL_ID, I.ANIMAL_TYPE, I.NAME
FROM ANIMAL_INS I INNER JOIN ANIMAL_OUTS O ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE
I.SEX_UPON_INTAKE LIKE "Intact%"
AND
(O.SEX_UPON_OUTCOME LIKE "Spayed%" OR O.SEX_UPON_OUTCOME LIKE "Neutered%")
ORDER BY ANIMAL_ID;
```

```sql
-- Oracle 정답
SELECT I.ANIMAL_ID, I.ANIMAL_TYPE, I.NAME
FROM ANIMAL_INS I INNER JOIN ANIMAL_OUTS O ON I.ANIMAL_ID = O.ANIMAL_ID
WHERE
I.SEX_UPON_INTAKE LIKE 'Intact%'
AND
(O.SEX_UPON_OUTCOME LIKE 'Spayed%' OR O.SEX_UPON_OUTCOME LIKE 'Neutered%')
ORDER BY ANIMAL_ID;
```

## SELF JOIN

개념상 존재하는 조인(JOIN)이다. 자기 스스로 테이블 조인하는 경우에 해당된다. COLUMN 중 **ADMINISTRATOR_ID** 라는 값이 있다고 가정하자. 자신의 관리자를 각각 저장하고 있다면 SELF JOIN 을 사용해 특정 데이터의 상사나 부서원 전부를 출력할 수 있다.

이때, 같은 테이블 이름을 두 번 사용하므로 별칭을 꼭 붙여줘야 한다. 그리고 최종 관리자가 나타날 때까지 계속해서 테이블을 작성하기 위해 반복문을 사용할 수도 있을 것이다. 이 방법은 나중에 배울 예정.

## CROSS JOIN

A TABLE 행 개수가 **N 개**, B TABLE 행 개수가 **M 개**라면 **총 N * M 개 행**을 만드는 JOIN.

```sql
SELECT N.*, M.NAME WRITER_NAME
FROM NOTICE N CROSS JOIN MEMBER M;
```