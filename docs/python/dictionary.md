---
layout: default
title: Dictionary
last_modified_date: 2020-12-02 22:12:18
parent: Python
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Dictionary</div>

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

# Dictionary

Dictionary는 대응 관계를 나타낼 수 있는 자료형이다.

## Dictionary의 기본 구조

```python
{Key1:Value1, Key2:Value2, Key3:Value3, ...}
```

- Value에는 정수, 문자열 등 외에도 리스트도 넣을 수 있다.

### Dictionary 쌍 추가하기

```python
dict = {1: 'a'}
dict[2] = 'b'
print(dict)

# {1: 'a', 2: 'b'}
```

### Dictionary 쌍 삭제하기

```python
dict = {1: 'a', 2: 'b'}
del dict[1]
print(dict)

# {2: 'b'}
```

### Key를 이용해 Value 값 얻기

```python
dict = {1: 'a', 2: 'b'}
print(dict[1])

# a
```

### ⚠️ Dictionary의 Key는 중복될 수 없다

Dictionary의 Key는 고유한 값으로 중복될 수 없다. 만약 중복된 Key를 설정한다면 이전의 같은 Key들은 모두 무시하고 나중에 위치한 Key를 반환한다.

```python
dict = {'A': 100, 'B': 85, 'C': 90, 'A':85}
print(dict)

# {'A': 85, 'B': 85, 'C': 90}
```

## Dictionary 관련 함수

### `keys()` ⇒ Key 리스트 만들기

keys() 함수를 이용하면 딕셔너리의 Key만 모아서 dict_keys 객체를 돌려준다. dict_keys 객체를 리스트처럼 사용하기 위해서는 list() 함수를 이용해 리스트로 형 변환을 해야 한다.

```python
dict = {'A': 100, 'B': 85, 'C': 90}
print(dict.keys())

# dict_keys(['A', 'B', 'C'])
```

```python
dict = {'A': 100, 'B': 85, 'C': 90}
print(list(dict.keys()))

# ['A', 'B', 'C']
```

### `values()` ⇒ Value 리스트 만들기

keys() 함수처럼 values() 함수는 딕셔너리의 Value만을 모아서 dict_values 객체를 돌려준다. dict_values 객체를 리스트처럼 사용하기 위해서는 list() 함수를 이용해 리스트로 형 변환을 해야 한다.

```python
dict = {'A': 100, 'B': 85, 'C': 90}
print(dict.values())

# dict_values([100, 85, 90])
```

```python
dict = {'A': 100, 'B': 85, 'C': 90}
print(list(dict.values()))

# [100, 85, 90]
```

### `items()` ⇒ Key, Value 쌍 얻기

items() 함수는 딕셔너리의 Key와 Value를 모아서 쌍으로 만들어 dict_items 객체를 돌려준다. dict_items 객체를 리스트처럼 사용하기 위해서는 list() 함수를 이용해 리스트로 형 변환을 해야 한다.

```python
dict = {'A': 100, 'B': 85, 'C': 90}
print(dict.items())

# dict_items([('A', 100), ('B', 85), ('C', 90)])
```

```python
dict = {'A': 100, 'B': 85, 'C': 90}
print(list(dict.items()))

# [('A', 100), ('B', 85), ('C', 90)]
```

### `clear()` ⇒ Dictionary의 모든 쌍 지우기

clear() 함수를 이용하면 Dictionary 안의 모든 요소를 삭제할 수 있다.

```python
dict = {'A': 100, 'B': 85, 'C': 90}
print(dict.clear())

# None
```

### `in` ⇒ Dictionary에 해당 Key가 있는지 확인하기

in 은 Dictionary에 해당 Key가 있는지 확인할 때 사용한다. 있으면 True, 없으면 False를 반환한다.

```python
dict = {'A': 100, 'B': 85, 'C': 90}
print("A" in dict)
print("D" in dict)

# True
# False
```

### `get()` ⇒ Key로 Value 얻기

get() 함수를 이용하면 Dictionary에서 Key에 대응하는 Value 값을 얻을 수 있다. `["key"]` 를 사용했을 때와 동일한 결과를 가져오는데, get() 함수는 존재하지 않는 키 값을 가져오려고 할 때도 에러가 아니라 None 값을 돌려준다는 차이가 있다. 그리고 Value에 디폴트 값을 지정할 수 있다!

- get() 함수로 존재하지 않는 키 값을 가져오면 None이 반환된다

```python
dict = {'A': 100, 'B': 85, 'C': 90}
print(dict.get("D"))
**#** None
```

- 존재하지 않는 Key 값을 찾을 때 미리 정해둔 디폴트 값을 가져올 수 있다

```python
dict = {'A': 100, 'B': 85, 'C': 90}
print(dict.get("D", 70))
# 70
```

이 방법을 이용하면 프로그래머스 [베스트앨범](https://programmers.co.kr/learn/courses/30/lessons/42579) 문제에서 같은 장르별로 플레이 횟수를 더할 수 있다. 이 때 get() 함수를 이용해 디폴트 값을 따로 지정해주지 않으면 처음에는 Value값이 존재하지 않아 에러가 발생한다.

```python
def solution(genres, plays):
    dic = {}
    for i in range(len(genres)):
        dic[genres[i]] = dic.get(genres[i], 0) + plays[i]

    print(dic)

genres = ['classic', 'pop', 'classic', 'classic', 'pop']
plays = [500, 600, 150, 800, 2500]
solution(genres, plays)

# {'classic': 1450, 'pop': 3100}
```
