---
layout: default
title: List Comprehension
last_modified_date: 2020-12-02 14:12:83
parent: Python
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">List Comprehension</div>

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

# List Comprehesion

새로운 리스트를 만들 때 사용하는 간단한 표현식

## List Comprehesion의 기본 구조

```python
[<반복 실행문> for <반복 변수> in <반복 범위>]
```

**실행 순서**

1. for <반복 변수> in <반복 범위>
2. <반복 실행문>

❗ 콜론`:`을 이용하지 않음

### List Comprehesion 예제

- 1~5까지 숫자가 들어있는 리스트에서 각 항목의 숫자를 제곱하는 코드

  ```python
  numbers = [1,2,3,4,5]
  square = []

  for i in numbers:
      square.append( i ** 2)

  print(square)

  # [1, 4, 9, 16, 25]
  ```

- 위 코드에서 리스트 컴프리헨션 방법을 사용한 코드

  ```python
  numbers = [1,2,3,4,5]
  square = [i ** 2 for i in numbers]
  print(square)

  # [1, 4, 9, 16, 25]
  ```

## 조건문을 포함한 List Comprehesion

```python
[<반복 실행문> for <반복 변수> in <반복 범위> **if <조건문>**]
```

**실행 순서**

1. for <반복 변수> in <반복 범위>
2. if <조건문>
3. <반복 실행문>

✔ 반복문을 수행하다가 if<조건문>을 만족하는 경우에만 <반복 실행문>을 실행한다

### 조건문을 포함한 List Comprehesion 예제

- 1~5까지 숫자가 들어있는 리스트에서 각 항목의 숫자가 3 이상일 때 제곱하는 코드

  ```python
  numbers = [1,2,3,4,5]
  square = []

  for i in numbers:
      if i >= 3:
          square.append( i ** 2)

  print(square)

  # [9, 16, 25]
  ```

- 위 코드에서 리스트 컴프리헨션 방법을 사용한 코드

  ```python
  numbers = [1,2,3,4,5]
  square = [i ** 2 for i in numbers if i>=3]
  print(square)

  # [9, 16, 25]
  ```

## 이중 리스트 컴프리헨션

프로그래머스 1단계 문제 중에 [[행렬의 덧셈](https://programmers.co.kr/learn/courses/30/lessons/12950)]이란 문제가 있는데, 거기서 이중 리스트 컴프리헨션을 사용해 문제를 풀었어야 했다. 개인적으로 리스트 컴프리헨션이 처음에 익숙하지 않아 항상 리스트 컴프리헨션 코드를 보면 원래 상태로 푸는 연습을 많이 했는데, 이중 리스트 컴프리헨션은 푸는데 시간이 꽤 오래 걸렸는데 정보가 많지 않아 공유 차원에서 적고자 한다.

### 이중 리스트 컴프리헨션으로 행렬의 덧셈 문제 풀이

- 이중 리스트 컴프리헨션

  ```python
  def solution(arr1, arr2):
      answer = [[c + d for c, d in zip(a, b)] for a, b in zip(arr1,arr2)]
      return answer

  arr1 = [[1,2],[2,3]]
  arr2 = [[3,4],[5,6]]
  solution(arr1,arr2)

  # [[4, 6], [7, 9]]
  ```

- 이중 리스트 컴프리헨션을 풀어서 쓴 코드

  ```python
  def solution(arr1, arr2):
      answer = [[],[]]

      i = 0
      for a, b in zip(arr1,arr2) :
          for c, d in zip(a, b) :
              answer[i].append(c+d)
          i = i + 1
      return answer

  arr1 = [[1,2],[2,3]]
  arr2 = [[3,4],[5,6]]
  solution(arr1,arr2)
  ```
