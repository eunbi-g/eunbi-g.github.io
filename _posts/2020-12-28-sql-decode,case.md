---
title: SQL 조건문 (DECODE, CASE)
date: 2020-12-28
categories:
- SQL
tags:
- sql
- decode
- case
- 조건문
---


decode, case 함수를 이용해 sql 안에서 조건문을 실행할 수 있다.

- decode 함수는 대소비교(<,>,=)가 불가능하다. case 함수는 가능.




## 1. DECODE


```sql
DECODE(A, B, '1', '2')  # A=B이면 1을, 아니면 2를 출력 (2 대신 null 사용 가능)
DECODE(A, B, '1', C, '2', '3')  # A=B이면 1을, A=C이면 2를, 모두 아니면 3를 출력
DECODE(A, B, '1', '2')  # A=B이면 1을, 아니면 2를 출력
DECODE(A, B, DECODE(C, D, '1', '2'))  #A=B이고, C=D이면 1을, C!=D이면 2를 출력
```



## 2. CASE ~ WHEN ~ THEN

기본형

```sql
case
when A=1 then 'a'
when A=2 then 'b'
when A=3 then 'c'
else 'd'
end
```

축약형

```sql
case A
when 1 then 'a'
when 2 then 'b'
when 3 then 'c'
else 'd'
end
```



## 3. DECODE, CASE 이중 사용

decode 함수를 중복해서 사용하면 성능상 좋지 않다.
이중 사용을 해야한다면 case를 사용하자.


decode 이중 사용

```sql
decode(A, 1, decode(B, 1, 'a', 'b'), 3, 'c', 'd')
```


```sql
case
when A=1 and B=1 then 'a'
when A=1 and B=2 then 'b'
when A=3 then 'c'
else 'd'
end
```
