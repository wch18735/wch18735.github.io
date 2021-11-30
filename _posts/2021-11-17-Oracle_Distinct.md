---
title: "[Oracle] ROWNUM 과 DISTINCT 키워드"
excerpt: "rownum in oracle database"
date: 2021-11-17
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

## 중복 값 제거 DISTINCT

`DISTINCT`는 레코드에 존재하는 중복값을 없애는 역할을 한다. `DISTINCT` 에 인자로 주어지는 컬럼에 SET 연산을 취하는 것과 같은 효과다. 

활용법은 아래와 같다.

```sql
SELECT DISTINCT(AGE) "MEMBER AGE" FROM MEMBER;
```

예시를 보면 더 쉽게 이해할 수 있다. *sqldeveloper* 에서 따라할 분들은 아래 더미 데이터와 테이블 생성 코드를 참고하자.

```sql
/* 테이블 생성 */
CREATE TABLE TEST_TABLE
(
"ID" NUMBER,
"NAME" VARCHAR2(10),
COLOR VARCHAR2(10)
);

/* 테스트 데이터 */
INSERT INTO TEST_TABLE VALUES (1, 'JACK', 'BLUE');
INSERT INTO TEST_TABLE VALUES (2, 'CINDY', 'RED');
INSERT INTO TEST_TABLE VALUES (3, 'JIMMY', 'YELLOW');
INSERT INTO TEST_TABLE VALUES (2, 'DONGSEOK', 'GREEN');
INSERT INTO TEST_TABLE VALUES (4, 'MING', 'RED');
INSERT INTO TEST_TABLE VALUES (2, 'TIM', 'BLUE');
INSERT INTO TEST_TABLE VALUES (3, 'TIM', 'BLUE');
```

위 테이블을 조회하면 다음과 같은 결과가 나온다. 모든 컬럼을 조회하기 위해 `SELECT * FROM TEST_TABLE` 을 쳐보자.

```sh
1	JACK	BLUE
3	JIMMY	YELLOW
2	DONGSEOK	GREEN
4	MING	RED
2	CINDY	RED
2	TIM	BLUE
3   TIM BLUE
```

여기서 어떤 색들만 존재하는지 알기 위해 사용하는 키워드가 `DISTINCT` 이다. 사용법은 아래와 같이 간단하다. 키워드 안에 *SET* 연산을 적용하고 싶은 컬럼을 집어 넣으면 된다.

```sql
SELECT DISTINCT(COLOR) FROM TEST_TABLE;
```
```sh
RED
BLUE
YELLOW
GREEN
```

이렇게 어떤 색들이 있는지 나타났다. `RED`와 `BLUE`는 각각 2개씩 값을 가지고 있다. 여기서 `DISTINCT(COLOR)` 옆에 `"NAME"`을 적으면 어떻게 될까. 중복된 색을 가진 두 명의 이름 중 어떤 값이 나타날까? 혹시 에러?

```sql
SELECT DISTINCT(COLOR), "NAME" FROM TEST_TABLE;
```
```sh
BLUE	JACK
YELLOW	JIMMY
RED	MING
RED	CINDY
GREEN	DONGSEOK
BLUE	TIM
```

정답은 중복된 행을 합치고, 모든 행을 출력한다. 위와 어떤 것이 달라졌는지 모르겠지만 *BLUE* 색을 좋아하는 두 명의 *TIM* 이 하나로 합쳐졌다. 이렇게 동작하는 원리는 무엇일까. 아래는 우리가 흔히 알고있는 SQL 실행 순서다. 

<br/>
<img src="/_img/2021-11-17/execute_order.jpg" style="margin: atuo auto; display: block; height: 10em;">
<br/>

여기서 `SELECT`와 `ORDER BY` 사이에 `DISTINCT` 가 들어가는데, 모든 행이 선택된 상태에서 `COLOR` 열을 기준으로 중복된 행들을 취합한다. 이 때, *중복된 행* 에 초점을 맞춰야 한다. 

비록 *ID* 값은 달라도 결과 테이블 상에서 두 명의 *TIM* 은 모두 *BLUE* 색을 좋아하고, 이 중복된 값이 하나로 합쳐진 것이다. 그럼 DISTINCT 키워드를 색이 아닌 이름에 걸어도 똑같은 결과가 나와야 하는 것 아닌가?

맞다.

똑같은 결과가 나온다.

이런 여러 실험을 시도할 수 있도록 아래 짤막한 문장들을 남긴다.

```sql
/* 전체 조회 */
SELECT * FROM TEST_TABLE;

/* DISTINCT 키워드 활용 */
SELECT DISTINCT(ID), "NAME" FROM TEST_TABLE;
SELECT DISTINCT(COLOR) FROM TEST_TABLE;
SELECT DISTINCT(COLOR), "NAME" FROM TEST_TABLE;
SELECT DISTINCT("NAME"), COLOR FROM TEST_TABLE;
SELECT COLOR, COUNT("ID") FROM TEST_TABLE GROUP BY "COLOR";
```