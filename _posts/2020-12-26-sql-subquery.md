---
title: SQL 서브쿼리 (WHERE/FROM/SELECT)
date: 2020-12-26
categories:
- SQL
---

## 서브쿼리 (Subquery)

> 쿼리 안의 쿼리. WHERE절 / FROM절 / SELECT절 안의 쿼리를 통칭하여 서브쿼리라고 한다.

- 서브쿼리는 메인쿼리가 실행되기 이전에 한 번만 실행된다.
- 서브쿼리의 장점: 한 번 디스크에서 읽어온 데이터를 메모리 안에서 가공, 물리적인 I/O를 줄여준다.
- 서브쿼리에는 ORDER BY절이 포함될 수 없다.


# 1. WHERE절 서브쿼리

가장 자주 쓰이고, 중첩서브쿼리(nested subqueries)라고 불린다.

다중 행 서브쿼리 (in, all, any, some, exists)
```sql
select *
from student A
where A.name in (select B.name
					from subject B
					where B.name = 'MATH')
;
```

B 테이블에서 'MATH'를 선택한 학생들의 모든 정보를 A 테이블에서 조회
in 대신 = 연산자를 사용하면 단일 행 서브쿼리가 된다.


다중 칼럼 서브쿼리
서브쿼리의 결과로 여러 개의 칼럼이 반환된다.

```sql
select class, name, test
from student
where (class, test) in (select class, max(test)
							from subject
							group by class)
;
```

각 반의 최고 점수를 받은 학생의 정보를 student 테이블에서 조회


# 2. FROM절 서브쿼리

인라인뷰(inline views) 라고 불린다.

```sql
select A.item_name, sub.total_amt
from suppliers A, (select id, sum(B.amount) as total_amt
					from orders B
					group by id) sub
where A.id = sub.id
;
```

원하는 정보가 A, B 다른 테이블에 들어있는 경우, 동일한 id 컬럼을 이용해 원하는 데이터를 조회


# 3. SELECT절 서브쿼리

스칼라 서브쿼리(scalar subqueries) 라고 불린다.
반드시 단일 값을 리턴해야 하며, SUM, COUNT, MIN, MAX 등의 집계 함수가 자주 쓰인다.

```SQL
select name, price, round(
							(select avg(price)
							from products p1
							where p1.id = p2.id), 2) avg_price
from products p2
group by name
;
```

