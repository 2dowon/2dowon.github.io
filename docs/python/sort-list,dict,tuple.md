---
layout: default
title: LIST, TUPLE, DICT 정렬
last_modified_date: 2021-02-05 14:02:59
parent: Python
---

# LIST, TUPLE, DICT 정렬

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

파이썬에서 정렬이 필요할 때는 `sort()` 나 `sorted()` 함수를 이용하면 쉽게 리스트의 원소를 정렬할 수 있다. 만약 리스트가 아닌 tuple, dict와 같은 자료형을 정렬하고 싶다면 그 때는 sort, sorted 함수에 파라마터 key를 이용해 정렬할 수 있다.

# sort()

- sort 함수를 이용하면 리스트의 원소를 정렬할 수 있다.
- 이 때, sort 함수는 원본의 순서를 변경한다.
- 기본 값은 `reverse=False`로 오름차순으로 정렬한다.
  만약 내림차순으로 정렬하고 싶다면 `reverse=True` 를 이용하면 된다.

```python
a = [3, 2, 1, 4, 5]
a.sort()
print(a) # [1, 2, 3, 4, 5]

b = [3, 2, 1, 4, 5]
b.sort(reverse=True)
print(b) # [5, 4, 3, 2, 1]
```

# sorted()

- sorted 함수도 sort 함수처럼 리스트의 원소를 정렬할 때 사용한다.
- sorted 함수는 sort 함수와 다르게 원본의 순서를 변경하지 않는다.
- 기본 값은 `reverse=False`로 오름차순으로 정렬한다.
  만약 내림차순으로 정렬하고 싶다면 `reverse=True` 를 이용하면 된다.

```python
a = [3, 2, 1]
b = sorted(a)

print(a) # [3, 2, 1]
print(b) # [1, 2, 3]
```

# key를 이용한 정렬

- sort와 sorted 함수에 파라미터 key값을 이용해 이를 기준으로 정렬할 수 있다.
- key값을 기준으로 기본값인 오름차순으로 정렬한다.

> 원소의 길이를 기준으로 정렬

```python
a = ["hello", "my", "name", "is", "dowon"]
a.sort(key=len)

print(a)
# ['my', 'is', 'name', 'hello', 'dowon']
```

# key - lambda를 이용한 정렬

key 값에는 lambda를 이용할 수 있다. lambda는 tuple, dict 자료형을 정렬할 때 효과적이지만, 리스트 자료형을 정렬할 때도 사용할 수 있다. lambda를 이용한 정렬을 알아보기 전, 간단히 lambda를 정리하고 넘어가려고 한다.

### lambda 람다

- lambda 함수는 식 형태로 되어 있어 람다 표현식이라고 부른다.
- 람다 표현식은 이름이 없는 함수를 만들기 때문에 **익명 함수**라고도 한다.
- lambda 람다를 이용하면 함수를 간편하게 작성할 수 있어 다른 함수의 인수로 넣을 때 주로 사용한다.

> x를 전달받아서 10을 더해서 리턴하는 함수

```python
plus_ten = lambda x: x + 10
plus_ten(1) # 11
```

## list 정렬

> lambda를 이용해 알파벳 기준으로 정렬

```python
a = ["hello", "my", "name", "is", "dowon"]
a.sort(key=lambda x:x[0])
print(a)

# ['dowon', 'hello', 'is', 'my', 'name']
```

> 리스트 안의 두 번째 리스트의 최댓값을 기준으로 정렬

```python
mylist = [ ['a', [5, 6, 7]], ['b', [2, 3, 4]], ['c', [1, 2, 3]] ]
a.sort(key=lambda x: max(x[1]))
print(a)

# [['c', [1, 2, 3]], ['b', [2, 3, 4]], ['a', [5, 6, 7]]]
```

## tuple 정렬

> 숫자를 기준으로 먼저 정렬하고, 숫자가 같다면 그때는 단어의 알파벳 순으로 정렬

```python
a = [("hello",0), ("my",5), ("name",4), ("is",10), ("dowon", 10)]
a.sort(key=lambda x:(x[1], x[0]))
print(a)

# [('hello', 0), ('name', 4), ('my', 5), ('dowon', 10), ('is', 10)]
```

> (name, grade, age) 순으로 저장되어 있는 튜플을 age를 기준으로 오름차순 정렬

```python
student_tuples = [
		('john', 'A', 15),
		('jane', 'B', 12),
		('dave', 'B', 10),
]

sorted(student_tuples, key=lambda student: student[2])
# sort by age
# [('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

> (name, grade, age) 순으로 저장되어 있는 튜플을 age를 기준으로 내림차순 정렬

- key 기준이 int형인 경우 key값에 마이너스(-)를 붙이면 내림차순으로 정렬할 수 있다

```python
student_tuples = [
    ('john', 'A', 15),
    ('jane', 'B', 12),
    ('dave', 'B', 10),
]

sorted(student_tuples, key=lambda student: -student[2])
# [('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]
```

## dict 정렬

> key값을 기준으로 정렬

```python
student_dict = {'john':15, 'jane':12, 'dave':17}
sorted(student_dict.items(), key=lambda student: student[0])
# [('dave', 17), ('jane', 12), ('john', 15)]
```

> value값을 기준으로 정렬

```python
student_dict = {'john':15, 'jane':12, 'dave':17}
sorted(student_dict.items(), key=lambda student: student[1])
# [('jane', 12), ('john', 15), ('dave', 17)]
```

# Ref.

- [파이썬 코딩 도장](https://dojang.io/mod/page/view.php?id=2359)

- [파이썬 정렬시 특정 기준으로 정렬하는 방법 - key Fucntion, repr 메소드](https://wayhome25.github.io/python/2017/03/07/key-function/)

- [파이썬 정렬 함수 sort, sorted \_ key = lambda, function / reverse= 파라미터 이용 방법 (Python)](https://ooyoung.tistory.com/59)

- [파이썬 리스트, 튜플 정렬하기 - sort(), sorted(), reverse()](https://zidarn87.tistory.com/25)
