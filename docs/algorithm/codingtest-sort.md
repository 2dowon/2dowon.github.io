---
layout: default
title: 정렬(Sort)
date: 2021-01-20 01:01:17
parent: 이것이 취업을 위한 코딩테스트다 with 파이썬
grand_parent: Algorithm
permalink: /docs/algorithm/python-book/sort/
---

| [이것이 취업을 위한 코딩테스트다 with 파이썬](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)를 읽고 정리한 내용입니다.

# 정렬

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

정렬은 데이터를 특정한 기준에 따라서 순서대로 나열하는 것이다.

알고리즘 문제를 풀면서 제일 많이 사용한 것이 정렬이라고도 할 수 있을 정도인데, 항상 파이썬 라이브러리인 `sort()` 나 `sorted()` 함수를 사용했다. 물론 알고리즘 문제를 풀 때는 이미 잘 만들어진 라이브러리를 사용하는 것이 더 효과적인 경우가 많지만, 그래도 정렬 알고리즘에 대해 직접 이해하고 알고 있는 것도 중요하다고 생각한다. 밑에서도 얘기하겠지만, 데이터의 범위가 한정되어 있는 경우에는 정렬 라이브러리보다 계수 정렬을 사용하는 것이 더 효과적이고 빠르기 때문이다. 또 가끔 선택 정렬, 삽입 정렬, 퀵 정렬 등의 원리에 대해 물어보는 문제도 있다.

## 선택 정렬

- 정렬 알고리즘 중에서 가장 원시적인 방법으로 매번 **가장 작은 것을 선택**하는 알고리즘
- 데이터가 무작위로 여러 개 있을 때, 이 중에서 가장 작은 데이터를 선택해 맨 앞에 있는 데이터와 바꾸고, 그 다음 작은 데이터를 선택해 앞에서 두 번째 데이터와 바꾸는 과정을 반복하다보면 전체 데이터의 정렬이 된다
- 선택 정렬 동작 순서
  1. 예를 들어, 0부터 9까지 10개의 수가 무작위로 있다.
  2. 먼저 가장 작은 데이터인 0을 선택해서 맨 앞에 있는 데이터와 바꾼다.
  3. 첫 번째 데이터는 정렬이 되었으므로 이제 두 번째 데이터를 남은 데이터 중에서 가장 작은 데이터와 바꾼다.
  4. 이를 반복하다보면 전체 데이터가 정렬된다.
- 선택 정렬의 시간 복잡도 ⇒ $O(N^2)$
- 선택 정렬은 매우 비효율적이지만, 알고리즘 문제에서 가장 작은 데이터를 찾는 일이 자주 있으므로 익숙해면 좋다!

> 선택 정렬 소스코드

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(len(array)):
    min_index = i # 가장 작은 원소의 인덱스
    for j in range(i + 1, len(array)):
        if array[min_index] > array[j]:
            min_index = j
    array[i], array[min_index] = array[min_index], array[i] # 스와프

print(array)
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 삽입 정렬

- **데이터를 하나씩 확인하며, 각 데이터를 적절한 위치에 삽입하는 알고리즘**
- 삽입 정렬은 필요할 때만 위치를 바꾸기 때문에 선택 정렬에 비해 더 효율적이고, 특히 **데이터가 거의 정렬되어 있다면 훨씬 효율적**이다
- 삽입 정렬 동작 순서
  1. 예를 들어, 0부터 9까지 10개의 수가 무작위로 있다.
  2. 첫 번째 데이터는 그 자체로 정렬되어 있다고 판단하므로 두 번째 데이터의 위치를 판단한다.
  3. 두 번째 데이터가 첫 번째 데이터보다 작다면 첫 번째 데이터 앞에 삽입하고, 크다면 원래 자리 그대로 둔다.
  4. 그 다음 데이터로 위 과정을 반복하면 전체 데이터가 정렬된다.
- 특정한 데이터가 삽입될 위치를 선정하기 위해서 왼쪽으로 한 칸씩 이동하면서 찾고, 삽입될 데이터보다 작은 데이터를 만나면 그 위치에서 멈추면 된다.
- 삽입 정렬의 시간 복잡도 ⇒ $O(N^2)$
- 선택 정렬의 시간 복잡도와 같아 보이지만, 삽입 정렬의 경우 데이터가 거의 정렬되어 있다면 O(N)의 시간 복잡도처럼 매우 빠르게 동작한다. 거의 정렬되어 있을 때는 퀵 정렬보다도 효과적이다.

> 삽입 정렬 소스코드

