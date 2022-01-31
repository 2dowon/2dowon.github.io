---
layout: default
title: Dictionary Comprehesion
last_modified_date: 2020-12-02 17:12:27
parent: Python
---

# Dictionary Comprehesion

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

# Dictionary Comprehesion

새로운 딕셔너리를 만들 때 사용하는 간단한 표현식

## Dictionary Comprehesion의 기본 구조

```python
{<반복 실행문> for <반복 변수> in <반복 범위>}
```

**실행 순서**

1. for <반복 변수> in <반복 범위>
2. <반복 실행문> ⇒ 반복 실행문은 Key : Value의 구성으로 작성

❗ 콜론`:`을 이용하지 않음

### Dictionary Comprehesion 예제

- A, B, C 문자가 들어있는 리스트를 Key 값으로 하는 딕셔너리 만들기

  ```python
  string_list = ['A','B','C']
  dictionary = {string : 0 for string in string_list}
  print(dictionary)

  # {'A': 0, 'B': 0, 'C': 0}
  ```

- `enumerate` 를 활용해 반복 횟수를 value로 하는 딕셔너리 만들기

  ```python
  string_list = ['A','B','C']
  dictionary = {string : i for i,string in enumerate(string_list)}
  print(dictionary)

  # {'A': 0, 'B': 1, 'C': 2}
  ```

## zip을 활용한 Dictionary Comprehesion

zip을 활용하면 두 개의 리스트를 하나의 딕셔터리로 만들 수 있다.

```python
students = ['A', 'B', 'C']
scores = [85, 92, 100]

score_dic = {
    students : score for students, score in zip(students, scores)
}

print(score_dic)

# {'A': 85, 'B': 92, 'C': 100}
```

🤔 근데 사실 두 개의 리스트를 하나의 딕셔너리로 만드는 경우에는 굳이 Dictionary Comprehesion을 이용하지 않아도 가능하다.

```python
students = ['A','B','C']
scores = [85, 92, 100]

score_dic  = dict(zip(students, scores))

print(score_dic)

# {'A': 85, 'B': 92, 'C': 100}
```
