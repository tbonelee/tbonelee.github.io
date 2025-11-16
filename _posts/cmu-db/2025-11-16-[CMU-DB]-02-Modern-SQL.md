---
layout: post
title: "[CMU-DB] 02. Modern SQL"
date: "2025-11-16 22:49:00 +0900"
categories:
  - Database
tags:
  - [CMU, Database]
---

[강의 영상 링크](https://www.youtube.com/watch?v=MzigBKf84aY&list=PLSE8ODhjZXjYDBpQnSymaectKjxCy6BYq&index=3)

# Relational Languages

- **Data Manipulatin Language (DML)** : `SELECT`, `INSERT`, `UPDATE`, `DELETE`
- **Data Definition Language (DDL)**
- **Data Control Language (DCL)** : 보안, 권한 제어 등

**sets(unordered, no duplicates)**에 기반한 Relational algebra와 달리, SQL은 **bags(unordered, allows duplicates)**에 기반

=> 중복 허용

# Aggregates

- Functions that return a single value from a bag of tuples.

- e.g. `AVG(col)`, `MIN(col)`, `MAX(col)`, `SUM(col)`, `COUNT(col)`

- `SELECT`아웃풋 문의 Non-aggregated 값 필드는 `GROUP BY` clause에 나와야 함.

잘못된 쿼리 :
```sql
SELECT AVG(s.gpa), e.cid
FROM enrolled AS e JOIN student AS s
ON e.sid = s.sid
```

# String Operations

- SQL 표준은 **case sensitive**, **single-quotes only**.
  - cf) MySQL 기본 모드는 cass insensitive
- **Pattern Matching**
  - `LIKE` 키워드는 조건식에서 문자열을 매칭할 때 사용된다.
    - "%"는 (공백을 포함한) 임의의 substring
    - "_"는 임의의 문자 한 개.
- SQL-92에 여러 string 함수가 있고, 여러 DB 시스템은 표준 함수와 각자의 함수를 구현해둠
- **Concatenation**: "\|\|"는 문자열을 이어줌.

# Output Redirection

- **새 테이블** : 쿼리 결과를 새 테이블에 저장

```sql
SELECT DISTINCT cid INTO CourseIds FROM enrolled;
```

- **기존 테이블** : 이미 존재하는 테이블에 쿼리 결과를 저장

```sql
INSERT INTO CourseIds (SELECT DISTINCT cid FROM enrolled);
```

# Output Control

`ORDER BY`, `LIMIT` 등으로 쿼리 결과물을 컨트롤 할 수 있음.

# Window Functions

연관된 튜플 set의 row들에 대해 sliding을 통한 계산을 수행.

aggregation과 달리 단일 row 결과로 압축되지 않음.

(모든 window function이 그런 건 아니지만) 개념적으로 window function은 다음 스텝으로 수행됨.

1. 테이블을 파티셔닝
2. 각 파티션을 정렬 (`ORDER BY`절이 있다면)
3. 각 record마다 여러 record에 걸친 window를 생성
4. 각 window에 대해 output 계산

**Functions**: 어떤 aggregation function도 사용 가능. 추가로 특별한 window function도 있음.

- `ROW_NUMBER`: 현재 row의 번호
- `RANK`: 현재 row의 <u>order</u> position

`ROW_NUMBER`는 유니크한 번호를 반환하고, `RANK`는 같은 값에 대해서 같은 번호를 반환

**Grouping**:

`OVER` 절은 window function을 계산할 때 어떻게 튜플을 그룹핑할지 명시해줌.

`PARTITION BY`로 그룹핑 기준을 명시할 수 있음.

```sql
SELECT cid, sid, ROW_NUMBER() OVER (PARTITION BY cid)
FROM enrolled ORDER BY cid;
```

`ORDER BY`를 넣어서 deterministic한 순서를 보장할 수도 있음.

```sql
SELECT *, ROW_NUMBER() OVER (ORDER BY cid)
FROM enrolled ORDER BY cid;
```

**주의**: 
- DBMS는 window function의 정렬 이후에 `RANK`를 계산한다. (`ROW_NUMBER`는 정렬 이전에 계산)
- SQL 쿼리에서 `WHERE` 절은 window function 계산 전에 처리되기 때문에, `WHERE` 절에서 window function 결과 필드를 사용할 수 없음.

```sql
SELECT * FROM (
  SELECT *, RANK() OVER (PARTITION BY cid
            ORDER BY grade ASC) AS rank
  FROM enrolled) AS ranking
WHERE ranking.rank = 2;
```

# Nested Queries

단일 쿼리에서 복잡한 로직을 실행하기 위해 쿼리 안에서 또 다른 쿼리를 호출하는 방식. 중첩 쿼리는 종종 최적화하기 어렵다.

Inner query는 outer query scope에 있는 속성에 접근 가능하다. 역은 성립하지 않는다.

Inner query는 웬만한 곳에 넣을 수 있다.

- `SELECT` Output Targets:

```sql
SELECT (SELECT 1) AS one FROM student;
```

- FROM Clause:

```sql
SELECT name
FROM student AS s, (SELECT sid FROM enrolled) AS e
WHERE s.sid = e.sid;
```

- WHERE Clause:

```sql
SELECT name FROM student
WHERE sid IN (SELECT sid FROM enrolled);
```

## Nested Query Results Expressions

- `ALL`: 모든 서브쿼리 row들에 대해 성립해야 함.
- `ANY`: 최소 한 개의 서브쿼리 row에 대해 성립해야 함.
- `IN`: `ANY()`와 동일.
- `EXISTS`: 최소 한 개의 row가 존재해야 함.

# Lateral Joins

`LATERAL` 오퍼레이터를 nested query에 붙이면 해당 쿼리 이전에 나온 newted 쿼리의 속성에 접근할 수 있음.

예시:

각 course에 등록한 학생의 수와 course별 평균 GPA를 계산하고, enrollment count에 대해 descending order로 정렬.

```sql
SELECT * FROM course AS c
  LATERAL (SELECT COUNT(*) AS cnt FROM enrolled
    WHERE enrolled.cid = c.cid) AS t1,
  LATERAL (SELECT AVG(gpa) AS avg FROM student AS s
    JOIN enrolled AS e ON s.sid = e.sid
    WHERE e.cid = c.cid) AS t2;
```
- 각 중첩 쿼리에서 `c.cid`에 접근하기 위해 `LATERAL` 필요

# Common Table Expressions (CTE)

단일 쿼리에 스코프가 제한되는 임시 테이블 같은 거로 생각할 수 있다.

`WITH` 절을 사용해 inner query의 결과를 임시 테이블로 바인딩할 수 있음.

```sql
WITH cteName AS (
  SELECT 1
)
SELECT * FROM cteName;
```

```sql
WITH cteName (col1, col2) AS (
  SELECT 1, 2
)
SELECT col1 + col2 FROM cteName;
```

```sql
WITH cte1 (col1) AS (SELECT 1), cte2 (col2) AS (SELECT 2)
SELECT * FROM cte1, cte2;
```

`RECURSIVE` 키워드를 `WITH`와 사용하면 CTE가 자기 자신을 참조하게 할 수 있음.
```sql
WITH RECURSIVE cteSource (counter) AS (
  (SELECT 1)
  UNION
  (SELECT counter + 1 FROM cteSource
    WHERE counter < 10)
)
SELECT * FROM cteSource;
```