```python
array = [7, 5, 9, 0, 3, 1, 6, 2, 4, 8]

for i in range(1, len(array)):
    for j in range(i, 0, -1): # 인덱스 i부터 1까지 1씩 감소하며 반복하는 문법
        if array[j] < array[j - 1]: # 한 칸씩 왼쪽으로 이동
            array[j], array[j - 1] = array[j - 1], array[j]
        else: # 자기보다 작은 데이터를 만나면 그 위치에서 멈춤
            break

print(array)
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 퀵 정렬

- 정렬 알고리즘 중에서 가장 많이 사용되는 알고리즘
- 퀵 정렬은 기준을 설정한 다음 큰 수와 작은 수를 교환한 후 리스트를 반으로 나누는 방식으로 동작한다
- 퀵 정렬은 피벗(Pivot)이 사용된다. 피벗은 큰 숫자와 작은 숫자를 교환할 때, 교환하기 위한 기준이다
- 퀵 정렬 동작 순서
  1. 예를 들어, 0부터 9까지 10개의 수가 무작위로 있다.
  2. 첫 번째 데이터를 피벗으로 정하고 왼쪽에서부터는 피벗보다 큰 데이터를 찾고, 오른쪽에서부터는 피벗보다 작은 데이터를 찾는다.
  3. 그 다음 큰 데이터와 작은 데이터의 위치를 서로 교환한다.
  4. 만약 왼쪽에서부터 찾는 값과 오른쪽에서부터 찾는 값의 위치가 서로 엇갈리면 작은 데이터와 피벗의 위치를 서로 변경한다.
  5. 그러면 피벗을 기준으로 왼쪽 리스트와 오른쪽 리스트로 분리된다.
  6. 이제 왼쪽 리스트와 오른쪽 리스트에서도 각각 피벗을 설정해서 동일한 방식으로 정렬을 하면 전체 리스트에 대해서 모두 정렬이 된다.
- 퀵 정렬의 평균 시간 복잡도 = $O(NlogN)$
- 하지만 최악의 경우 시간 복잡도 = $O(N^2)$
- 퀵 정렬은 데이터의 개수가 많을수록 선택 정렬, 삽입 정렬에 비해 압도적으로 빠르다. 하지만 이미 데이터가 정렬되어 있는 경우에는 삽입 정렬과 다르게 매우 느리게 동작한다.

> 퀵 정렬 소스코드

```python
array = [5, 7, 9, 0, 3, 1, 6, 2, 4, 8]

def quick_sort(array, start, end):
    if start >= end: # 원소가 1개인 경우 종료
        return
    pivot = start # 피벗은 첫 번째 원소
    left = start + 1
    right = end
    while(left <= right):
        # 피벗보다 큰 데이터를 찾을 때까지 반복
        while(left <= end and array[left] <= array[pivot]):
            left += 1
        # 피벗보다 작은 데이터를 찾을 때까지 반복
        while(right > start and array[right] >= array[pivot]):
            right -= 1
        if(left > right): # 엇갈렸다면 작은 데이터와 피벗을 교체
            array[right], array[pivot] = array[pivot], array[right]
        else: # 엇갈리지 않았다면 작은 데이터와 큰 데이터를 교체
            array[left], array[right] = array[right], array[left]
    # 분할 이후 왼쪽 부분과 오른쪽 부분에서 각각 정렬 수행
    quick_sort(array, start, right - 1)
    quick_sort(array, right + 1, end)

quick_sort(array, 0, len(array) - 1)
print(array)
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

> 파이썬의 장점을 살린 퀵 정렬 소스코드

