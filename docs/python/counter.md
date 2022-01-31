---
layout: default
title: collections.Counter
last_modified_date: 2021-03-09 12:03:04
parent: Python
---

# collections.Counter

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

알고리즘 문제를 풀다보면 생각보다 데이터의 개수를 세야하는 상황이 많다. 이 경우 파이썬의 자료구조 중 하나인 dictionary를 이용하면 쉽게 데이터의 개수를 구할 수 있다. 또한 한 번만 반복하면 되기 때문에 시간 복잡도 `$O(N)$`으로 괜찮은 방법이다.

> hello world 라는 문자열 안에 있는 데이터의 개수 세기

```python
word = "hello world"

counter_dict = {}
for i in word:
    counter_dict[i] = counter_dict.get(i, 0) + 1

print(counter_dict)
# {'h': 1, 'e': 1, 'l': 3, 'o': 2, ' ': 1, 'w': 1, 'r': 1, 'd': 1}
```

# collections.Counter

하지만 위 방법 외에도 Python의 collections 모듈의 counter를 이용하면 더 쉽게 데이터의 개수를 셀 수 있다.

dictionary를 이용해서 데이터의 개수를 셀 때와 결과는 동일한데, 훨씬 간결하게 작성할 수 있다.

```python
from collections import Counter
Counter('hello world')

# Counter({'h': 1, 'e': 1, 'l': 3, 'o': 2, ' ': 1, 'w': 1, 'r': 1, 'd': 1})
```

Counter는 dictionary를 확장하고 있기 때문에, 사전에서 제공하는 API를 그대로 다 사용할 수 있다. 따라서 아래 예시처럼 dictionary의 key를 이용한 방법으로 가장 많이 등장하는 아이템을 찾을 수 있다.

> hello world 문자열에서 가장 많이 등장하는 알파벳 찾기

```python
from collections import Counter

word = "hello world"

counter = Counter(word)
max_count = -1
for key in counter:
    if counter[key] > max_count:
        max_count = counter[key]
        max_letter = key

print(max_letter, max_count) # l 3
```

## counter.most_common()

Counter에는 가장 많이 등장하는 아이템을 쉽게 찾을 수 있도록 `most_common()` 메서드가 있다. `most_common()` 메서드는 데이터의 개수가 많은 순서로 정렬해 리스트 안의 튜플 형태로 반환해준다.

(dictionary로 정렬한다면 value값을 기준으로 내림차순 정렬을 한 것과 같은 역할을 해준다.)

```python
from collections import Counter
Counter('hello world').most_common()

# [('l', 3), ('o', 2), ('h', 1), ('e', 1), (' ', 1), ('w', 1), ('r', 1), ('d', 1)]
```

`most_common()` 메서드에 인자로 숫자를 넘기면 해당 숫자만큼 값을 리턴한다. 따라서 가장 개수가 많은 데이터를 특정 개수만큼 얻고 싶을 때 사용한다.

```python
from collections import Counter

Counter('hello world').most_common(1) # [('l', 3)]
```

# Ref.

- [[파이썬] collections 모듈의 Counter 클래스 사용법](https://www.daleseo.com/python-collections-counter/)

- [collections - Container datatypes - Python 3.9.2 documentation](https://docs.python.org/3/library/collections.html#collections.Counter)
