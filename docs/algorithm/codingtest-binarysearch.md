---
layout: default
title: 이진 탐색(Binary Search)
last_modified_date: 2021-01-23 23:01:45
parent: 이것이 취업을 위한 코딩테스트다 with 파이썬
grand_parent: Algorithm
permalink: /docs/algorithm/python-book/binary-search/
---

| [이것이 취업을 위한 코딩테스트다 with 파이썬](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)를 읽고 정리한 내용입니다.

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">순차 탐색 (Sequential Search)</div>

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

순차 탐색은 가장 기본 탐색 방법이다. N개의 데이터가 있을 때, 그 데이터를 차례대로 하나씩 확인하는 것으로 **리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 차례대로 확인하는 방법**이다.

순차 탐색은 보통 정렬되지 않은 리스트에서 데이터를 찾아야 할 때 사용한다. 리스트 내에 데이터가 아무리 많아도 시간만 충분하다면 항상 원하는 데이터를 찾을 수 있다는 장점이 있다. 예를 들어, count() 메서드를 이용할 때도 리스트의 데이터에 하나씩 방문하면서 특정 값을 가지는 원소의 개수를 세는 것이므로 순차 탐색이 수행된다.

순차 탐색은 데이터의 정렬 여부와 상관없이 가장 앞에 있는 원소부터 하나씩 확인해야 하므로 데이터의 개수가 N개일 때 최대 N번의 비교 연산이 필요하다. 따라서 최악의 경우 시간 복잡도는 $O(N)$이다.

> 순차 탐색 소스 코드

```python
# 순차 탐색 소스코드 구현
def sequential_search(n, target, array):
    # 각 원소를 하나씩 확인하며
    for i in range(n):
        # 현재의 원소가 찾고자 하는 원소와 동일한 경우
        if array[i] == target:
            return i + 1 # 현재의 위치 반환 (인덱스는 0부터 시작하므로 1 더하기)
    return -1 # 원소를 찾지 못한 경우 -1 반환

print("생성할 원소 개수를 입력한 다음 한 칸 띄고 찾을 문자열을 입력하세요.")
input_data = input().split()
n = int(input_data[0]) # 원소의 개수
target = input_data[1] # 찾고자 하는 문자열

print("앞서 적은 원소 개수만큼 문자열을 입력하세요. 구분은 띄어쓰기 한 칸으로 합니다.")
array = input().split()

# 순차 탐색 수행 결과 출력
print(sequential_search(n, target, array))
```

# 이진 탐색 (Binary Search)

이진 탐색은 배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘이다. 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 특징을 갖는다. 이진 탐색은 위치를 나타내는 변수 3개를 사용하는데, 탐색하고자 하는 범위의 시작점, 끝점, 중간점이다. 찾으려는 데이터와 중간점 위치에 있는 데이터를 반복적으로 비교해서 원하는 데이터를 찾는 것이 이진 탐색의 과정이다.

예를 들어, 전화번호부에서 '이도원'을 찾는다고 가정하자. 전화번호부이므로 ㄱ부터 ㅎ순으로 정렬되어있을 것이다. 먼저 전화번호부의 절반 부분을 펼쳤을 때 ㅅ의 이름이 있다면 '이도원'은 ㅅ 이후이므로 전화번호부의 앞쪽 절반은 필요가 없으므로 버린다. 따라서 이제 전화번호부 절반 뒷부분의 처음부터 끝의 중간을 펼치고 '이도원'이 앞쪽에 있다면 이번에는 중간 뒷부분을 버린다. 위 과정을 반복하다보면 어느순간 '이도원'을 찾을 수 있다. 좀 더 쉽게 페이지 수를 가정해보면 512페이지의 전화번호부가 있고 '이도원'은 64페이지에 있다. 이때 처음 절반으로 나누면 256페이지, 다시 절반으로 나누면 128페이지, 또 절반으로 나누면 64페이지를 확인할 수 있으므로 총 3번만에 '이도원'을 찾을 수 있다. 만약 순차 탐색으로 이를 찾게 되면 64페이지까지 확인을 해야 하지만, 이진 탐색의 경우 단 3번 만에 찾을 수 있는 것이다.

이처럼 이진 탐색은 한 번 확인할 때마다 확인하는 원소의 개수가 절반씩 줄어든다는 점에서 시간 복잡도가 $O(logN)$이다.

