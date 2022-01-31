---
layout: default
title: NULL값 처리하기
last_modified_date: 2021-01-21 00:01:06
parent: MySQL
grand_parent: Database
---

# NULL값 처리하기

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

데이터베이스의 데이터 중에서 값이 없는 경우 보통 NULL 값으로 채워진다. 하지만 NULL 값은 결측치이기 때문에 데이터를 검색할 때 NULL 값이 없는 데이터만 조회하거나 NULL 값을 다른 값으로 치환하는 경우 등이 생긴다.

# IS NULL / IS NOT NULL

데이터 중 NULL 값이 없는 데이터만 보고 싶거나 NULL 값의 데이터만 보고싶을 때 사용한다.

너무 간단하기 때문에 설명보다는 프로그래머스 SQL 문제로 대신한다.

## IS NULL

> ANIMAL_INS 테이블에서 ANIMAL_ID를 조회하고자 한다. 단, NAME이 없는 데이터만 조회한다.

```sql
SELECT ANIMAL_ID FROM ANIMAL_INS WHERE NAME IS NULL;
```

## IS NOT NULL

> ANIMAL_INS 테이블에서 ANIMAL_ID를 조회하고자 한다. 단, NAME이 없는 데이터는 제외한다.

```sql
SELECT ANIMAL_ID FROM ANIMAL_INS WHERE NAME IS NOT NULL;
```

# IFNULL

IFNULL은 해당 칼럼의 데이터에서 NULL 값이 있다면 그 값을 원하는 값으로 치환할 때 사용한다.

```sql
SELECT IFNULL(칼럼명, 치환할_값) FROM 테이블명;
```

> ANIMAL_INS 테이블에서 ANIMAL_TYPE, NAME, SEX_UPON_INTAKE를 ANIMAL_ID순으로 조회하고자 한다. 이때 NAME이 없는 데이터는 NULL 대신 No name으로 표시한다.

```sql
SELECT ANIMAL_TYPE, IFNULL(NAME, "No name"), SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY ANIMAL_ID;
```

# Ref.

- [IS NULL 문제](https://programmers.co.kr/learn/courses/30/lessons/59039)
- [IS NOT NULL 문제](https://programmers.co.kr/learn/courses/30/lessons/59407)
- [IFNULL 문제](https://programmers.co.kr/learn/courses/30/lessons/59410)