```python
array = [5, 7, 9, 0, 3, 1, 6, 2, 4, 8]

def quick_sort(array):
    # 리스트가 하나 이하의 원소만을 담고 있다면 종료
    if len(array) <= 1:
        return array

    pivot = array[0] # 피벗은 첫 번째 원소
    tail = array[1:] # 피벗을 제외한 리스트

    left_side = [x for x in tail if x <= pivot] # 분할된 왼쪽 부분
    right_side = [x for x in tail if x > pivot] # 분할된 오른쪽 부분

    # 분할 이후 왼쪽 부분과 오른쪽 부분에서 각각 정렬을 수행하고, 전체 리스트를 반환
    return quick_sort(left_side) + [pivot] + quick_sort(right_side)

print(quick_sort(array))
# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 계수 정렬

- 특정한 조건이 부합할 때만 사용할 수 있는 매우 빠른 정렬 알고리즘
- 데이터의 크기 범위가 제한되어 정수 형태로 표현할 수 있을 때만 사용 가능
- 가장 작은 데이터와 가장 큰 데이터의 차이가 1,000,000을 넘지 않을 때 효과적으로 사용 가능
- 계수 정렬은 다른 정렬 알고리즘과 다르게 일반적으로 별도의 리스트를 선언하고, 그 안에 정렬에 대한 정보를 담는다는 특징이 있다
- 계수 정렬 동작 방식
  1. 예를 들어, 0부터 9까지 범위에서 수가 무작위로 20개 있다.
  2. 계수 정렬은 가장 큰 데이터와 가장 작은 데이터의 범위가 모두 담길 수 있도록 하나의 리스트를 생성한다. 이 경우에는 0부터 9까지이므로 크기가 10인 리스트를 선언하면 된다. 처음에는 리스트의 모든 데이터가 0이 되도록 초기화한다.
  3. 이제 정렬하고자 하는 데이터를 하나씩 확인하면서 데이터의 값과 동일한 인덱스의 데이터를 1씩 증가시킨다.
  4. 마지막으로 리스트의 첫 번째 데이터부터 하나씩 그 값만큼 인덱스를 출력하면 된다. 예를 들어, 0이 2번 등장했다면 0은 두 번 출력하면 된다.
- 계수 정렬의 시간 복잡도 = $O(N+K)$
- 데이터의 범위만 한정되어 있다면 효과적이고 항상 빠르게 동작하는 알고리즘! 하지만 데이터의 크기가 한정되어 있고, 데이터의 크기가 많이 중복되어 있을 수록 유리하며 항상 사용할 수는 없다.

> 계수 정렬 소스 코드

```python
# 모든 원소의 값이 0보다 크거나 같다고 가정
array = [7, 5, 9, 0, 3, 1, 6, 2, 9, 1, 4, 8, 0, 5, 2]
# 모든 범위를 포함하는 리스트 선언 (모든 값은 0으로 초기화)
count = [0] * (max(array) + 1)

for i in range(len(array)):
    count[array[i]] += 1 # 각 데이터에 해당하는 인덱스의 값 증가

for i in range(len(count)): # 리스트에 기록된 정렬 정보 확인
    for j in range(count[i]):
        print(i, end=' ') # 띄어쓰기를 구분으로 등장한 횟수만큼 인덱스 출력

# 0 0 1 1 2 2 3 4 5 5 6 7 8 9 9
```

# 실전 문제

'이것이 코딩테스트다' 책에서 제공하는 실전 문제는 모두 input을 받아서 푸는 방식이지만, 개인적으로 프로그래머스에서 푸는 방식과 같은 함수를 이용해 푸는 것이 익숙해 아래와 같은 풀이로 풀었다.

## 위에서 아래로

크기에 상관없이 나열되어 있는 수를 큰 수부터 작은 수의 순서로 정렬하는 프로그램 작성

```python
def solution(array):
    answer = sorted(array, reverse=True)
    for i in answer:
        print(i, end=' ')

array = [15, 27, 12]
solution(array)
# 27 15 12
```

## 성적이 낮은 순으로 학생 출력하기

학생 정보는 학생의 이름과 학생의 성적으로 구분된다. 각 학생의 이름과 성적 정보가 주어졌을 때, 성적이 낮은 순서대로 학생의 이름을 출력하는 프로그램 작성

> setting 함수를 따로 만들어 key를 이용한 풀이

```python
def setting(data):
    return data[1]

def solution(array):
    result = sorted(array, key=setting)
    answer = [i[0] for i in result]
    for i in answer:
        print(i, end=' ')

array = [("홍길동", 95), ("이순신", 77)]
solution(array)
# 이순신 홍길동
```

> 람다를 이용한 풀이

```python
def solution(array):
    result = sorted(array, key=lambda student: student[1])
    answer = [i[0] for i in result]
    for i in answer:
        print(i, end=' ')

array = [("홍길동", 95), ("이순신", 77)]
solution(array)
# 이순신 홍길동
```

## 두 배열의 원소 교체

배열 A와 B가 있다. 바꿔치기 연산은 A에 있는 원소 하나와 B에 있는 원소 하나를 골라서 두 원소를 서로 바꾸는 것이다. 이때 최대 N번의 바꿔치기 연산을 수행해서 A의 모든 원소의 합이 최대가 되는 프로그램 작성

```python
def solution(n, A, B):
    A.sort()
    B.sort(reverse=True)
    for i in range(n):
        if A[i] < B[i]:
            A[i], B[i] = B[i], A[i]
        else:
            break
    print(sum(A))

A = [1, 2, 5, 4, 3]
B = [5, 5, 6, 6, 5]
solution(3, A, B)
# 26
```
