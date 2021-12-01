---
title: "[Oracle] 집합 연산자 사용법과 VIEW"
excerpt: "oracle union, minus, intersect, union all operator"
date: 2021-11-20
category:
    - oracle
tag:
    - database
    - oracle
    - sql
    - view
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# OPERATOR LIKE SET

Oracle 테이블을 집합이라고 생각했을 때, 레코드를 합치는 방법이 있다. Join 과 달리 컬럼명이 동일하지 않아도 컬럼 개수만 맞다면 합칠 수 있다.

UNION 연산자들은 다음과 같다.

- UNION : 같은 행은 중복을 없앤다
- MINUS : 좌측 테이블과  우측 테이블의 같은 부분을 제외한 좌측 테이블을 나타낸다
- INTERSECT : 같은 부분만 나타낸다
- UNION ALL : 같은 행도 중복을 허용한다

이 연산자들은 아래 예제 SQL문의 `UNION` 자리에 삽입되어 사용된다. 결과는 예상하는대로 나타날 것이다. 두 테이블만 아니라 계속해서 그 아래 연산자를 추가하는 것도 가능하다.

```sql
SELECT ID, NAME FROM MEMBER
UNION -- MINUS, INTERSECT, UNION ALL
SELECT WRITER_ID, NAME FROM NITICE
-- UNION SELECT WRITER_ID, NAME FROM COMMENT (중복으로 또 쓸 수 있음)
```

## VIEW

위 과정에서 생성한 TABLE 을 여러 번 사용해야 할 때마다 반복된 쿼리를 수행하는 것은 가독성을 떨어트린다.

생성된 테이블을 하나로 관리할 수 있는 **VIEW** 라는 것이 존재한다.

아래와 같이 VIEW 를 만들면 이후에도 VIEW 이름만 명시하여 사용할 수 있다.

```sql
CREATE VIEW CUSTOM_VIEW
AS
-- 복잡한 쿼리 테이블
```

이러한 VIEW 는 사용자들에게 **읽기 모드**로 제공된다. 

따라서 특정한 부분만 공개하고 보안적 요소를 보장하는 역할로도 사용된다. 

ENCAPSULATION 개념과 비슷하다.

관련 내용으로 `DATA DICTIONARY` 가 있다.

데이터 딕셔너리는 ORACLE DBMS 관련된 데이터 베이스를 직접 보여주는 것 대신 VIEW 들을 제공한다.

이때, 뷰 테이블은 `DBA_`, `ALL_`, `USER_` + `테이블 명` 과 같은 Convention 을 가진다.

과거 Console 기반 개발자들은 이 정보들을 뒤져가면서 개발했다고 한다.

따봉...