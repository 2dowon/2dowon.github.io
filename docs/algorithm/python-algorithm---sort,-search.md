---
layout: default
title: 정렬(sort), 탐색(search)
last_modified_date: 2021-01-03 23:01:58
parent: 어서와! 자료구조와 알고리즘은 처음이지?
grand_parent: Algorithm
permalink: /docs/algorithm/python-lecture/sort-search
---

| [어서와! 자료구조와 알고리즘은 처음이지?](https://programmers.co.kr/learn/courses/57) 강의를 듣고 정리한 내용입니다.

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">정렬 (sort)</div>

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

정렬은 복수의 원소로 주어진 데이터를 정해진 기준에 따라 새로 늘어놓는 작업이다. 정렬 알고리즘에는 선택 정렬, 버블 정렬, 삽입 정렬 등이 있다. 하지만 Python의 리스트(list)에는 내장된 정렬 기능이 있기 때문에 사실상 직접 정렬 알고리즘을 구현할 필요가 없다.

## 파이썬 내장 함수 `sorted()`

- 순서대로 정렬, 정렬된 리스트를 반환
- sorted() 를 이용하면 정렬된 새로운 리스트를 만든다 (기존 리스트는 그대로)
- L2 = sorted(L, reverse=True) : True는 내림차순 정렬, False는 오름차순 정렬

```python
L = [3, 8, 2, 7, 6, 10, 9]
L2 = sorted(L)
L2
# [2, 3, 6, 7, 8, 9, 10]

L
# [3, 8, 2, 7, 6, 10, 9]
```

## 리스트에 쓸 수 있는 메서드 `.sort()`

- sort() : 정렬, 기본값은 오름차순 정렬
- sort(reverse = True) : True는 내림차순 정렬, False는 오름차순 정렬
- sort()를 이용하면 기존 리스트 자체가 정렬

```python
a = [1, 10, 5, 7, 6]
a.sort()
a
# [1, 5, 6, 7, 10]

a = [1, 10, 5, 7, 6]
a.sort(reverse=True)
a
# [10, 7, 6, 5, 1]
```

## 데이터형의 정렬

문자열은 아스키 코드에 따라 A~Z 순으로 정렬된다. Python 문자열은 대문자가 소문자에 비해서 무조건 우선한다. (아스키 코드에 의하면 A =65, a =97이다.)

### 문자열의 길이 순서로 정렬

```python
L = ['abcd', 'xyz', 'spam']
sorted(L, key=lambda x: len(x))
# ['xyz', 'abcd', 'spam']

L = ['spam', 'xyz', 'abcd']
sorted(L, key=lambda x: len(x))
# ['xyz', 'spam', 'abcd']
```

여러 데이터의 복합으로 이루어진 데이터 원소를 보통 (데이터베이스에서) 레코드 (record) 라고 부른다. 레코드를 Python 사전 (dictionary) 으로 표현하고 이것은 아래와 같이 정렬할 수 있다.

### 레코드들을 이름 순서대로 정렬

```python
L = [{'name':'John', 'score':83}, {'name':'Paul', 'score':92}]
L.sort(key=lambda x:x['name'])
```

# 탐색 (search)

복수의 원소로 이루어진 데이터에서 특정 원소를 찾아내는 작업이다. 탐색 알고리즘에는 선형 탐색, 이진 탐색, 해시 탐색 등이 있다.

## 선형탐색(linear search), 또는 순차 탐색 (sequential search)

순차적으로 모든 요소들을 탐색하여 원하는 값을 찾아낸다. 배열의 길이에 비례하는 시간(`O(n)`)이 걸리므로, 최악의 경우에는 배열에 있는 모든 원소를 다 검사해야 할 수도 있다.

예를 들어서 `if x in L` 처럼 리스트 L 안에 변수 x가 있는지 판단하는 경우, `index()` 메서드, `count()` 메서드 등등은 선형 탐색을 하기 때문에 리스트의 길이에 비례하는 시간을 소요하게 된다.

```python
def linear_search(L, x) :
	i = 0
	while i < len(L) and L[i] != x :
		i += 1
	if i < len(L) :
		return i
	else :
		return -1
```

## 이진탐색(binary search)

탐색하려는 배열이 이미 정렬되어 있는 경우에만 적용할 수 있다. 배열의 가운데 원소와 찾으려 하는 값을 비교하면, (크기 순으로 정렬되어 있다는 성질을 이용하면) 왼쪽에 있을지 오른쪽에 있을지를 알 수 있다 (만약 있긴 있다면). 그러면, 적어도 반대쪽에 없는 것은 확실하므로, 배열의 반을 탐색하지 않고 버릴 수 있다. 이 과정을 반복하면 원하는 값을 찾아낼 수 있습니다.

예전에 [부스트코스 CS50 수업 중 알고리즘 강의](https://www.boostcourse.org/cs112/lecture/118999)를 들었었는데, 그때 전화번호부 예시가 이진탐색과 같다. 전화번호부에 이름은 A~Z순으로 정렬되어 있고, 거기서 TOM의 이름을 찾는다면 먼저 반을 펼치고 만약 MARY라면 TOM은 뒷부분에 있으므로 앞부분은 찢어서 버리고 다시 뒷부분을 반으로 나눠서 펼치고 이 과정을 반복하다보면 결국은 TOM을 찾을 수 있다.

```python
lower = 0
upper = len(L) - 1
idx = -1
while lower <= upper :
	middle = (lower + upper) // 2
	if L[middle] == target :
		...
	elif L[middle] < target :
		lower = ...
	else :
		upper = ...
```

⚠️ 이진 탐색이 선형 탐색보다 빠른 방법이다. 하지만 이진 탐색은 배열이 이미 정렬되어 있는 경우에만 사용될 수 있으므로 정렬이 안 된 배열의 경우 이를 정렬하고 이진 탐색을 하는 것보다 처음부터 선형 탐색을 하는 것이 더 빠를 수도 있다. (한 번만 탐색할 때의 얘기이고, 탐색을 여러 번 해야 한다면 한 번 정렬해두고 이진 탐색을 반복하는 것이 낫다!)

# 참고

- [어서와! 자료구조와 알고리즘은 처음이지?](https://programmers.co.kr/learn/courses/57)
- [Boostcourse - CS50](https://www.boostcourse.org/cs112/lecture/118999)
