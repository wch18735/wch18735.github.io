---
title: "[Oracle] 서브 쿼리란"
excerpt: "sub-query"
date: 2021-11-20
category:
    - oracle
tag:
    - database
    - oracle
    - sql
    - subquery
author: 1FeS
toc: true
toc_label: "Contents"
toc_icon: "heart"
---

# 서브 쿼리 (Sub-Query)

만약 `1) 정렬한 다음`` 2) 그 중 상위 10개` 를 뽑는 쿼리를 작성해보자. 아래와 같이 요구사항을 그대로 따르는 질의문을 작성하고 실행하면 오류가 발생한다.

```sql
SELECT * FROM MEMBER
ORDER BY REGDATE                -- 주어진 테이블을 정렬
WHERE ROWNUM BETWEEN 1 AND 10;  -- 정렬된 테이블에서 상위 10개
```

위 쿼리는 순서를 지키지 않았다. 그러나 이처럼 명령어 순서가 논리적으로 바뀌어야 하는 경우 사용할 수 있는 것이 **서브 쿼리** 이다.

```sql
SELECT * FROM (SELECT * FROM MEMBER ORDER BY REGDATE)
WHERE ROWNUM BETWEEN  1 AND 10;
```

소괄호 `()` 로 둘러싸인 쿼리를 서브 쿼리라고 한다. 수식에서도 **먼저** 계산하는 것을 소괄호로 묶는 것처럼 쿼리 문장 안에서도 먼저 만들 격자형 데이터에 소괄호를 묶어주면 된다.


## 활용 예시: ROWNUM 이용시 주의사항

ROWNUM이 1부터 5까지인 테이블을 뽑아보려고 한다.

```sql
SELECT * FROM MEMBER WHERE ROWNUM BETWEEN 1 AND 5;
-- 정상 동작
```

위 쿼리문을 응용해 ROWNUM이 6부터 10까지인 테이블을 뽑는 문장을 작성해보면 대부분 처음은 아래와 같이 작성한다.

```sql
SELECT * FROM MEMBER WHERE ROWNUM BETWEEN 5 AND 10;
-- RESULT: 없음
```

결과는 아무것도 나타나지 않는다. 왜?

이유는 WHERE 절 동작 원리와 ROWNUM 부여 방식에 있다. 
- `WHERE절`은 한 행씩 건너며 해당 행이 조건을 만족하는지 검사한다. 
- 첫 번째 행의 ROWNUM은 1이고 조건에 부합하지 않기 때문에 결과 테이블에 포함되지 않는다.
- 자연스럽게 그 다음 행은 앞에 아무 행도 존재하지 않기 때문에 ROWNUM 1번을 부여받는다.
- 역시 조건에 부합하지 않기 때문에 결과 테이블에 포함되지 않는다. 
- 이 과정이 모든 레코드에 동일하게 동작해 결과적으로 결과 테이블에 아무것도 나타나지 않는다.

이 문제를 해결하기 위해 활용하는 방식이 서브쿼리이다. 모든 테이블을 한 번 조회하며 NUM COLUMN을 만든 결과 테이블을 다시 한 번 조회하는 방법이다.

```sql
SELECT * FROM (SELECT ROWNUM NUM, MEMBER.* FROM MEMBER) WHERE NUM BETWEEN 6 AND 10;
```

