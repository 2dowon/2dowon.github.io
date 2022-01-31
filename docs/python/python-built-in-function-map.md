---
layout: default
title: map()
last_modified_date: 2020-12-02 00:12:30
parent: Python
---

# map()

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

# map()

`map()` 은 데이터를 지정된 함수로 처리해주는 파이썬 내장 함수이다. 여러 개의 데이터를 담고 있는 list나 tuple을 대상으로 모든 요소를 숫자 또는 문자 등 다른 자료형으로 형 변환하기 위해 사용한다.

## map()의 기본 구조

```python
map(변환 함수, 순회 가능한 데이터)
```

map 만 사용해 출력하면 아래 코드처럼 맵 객체가 만들어져 그 주소값이 출력된다. 따라서 맵 객체 안에 있는 내용을 보기 위해서는 list를 사용해야 한다.

```python
a = [1.2, 2.5, 3.7, 4.6]
a = map(int, a)
print(a)

# <map object at 0x7fc7c673b2e0>
```

### map() 사용 예제

- 실수형을 정수형으로 변환

  ```python
  a = [1.2, 2.5, 3.7, 4.6]
  a = list(map(int, a))
  print(a)

  # [1, 2, 3, 4]
  ```

- 숫자를 문자열로 변환

  ```python
  a = list(map(str, range(10)))
  print(a)

  # ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9']
  ```

- 리스트 요소에 3씩 곱하기

  ```python
  a = [3,4,5,2,4,3,5,13,91]

  def mul(n):
      n *=3
      return n

  print(list(map(mul,a)))

  # [9, 12, 15, 6, 12, 9, 15, 39, 273]
  ```

## map() 응용하기

### input().split()

`input()` 을 이용하면 유저의 입력을 받을 수 있다. `input().split()` 을 이용하면 공백을 기준으로 유저의 입력을 여러 개 받을 수 있다. 예를 들어, 유저가 '10 20'이라고 입력을 하면 가운데 공백을 기준으로 ["10", "20"]이라는 결과를 얻을 수 있다.

```python
a = input().split()
# 10 20 (입력)

print(a)
# ['10', '20']
```

지금 결과를 보는 것처럼 유저가 입력 값은 숫자일지라도 **input으로 받은 값은 무조건 String 문자열** 로 받게 되는데, 이때 숫자로 형변환을 쉽게 할 수 있는 방법이 바로 `map()` 을 이용하는 방법이다.

```python
a = list(map(int, input().split()))
# 10 20 (입력)

print(a)
# [10, 20]
```

### sum()

`input().split()`과 `map()` 의 조합에 `sum()` 을 이용하면 유저가 입력한 값들을 모두 한 번에 더할 수 있다.

```python
a = sum(map(int, input().split()))
# 1 2 3 (입력)

print(a)
# 6
```
