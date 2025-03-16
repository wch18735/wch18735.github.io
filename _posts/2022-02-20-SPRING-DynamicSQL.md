---
title: "[Spring] Dynamic SQL"
excerpt: "dynamic sql"
date: 2022-02-20
layout: single
classes: wide
category:
    - spring
tag:
    - spring
    - aop
    - aspectj
    - jdbc
author: 1FeS
comments: true
---

# MyBatis 동적 SQL

검색 조건에 따라 검색해야하는 SQL문이 달라지기 대문에 이를 처리하기 위해 동적 SQL이 사용된다. MyBatis의 표현식은 `if`, `trim(where, set)`, `choose(when, otherwise)`, `foreach` 가 있다. 이번 포스트에서 예제를 하나씩 정리할 예정이다.

동적 SQL 작성 시 유의사항으로 모든 케이스를 테스트 해보는 검증 과정을 가져야 한다는 것이 있다. MyBatis 구문을 이용해 SQL문이 실행 시에 개발자가 생각하지 못했던 방향으로 SQL문이 작성될 수 있다. 따라서 동적 SQL문이 아닌 일반적인 SQL문 상태에서 실행을 확인한 후, 이들을 동적 SQL문으로 관리하는 것이 올바르다고 한다.

## if

전달된 파라미터 값에 대한 조건검사를 수행한 후 구문을 쿼리에 포함할지 여부를 결정한다. 아래는 title 에 값이 존재할 경우 `AND title like ${title}` 구문을 본 쿼리에 포함시키는 예시다.

또한 파라미터 타입 안에 다른 타입(클래스)가 포함되어 캡슐화를 이룰 경우 `.(dot)` 연산자로 접근할 수 있음을 보여준다.

```xml
<select id="findActiveBlogLike" resultType="Blog"> 
    SELECT * FROM BLOG WHERE state = ‘ACTIVE’ 
    <if test="title != null"> 
        AND title like #{title} 
    </if> 
    <if test="author != null and author.name != null"> 
        AND author_name like #{author.name} 
    </if> 
</select>
```

## choose(when, otherwise)

`<if>` 구문은 조건의 참 여부에 따라 여러 개가 실행될 수 있으며, 단 하나도 실행되지 않을 수 있다. 반면 `choose ~ when ~ otherwise` 는 `<choose></choose>` 내부 여러 구문 중 한 개만 실행한다.

```xml
 <select id="findActiveBlogLike" resultType="Blog"> 
    SELECT * FROM BLOG WHERE state = ‘ACTIVE’ 
    <choose> 
        <when test="title != null"> 
            AND title like #{title} 
        </when> 
        <when test="author != null and author.name != null"> 
            AND author_name like #{author.name} 
        </when> 
        <otherwise> 
            AND featured = 1 
        </otherwise> 
    </choose> 
 </select>
```

조건 추가 순서는 먼저 적힌 것부터 진행된다. 만약 `title` 값이 없다면 `author` 값을 확인한다. 둘 다 값이 없다면 `otherwise`에 기재된 값을 따른다.

## trim(where, set)

`<where></where>` 는 내부에 컨텐츠가 존재할 때, where 키워드를 포함시킨다. 그렇지 않은 경우는 `where` 키워드를 사용하지 않는다.

이럴 경우 WHERE 다음에 곧바로 AND 가 붙는 경우가 생길 수 있다. 이를 방지하기 위해 `trim`을 사용한다. 

```xml
<select id="findActiveBlogLike" resultType="Blog">
		SELECT *
		FROM BLOG
		<trim prefix=“WHERE” prefixOverrids=“AND”>
			<if test="state != null">
				state = #{state}
			</if>
			<if test="title != null">
				title like #{title}
			</if>
			<if test="author != null and author.name != null">
				author_name like #{author.name}
			</if>
		</trim>
</select>
```

여기서 prefix 란 만약 `trim` 내부에 추가될 구문이 존재한다면 가장 앞에 `WHERE` 를 붙인다는 뜻이다. 그리고 prefixOverrids 란 추가적으로 붙는 구문들 사이에 자동으로 `AND | OR` 를 붙인다로 해석할 수 있다.

`<set></set>` 역시 내부에 추가할 값이 있으면 set 키워드를 포함시킨다. 이 때, 업데이트할 항목을 추가하기 위해 붙일 `,` 가 곧바로 붙는 경우가 생길 수 있다. 이를 방지하기 위해 아래와 같이 작성할 수 있다.

```xml
<update id="updateAuthorIfNecessary">
		update Author
		<trim prefix=“SET” suffixOverrids=“,”>
			<if test="state != null">
				state = #{state}
			</if>
			<if test="title != null">
				title = #{title}
			</if>
			<if test="author != null and author.name != null">
				author_name = #{author.name}
			</if>
		</trim>
</update>
```

## foreach

Dynamic Query 에서 공통적으로 필요한 기능은 Collection 반복 처리 기능이다. IN 키워드를 사용할 때 종종 사용되며 예시는 아래와 같다.

collection 속성은 List 일 경우 list, Array 일 경우 array 가 사용된다.

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
		SELECT * FROM POST P WHERE ID in
		<foreach item="item" index="index" collection="list" open="("
			separator="," close=")">
			#{item}
		</foreach>
</select>
```