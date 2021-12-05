---
title: "[JDBC] SELECT 예제 (테이블의 모든 행을 조회하는 방법)"
excerpt: "select all records"
date: 2021-11-21
layout: single
classes: wide
category:
 - jdbc
tag:
 - java
 - jdbc
toc: true
toc_label: "Contents"
toc_icon: "heart"
author: 1FeS
comments: true
---

결과 집합을 나타내는 `rs` 객체는 테이블의 한 행을 가리키는 커서를 가진다. `rs.next()` 를 호출하면 해당 커서가 다음 행을 가리키고, 끝에 다다르면 더 이상 조회 가능한 행이 없다는 뜻의 False 를 반환한다.

아래는 `rs.next()` 가 더 이상 가져올 행이 없을 때 **False** 를 반환하는 점을 이용해 테이블 내 모든 데이터를 조회하는 코드다.

```java
/*
IMPORT PACKAGE AND LIBRARY
*/

public class Program {
	public static void main(String[] args) throws ClassNotFoundException, SQLException {
		
		/* BUILD CONNECTION TO DATABASE*/
		
		while(rs.next()) {
			String titleString = rs.getString("ID");
			System.out.println(titleString);
		}
		
		/* CLOSE CONNECTION */
	}
}
```

이걸 응용하면 `조회수가 N 이상`이거나 `M 번째 이후 행`만 조회하는 코드도 작성할 수 있다. 그러나 이런 방법은 권장되지 않는다.

데이터를 다루는 작업은 간단하게 아래 3가지 유형으로 나눠보자. 꼭 테이블 내부 모든 데이터를 불러와 아래 작업을 수행해야 할까?

- **Filtering**
- **Grouping**
- **Sorting** 

그렇지 않다. 세 가지 데이터 관련 작업은 모두 Oracle 에서 수행할 수 있다. `WHERE`, `GROUP BY`, `ORDER BY` 를 잘 사용하면 굳이 모든 데이터를 불러올 필요가 없다. 

만약 10억 개의 레코드를 불러와 데이터를 처리하는 프로그램이 있다고 가정하자. 분명 굉장히 많은 수행시간을 요구할 것이다. 따라서 자바는 UI 레이아웃을 신경쓰도록 하자. 