이진 탐색 문제는 탐색 범위가 큰 상황에서의 탐색을 가정하는 문제가 많다. 따라서 탐색 범위가 2,000만을 넘어가거나 처리해야할 데이터의 개수나 값이 1,000만 단위 이상으로 넘어가면 이진 탐색으로 접근하는 것이 좋다.

이진 탐색을 구현하는 방법에는 재귀 함수를 이용하는 방법과 단순하게 반복문을 이용하는 방법이 있다.

> 재귀함수로 구현한 이진 탐색 소스 코드

```python
# 이진 탐색 소스코드 구현 (재귀 함수)
def binary_search(array, target, start, end):
    if start > end:
        return None
    mid = (start + end) // 2
    # 찾은 경우 중간점 인덱스 반환
    if array[mid] == target:
        return mid
    # 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
    elif array[mid] > target:
        return binary_search(array, target, start, mid - 1)
    # 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
    else:
        return binary_search(array, target, mid + 1, end)

# n(원소의 개수)과 target(찾고자 하는 값)을 입력 받기
n, target = list(map(int, input().split()))
# 전체 원소 입력 받기
array = list(map(int, input().split()))

# 이진 탐색 수행 결과 출력
result = binary_search(array, target, 0, n - 1)
if result == None:
    print("원소가 존재하지 않습니다.")
else:
    print(result + 1)
```

> 반복문으로 구현한 이진 탐색 소스 코드

```python
# 이진 탐색 소스코드 구현 (반복문)
def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2
        # 찾은 경우 중간점 인덱스 반환
        if array[mid] == target:
            return mid
        # 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
        elif array[mid] > target:
            end = mid - 1
        # 중간점의 값보다 찾고자 하는 값이 큰 경우 오른쪽 확인
        else:
            start = mid + 1
    return None

n = 10
target = 7
array = [1, 3, 5, 7, 9, 11, 13, 15, 17, 19]

# 이진 탐색 수행 결과 출력
result = binary_search(array, target, 0, n - 1)
if result == None:
    print("원소가 존재하지 않습니다.")
else:
    print(result + 1)
```

# 트리 자료구조

트리 자료구조는 노드와 노드의 연결로 표현한다. 노드는 정보의 단위로서 어떠한 정보를 가지고 있는 개체로 이해할 수 있다. 트리 자료구조는 그래프 자료구조의 일종으로 데이터베이스 시스템이나 파일 시스템과 같은 곳에서 많은 양의 데이터를 관리하기 위한 목적으로 사용한다.

![binarysearch1](/assets/images/algorithm/binarysearch1.png)

> 트리 자료구조의 특징

- 트리는 부모(Parent) 노드와 자식(Child) 노드의 관계로 표현된다.
- 트리의 최상단 노드를 루트(Root) 노드라고 한다.
- 트리의 최하단 노드를 단말(Leaf) 노드라고 한다.
- 트리에서 일부를 떼어내도 트리 구조이며 이를 서브 트리(Sub-tree)라 한다.
- 트리는 파일 시스템과 같이 계층적이고 정렬된 데이터를 다루기에 적합하다.

## 이진 탐색 트리

이진 탐색 트리는 트리 자료구조 중에서 가장 간단한 형태로 이진 탐색이 동작할 수 있도록 고안된, 효율적인 탐색이 가능한 자료구조이다.

![binarysearch2](/assets/images/algorithm/binarysearch2.png)

> 이진 탐색 트리의 특징

- 부모 노드보다 왼쪽 자식 노드가 작다
- 부모 노드보다 오른쪽 자식 노드가 크다
- **왼쪽 자식 노드 < 부모 노드 < 오른쪽 자식 노드**

# 실전 문제

'이것이 코딩테스트다' 책에서 제공하는 실전 문제는 모두 input을 받아서 푸는 방식이지만, 개인적으로 프로그래머스에서 푸는 방식이 익숙해 책의 소스코드를 약간 수정해 아래와 같은 풀이로 풀었다.

## 부품 찾기

손님이 요청한 부품 번호의 순서대로 부품을 확인해 있으면 yes, 없으면 no를 출력하는 프로그램

> 집합 자료형을 이용한 풀이

- 단순히 특정한 데이터가 존재하는지 검사할 때 매우 효과적
- 이 문제에서는 이 풀이가 가장 간결한 풀이

