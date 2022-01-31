---
layout: default
title: 선형 배열(Linear Arrays)
last_modified_date: 2020-12-19 21:12:12
parent: 어서와! 자료구조와 알고리즘은 처음이지?
grand_parent: Algorithm
permalink: /docs/algorithm/python-lecture/linear-arrays
---

| [어서와! 자료구조와 알고리즘은 처음이지?](https://programmers.co.kr/learn/courses/57) 강의를 듣고 정리한 내용입니다.

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">선형 배열(Linear Arrays)</div>

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

선형 배열 (Linear Arrays)은 데이터들이 선 (line) 처럼 일렬로 늘어선 형태를 말한다.

## 배열

- 보통 프로그래밍에서 배열 (array) 이라고 하면 같은 종류의 데이터가 줄지어 늘어서 있는 것을 뜻한다.
- 변수가 하나의 데이터를 저장하기 위한 것이라면, 배열은 여러 개의 데이터를 하나의 변수에 저장하기 위한 것이다.
- 배열의 인덱스는 0부터 시작한다.

## 리스트

- Python 에서는 서로 다른 종류의 데이터 또한 줄세울 수 있는 리스트 (list) 라는 데이터형이 있다.
- Python에서는 기본적으로 리스트를 제공하고, 배열을 제공하지 않는다. 즉, 리스트가 배열이다.

# Python 리스트에 활용할 수 있는 연산들

## 리스트 길이과 관계 없이 빠르게 실행 결과를 보게되는 연산들 ⇒ 상수 시간 O(1)

- 원소 덧붙이기 `.append()`
- 원소 하나를 꺼내기 `.pop()`

위 연산들은 리스트의 길이와 무관하게 빠르게 실행할 수 있는 연산들이다. 리스트의 길이가 아무리 길어도 맨 끝에 요소 하나를 추가하는 것이나 맨 끝 요소 하나를 빼는건 빠르게 할 수 있기 때문이다. 단, 마지막 원소를 덧붙이거나 꺼내는 경우에만 해당한다. 중간에 요소 하나를 추가하거나 빼는건 시간이 오래 걸린다.

## 리스트의 길이에 비례해서 실행 시간이 걸리는 연산들 ⇒ 선형 시간 O(n)

- 원소 삽입하기 `.insert()`
- 원소 삭제하기 `.del()`

위 연산들은 리스트의 길이가 길면 길수록 처리가 오래 걸린다. 특정 위치에 삽입하고, 삭제하기 때문에 삽입과 삭제 후에는 나머지를 모두 한칸씩 앞으로 당기거나 뒤로 미뤄야 하기 때문이다.

구체적으로 말하면 리스트의 길이예 실행 시간이 비례한다. 리스트 길이가 100 배가 되면, 위 연산들을 실행하는 데 걸리는 시간도 100 배 커진다.

### `remove` `del` `pop` 의 차이

Python 리스트에 활용할 수 있는 연산들을 공부하다 보니 remove, del, pop의 차이가 궁금해졌다. 3개 모두 리스트의 특정 요소를 삭제할 때 사용하기 때문이다. 간단히 정리하자면

- remove() 는 특정 값을 찾아서 삭제한다

  ```python
  >>> a = [0, 2, 3, 2]
  >>> a.remove(2)
  >>> a
  [0, 3, 2]
  ```

- del() 은 특정 인덱스를 찾아서 삭제한다

  ```python
  >>> a = [9, 8, 7, 6]
  >>> del a[1]
  >>> a
  [9, 7, 6]
  ```

- pop() 은 특정 인덱스를 찾아서 삭제하고 그 값을 반환한다

  ```python
  >>> a = [4, 3, 5]
  >>> a.pop(1)
  3
  >>> a
  [4, 5]
  ```

- 참고 [Stack Overflow](https://stackoverflow.com/questions/11520492/difference-between-del-remove-and-pop-on-lists)

# 빅오 표기법

위에서 빅오표기법을 통해 효율성을 알아봤기 때문에 빅오 표기법에 대해 간단히 정리하고자 한다. 빅오 표기법은 기본적으로 알고리즘 최악의 경우 복잡도를 측정한다고 한다. 즉 데이터의 양에 따라 얼마나 오랜 시간이 걸리는지 표기하는 것이다. 아래 그림에서 확인할 수 있듯이 빅오 표기법은 $O(1) < O(log n) < O(n) < O(nlogn) < O(n^2) <br O(2^n)$ 이 순서대로 시간이 오래 걸린다.
![BigO](/assets/images/algorithm/BigO.png)

# 참고

- [어서와! 자료구조와 알고리즘은 처음이지?](https://programmers.co.kr/learn/courses/57)
- [생활코딩 - 배열](https://opentutorials.org/module/1335/8677)
- [Python 연산에 따른 복잡도](https://www.ics.uci.edu/~pattis/ICS-33/lectures/complexitypython.txt)
- [빅오 표기법](https://medium.com/@callmedevmomo/%EC%9B%B9-%EA%B0%9C%EB%B0%9C%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-%EC%9E%90%EB%A3%8C%EA%B5%AC%EC%A1%B0%EC%99%80-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-01-%EB%B9%85%EC%98%A4-%ED%91%9C%EA%B8%B0%EB%B2%95-ff369f0efc1d)
