---
layout: default
title: DECLARE / SET @var_name
last_modified_date: 2021-01-16 23:01:16
parent: MySQL
grand_parent: Database
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">DECLARE / SET @var_name</div>

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

프로그래머스에서 SQL - GROUP BY 파트의 [입양 시각 구하기(2)](https://programmers.co.kr/learn/courses/30/lessons/59413) 문제를 푸는데, SET을 이용해 변수를 선언해서 해당 변수를 쿼리문 안에서 활용하는 문제였다. SQL을 공부하면서 SQL 변수에 대해서는 공부했던 적이 없는 것 같아, 한 번 정리해보려고 한다.

# SQL 지역변수

쿼리에서 변수를 사용하고 싶다면 DECLARE를 이용해 변수를 선언하고, SET을 이용해 변수에 값을 대입하면 된다. 이 때 값 뿐만 아니라 여러 행으로 떨어지는 결과를 담을 수 있어서 '테이블 변수'도 만들 수 있다. 변수의 값을 출력할 때는 SELECT를 이용하면 된다. 변수는 프로시저와 함께 쓰면 유용하다.

### SQL 지역 변수를 사용하는 경우

- 루프가 수행되는 횟수를 세거나 제어하는 카운터로 사용
- 흐름 제어문에서 테스트되는 데이터 값을 보유할 때 사용
- 저장 프로시저 반환 코드 또는 함수 반환 값으로 반환되는 데이터 값을 저장할 때 사용

## DECLARE

DECLARE를 이용하면 변수의 타입을 설정할 수 있다. 그래서 보통 SQL에서 변수를 사용할 때는 DECLARE로 변수의 이름과 타입을 설정 한 후, SET을 이용해 그 값을 대입한다.

SQL에서 변수를 선언할 때는 변수명 앞에 @를 붙여야 한다.
@가 붙은 변수는 프로시저가 종료되어도 유지된다고 생각하면 된다.

```sql
DECLARE @var_name INT;
DECLARE @var_name varchar(20);
```

## SET

DECLARE로 변수의 타입을 설정하지 않고, 바로 SET을 이용해 한 번에 변수를 선언하고 초기화할 수도 있다. 일반적으로는 `=` 을 이용해서 변수에 값을 대입한다. 이 경우에는 SELECT문과 따로 선언하는 것이므로 쿼리를 각각 실행해야 한다.

```sql
SET @var_name = expr [, @var_name = expr] ;
```

`:=` 을 이용하면 SELECT문 안에서 선언할 수 도 있다. 하지만 이 경우 정확성을 보장할 수 없어 [MySQL 매뉴얼](https://dev.mysql.com/doc/refman/8.0/en/user-variables.html)에서는 이를 권장하지 않는다.

```sql
SELECT (@var_name := expr), * FROM table_name ;
```

# 프로그래머스 - 입양 시각 구하기(2)

이 문제가 바로 SQL에서 변수를 선언하는 것을 공부하게 된 문제이다. 간단하게 이 문제를 설명하면 동물 보호소에서 입양 보낸 동물의 정보를 담은 테이블을 이용해 보호소에서 몇 시에 입양이 가장 활발하게 일어나는지를 알아보려고 한다. 0시부터 23시까지, 각 시간대별로 입양이 몇 건이나 발생했는지 조회하는 SQL문을 작성해야 한다.

처음에는 하루의 모든 시간대인 0시에서 23시까지 구해야 하므로 WHERE ~ BETWEEN을 이용하면 되지 않을까해서 아래와 같은 코드를 작성했다. 하지만 이 경우, 0시부터 6시 그리고 20시부터 23시의 경우 데이터가 아예 없어서 이 부분은 출력되지 않는 문제가 발생했다.

```sql
SELECT HOUR(DATETIME), COUNT(*)
FROM ANIMAL_OUTS
WHERE HOUR(DATETIME) BETWEEN 0 AND 23
GROUP BY HOUR(DATETIME)
ORDER BY HOUR(DATETIME);
```

그래서 이 문제를 풀기 위해 구글링을 하는 도중에 아래와 같은 풀이를 알게 되었고, SET 문법을 알게 되었다. 일단 먼저 아래와 같은 풀이를 이용하면 문제에서 원하는 값을 구할 수 있다. SET을 이용해 hour를 변수로 선언한 다음 변수 hour를 이용해서 0시부터 23시까지 루프를 돌려 매 시각마다 입양 건수를 체크하는 것이다.

```sql
SET @hour = -1; -- 변수 선언 및 초기화

SELECT (@hour := @hour + 1) as HOUR,
(SELECT COUNT(*) FROM ANIMAL_OUTS WHERE HOUR(DATETIME) = @hour) as COUNT
FROM ANIMAL_OUTS
WHERE @hour < 23
```

# Ref.

- [코딩테스트 연습 - 입양 시각 구하기(2)](https://programmers.co.kr/learn/courses/30/lessons/59413)

- [[MySQL] 날짜 데이터에서 일부만 추출하기](https://extbrain.tistory.com/60)

- [[프로그래머스] 입양 시각 구하기(1), (2) (GROUP BY, HAVING, SET)](https://chanhuiseok.github.io/posts/db-6/)

- [변수(Transact-SQL) - SQL Server](https://docs.microsoft.com/ko-kr/sql/t-sql/language-elements/variables-transact-sql?view=sql-server-ver15)