```python
def solution(array, target):
	array = set(array) # 집합(set) 자료형에 기록

    # 손님이 확인 요청한 부품 번호를 하나씩 확인
    for i in target:
        # 해당 부품이 존재하는지 확인
        if i in array:
            print('yes', end=' ')
        else:
            print('no', end=' ')


array = [8, 3, 7, 9, 2] # 가게의 부품 번호
target = [5, 7, 9] # 손님의 요청 부품 번호
solution(array, target)
# no yes yes
```

> 이진 탐색을 이용한 풀이

```python
# 이진 탐색 소스코드 구현 (반복문)
def binary_search(array, target, start, end):
    while start <= end:
        mid = (start + end) // 2
        # 찾은 경우 중간점 인덱스 반환
        if array[mid] == target:
            return mid
        # 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
        elif array[mid] > target:
            end = mid - 1
        # 중간점의 값보다 찾고자 하는 값이 작은 경우 오른쪽 확인
        else:
            start = mid + 1
    return None


def solution(array, x):
    array.sort() # 이진 탐색을 수행하기 위해 사전에 정렬 수행

    # 손님이 확인 요청한 부품 번호를 하나씩 확인
    for i in x:
        # 해당 부품이 존재하는지 확인
        result = binary_search(array, i, 0, len(array) - 1)
        if result != None:
            print('yes', end=' ')
        else:
            print('no', end=' ')


array = [8, 3, 7, 9, 2]
x = [5, 7, 9]
solution(array, x)
# no yes yes
```

> 계수 정렬을 이용한 풀이

```python
def solution(array, target):
    n = len(array)
    array_list = [0] * 1000001 # 모든 원소의 번호를 포함할 수 있는 크기의 리스트

    for i in array:
        array_list[int(i)] = 1

    # 손님이 확인 요청한 부품 번호를 하나씩 확인
    for i in target:
        # 해당 부품이 존재하는지 확인
        if array_list[i] == 1:
            print('yes', end=' ')
        else:
            print('no', end=' ')


array = [8, 3, 7, 9, 2]
target = [5, 7, 9]
solution(array, target)
# no yes yes
```

## 떡볶이 떡 만들기

떡볶이 떡은 절단기에 높이(H)를 지정하여 줄지어진 떡을 한 번에 절단된다. 높이가 H보다 긴 떡은 H 위의 부분이 잘릴 것이고, 낮은 떡은 잘리지 않는다. 손님은 잘린 떡을 가져갈 수 있다. 손님이 왔을 때 요청한 총 길이가 M일 때 적어도 M만큼의 떡을 얻기 위해 절단기에 설정할 수 있는 높이의 최댓값을 구하는 프로그램

```python
def solution(array, target):
    # 이진 탐색을 위한 시작점과 끝점 설정
    start = 0
    end = max(array)

    # 이진 탐색 수행 (반복적)
    result = 0
    while(start <= end):
        total = 0
        mid = (start + end) // 2
        for x in array:
            # 잘랐을 때의 떡볶이 양 계산
            if x > mid:
                total += x - mid
        # 떡볶이 양이 부족한 경우 더 많이 자르기 (오른쪽 부분 탐색)
        if total < target:
            end = mid - 1
        # 떡볶이 양이 충분한 경우 덜 자르기 (왼쪽 부분 탐색)
        else:
            result = mid # 최대한 덜 잘랐을 때가 정답이므로, 여기에서 result에 기록
            start = mid + 1

    # 정답 출력
    print(result)


array = [19, 15, 10, 17]
target = 6
solution(array, target)
# 15
```

- 적절한 높이를 찾을 때까지 절단기의 높이 H를 반복해서 조정하는 것
- 절단기의 높이 범위는 1부터 10억까지의 정수이므로 순차 탐색을 이용하면 시간 초과를 받는다. 따라서 이와 같은 큰 수의 범위를 보면 이진 탐색을 떠올려서 문제를 풀어야 한다.
- 위와 같은 파라메트릭 서치 문제 유형은 이진 탐색을 재귀적으로 구현하지 않고 반복문을 이용해 구현하면 더 간결하게 문제를 풀 수 있다.
- 파라메트릭 서치(Parametric Search) : 최적화 문제를 결정 문제(예/아니오)로 바꾸어 해결하는 기법

# Ref.

- [이것이 취업을 위한 코딩테스트다 with 파이썬](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [[자료구조] 트리(Tree)란 - Heee's Development Blog](https://gmlwjd9405.github.io/2018/08/12/data-structure-tree.html)
