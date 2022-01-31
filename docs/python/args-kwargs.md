---
layout: default
title: args, kwargs
last_modified_date: 2021-02-02 12:02:21
parent: Python
---

# \*args, \*\*kwargs

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

`*args`, `**kwargs`는 무한대로 arguments를 받고 싶을 때 사용한다.

따라서 몇 개의 arguments를 받을지 모르는 함수를 만들 때 주로 사용한다.

# \*args

- \*arguments의 줄임말
- 여러 개의 positional argument를 함수로 받고자 할 때 쓴다
- 여러개의 인자로 함수를 호출할 경우, tuple 형태`( )`로 함수 내부로 전달된다.
- 반드시 `*args` 라고 적을 필요는 없다. `*potato` 처럼 원하는 단어로 사용 가능하다.

## 예시

### plus(a, b)

- `plus(a, b)`는 arguments를 2개만 받을 수 있다.
- 만약 3개 이상의 arguments를 전달하면 에러가 발생한다.

```python
def plus(a, b):
	print(a+b)

plus(1,2)
# 3
plus(1,2,12,2)
# plus() takes 2 positional arguments but 4 were given
```

### plus(\*args)

- `plus(*args)` 는 arguments를 무한대로 받을 수 있다.

```python
def plus(*args):
	result = 0
	for number in args:
		result += number
	print(result)

plus(1,2)
# 3
plus(1,2,3,4,5,6,7,8,9,10)
# 55
```

### plus(a, b, \*args)

- 더하기 함수이므로 최소한 2개의 수를 받고 싶다면 아래처럼 a, b는 무조건 받고 그 다음에는 원하는 개수의 arguments를 받을 수 있도록 한다

```python
def plus(a, b, *args):
	result = a+b
	for number in args:
		result += number
	print(result)

plus(1)
# plus() missing 1 required positional argument: 'b'
plus(1,2)
# 3
plus(1,2,3,4,5,6,7,8,9,10)
# 55
```

⚠️ 이 때 항상 받고자 하는 인자(여기서는 a, b)가 *args보다 먼저 위치해야 한다. `plus(*args, a, b)` 라고 적으면 모든 인자가 \*args로 전달되기 때문에 인자 a와 b가 존재하지 않는다는 에러가 발생한다.

```python
def plus(*args, a , b):
	result = a+b
	for number in args:
		result += number
	print(result)

plus(1,2,3,4,5,6,7,8,9,10)
# plus() missing 2 required keyword-only arguments: 'a' and 'b'
```

# \*\*kwargs

- kwargs = keyword argument
- keyword argument를 받을 때 사용
- \*\*kwargs는 `(키워드 = 특정 값)` 형태로 함수를 호출 가능하다
- 이 경우 딕셔너리 형태`{'키워드' : '특정 값'}`로 함수 내부로 전달된다
- 반드시 `**kwargs` 라고 적을 필요는 없다. `**potato` 처럼 원하는 단어로 사용 가능하다.

## 예시

```python
def myName(**kwargs):
  for key, value in kwargs.items():
    print(f"My {key} is {value}.")

myName(name="Dowon")
# My name is Dowon.
```

⚠️ 만약 `*args`와 같이 사용하는 경우 `*args`가 `**kwargs` 보다 앞에 와야 한다.

```python
def myName(*args, **kwargs):
	pass
```

# Ref.

- [Lecture - 노마드 코더 Nomad Coders](https://nomadcoders.co/python-for-beginners/lectures/135)

- [[나름 중급 파이썬1] \*args와 \*\*kwargs](https://brunch.co.kr/@princox/180)
