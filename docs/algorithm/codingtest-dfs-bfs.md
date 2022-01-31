---
layout: default
title: 탐색(DFS, BFS)
last_modified_date: 2021-01-14 22:01:45
parent: 이것이 취업을 위한 코딩테스트다 with 파이썬
grand_parent: Algorithm
permalink: /docs/algorithm/python-book/dfs-bfs/
---

| [이것이 취업을 위한 코딩테스트다 with 파이썬](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)를 읽고 정리한 내용입니다.

# 탐색

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

- 많은 양의 데이터 중에서 원하는 데이터를 찾는 과정
- ex. 그래프, 트리 등의 자료구조 안에서 탐색을 하는 문제
- 대표적인 탐색 알고리즘 ⇒ DFS / BFS
- DFS / BFS를 제대로 이해하기 위해서는 자료구조를 먼저 알아야한다.
- 자료구조는 데이터를 표현하고 관리하고 처리하기 위한 구조이다
- 스택, 큐는 자료구조의 기초 개념이다.

## 스택 stack

- FILO (First In Last Out) 또는 LIFO(Last In First Out) 구조
- 처음 들어간 것이 가장 나중에 나오고, 나중에 들어간 것이 가장 먼저 나온다
- ex. 박스 쌓기, 막다른 곳에 주차할 때
- Python List에서 `append()` , `pop()` 메서드를 이용하면 스택 자료구조와 동일하게 동작
  - `append()` : 리스트의 가장 뒤쪽에 데이터를 삽입한다
  - `pop()` : 리스트의 가장 뒤쪽에서 데이터를 꺼낸다

```python
stack = []

# 삽입(5) - 삽입(2) - 삽입(3) - 삽입(7) - 삭제() - 삽입(1) - 삽입(4) - 삭제()
stack.append(5)
stack.append(2)
stack.append(3)
stack.append(7)
stack.pop()
stack.append(1)
stack.append(4)
stack.pop()

print(stack) # 최하단 원소부터 출력
# [5, 2, 3, 1]
print(stack[::-1]) # 최상단 원소부터 출력
# [1 3, 2, 5]
```

## 큐 Queue

- FIFO (First In First Out) 구조
- 먼저 들어간 것이 먼저 나오고, 나중에 들어간 것이 나중에 나온다
- ex. 대기줄
- Python에서 큐를 구현할 때는 collections 모듈에서 제공하는 deque 자료구조를 활용하는 것이 좋다
- deque는 스택과 큐의 장점을 모두 채택한 것으로 데이터를 넣고 빼는 속도가 리스트에 비해 효율적

```python
from collections import deque

# 큐(Queue) 구현을 위해 deque 라이브러리 사용
queue = deque()

# 삽입(5) - 삽입(2) - 삽입(3) - 삽입(7) - 삭제() - 삽입(1) - 삽입(4) - 삭제()
queue.append(5)
queue.append(2)
queue.append(3)
queue.append(7)
queue.popleft()
queue.append(1)
queue.append(4)
queue.popleft()

print(queue) # 먼저 들어온 순서대로 출력
# deque([3, 7, 1, 4])
queue.reverse() # 다음 출력을 위해 역순으로 바꾸기
print(queue) # 나중에 들어온 원소부터 출력
# deque([4, 1, 7, 3])
```

## 재귀함수

- 자기 자신을 다시 호출하는 함수
- 재귀함수에서는 종료 조건을 반드시 명시해야 한다. 안그러면 함수가 무한 호출될 수 있다.
- 재귀함수는 내부적으로 스택 자료구조와 동일
  ⇒ 스택 자료구조를 활용해야 하는 상당수 알고리즘은 재귀 함수를 이용해서 간편하게 구현 가능 ex. DFS

> 팩토리얼 예제

```python
# 반복적으로 구현한 n!
def factorial_iterative(n):
    result = 1
    # 1부터 n까지의 수를 차례대로 곱하기
    for i in range(1, n + 1):
       result *= i
    return result

# 재귀적으로 구현한 n!
def factorial_recursive(n):
    if n <= 1: # n이 1 이하인 경우 1을 반환
        return 1
    # n! = n * (n - 1)!를 그대로 코드로 작성하기
    return n * factorial_recursive(n - 1)

# 각각의 방식으로 구현한 n! 출력(n = 5)
print('반복적으로 구현:', factorial_iterative(5))
# 반복적으로 구현: 120
print('재귀적으로 구현:', factorial_recursive(5))
# 반복적으로 구현: 120
```

# DFS / BFS

## DFS

- Depth-First Search 깊이 우선 탐색
- 그래프에서 깊은 부분을 우선적으로 탐색하는 알고리즘

### 프로그래밍에서 그래프는 2가지 방식으로 표현

1. 인접 행렬 (Adjacency Matrix)

   ![dfs1](/assets/images/algorithm/dfs1.png)

   - 2차원 배열로 그래프의 연결관계를 표현하는 방식
   - 인접 행렬로 표현하기 위해 파이썬에서 2차원 리스트로 구현한다. 연결이 되어 있지 않은 노드끼리는 무한의 비용이라고 작성한다. 보통 999999999 등과 같은 값으로 초기화한다.
   - 모든 관계를 저장하므로 노드 개수가 많을수록 메모리가 불필요하게 낭비된다.
   - 하지만 특정 두 노드가 연결되어 있는지에 대한 정보를 얻는 속도는 상대적으로 빠르다

   ```python
   INF = 999999999 # 무한의 비용 선언

   # 2차원 리스트를 이용해 인접 행렬 표현
   graph = [
       [0, 7, 5],
       [7, 0, INF],
       [5, INF, 0]
   ]

   print(graph)
   # [[0, 7, 5], [7, 0, 999999999], [5, 999999999, 0]]
   ```

2. 인접 리스트 (Adjacency List)

   ![dfs2](/assets/images/algorithm/dfs2.png)

   - 리스트로 그래프의 연결 관계를 표현하는 방식
   - 인접 리스트에서는 연결 리스트라는 자료구조를 이용해 구현한다.
   - 연결된 정보만 저장하기 때문에 메모리를 효율적으로 사용
   - 하지만 특정한 두 노드가 연결되어 있는지에 대한 정보를 얻는 속도가 느리다 ⇒ 하나씩 확인해야 하므로

   ```python
   # 행(Row)이 3개인 2차원 리스트로 인접 리스트 표현
   graph = [[] for _ in range(3)]

   # 노드 0에 연결된 노드 정보 저장 (노드, 거리)
   graph[0].append((1, 7))
   graph[0].append((2, 5))

   # 노드 1에 연결된 노드 정보 저장 (노드, 거리)
   graph[1].append((0, 7))

   # 노드 2에 연결된 노드 정보 저장 (노드, 거리)
   graph[2].append((0, 5))

   print(graph)
   #[[(1, 7), (2,5)], [(0, 7)], [(0, 5)]]
   ```

### **DFS의 동작 과정**

1. 탐색 시작 노드를 스택에 삽입하고 방문처리를 한다
2. 스택의 최상단 노드에 방문하지 않은 인접 노드가 있으면 그 인접 노드를 스택에 넣고 방문 처리를 한다. 방문하지 않은 인접 노드가 없으면 스택에서 최상단 노드를 꺼낸다
3. 2번의 과정을 더 이상 수행할 수 없을 때까지 반복한다

> 아래 그림으로 DFS 동작과정 살펴보기

![dfs3](/assets/images/algorithm/dfs3.png)

1. 최상단 노드인 A를 스택에 삽입하고 방문 처리를 한다

   ⇒ STACK = [A]

2. 스택의 최상단 노드인 A에 방문하지 않은 인접 노드 B, C, D 중에서 가장 작은 노드인 B를 스택에 넣고 방문처리를 한다.

   ⇒ STACK = [A, B]

3. 스택의 최상단 노드인 B에 방문하지 않은 인접 노드 E, F가 있다. 이 중에서 가장 작은 노드인 E를 넣고 방문처리를 한다.

   ⇒ STACK = [A, B, E]

4. 스택의 최상단 노드인 E에 방문하지 않은 노드 I가 있다. 따라서 I 노드를 스택에 넣고 방문 처리를 한다.

   ⇒ STACK = [A, B, E, I]

5. 스택의 최상단 노드인 I에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 I 노드를 꺼낸다.

   ⇒ STACK = [A, B, E]

6. 스택의 최상단 노드인 E에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 E 노드를 꺼낸다.

   ⇒ STACK = [A, B]

7. 스택의 최상단 노드인 B에 방문하지 않은 노드 F가 있다. 따라서 F 노드를 스택에 넣고 방문 처리를 한다.

   ⇒ STACK = [A, B, F]

8. 스택의 최상단 노드인 F에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 F 노드를 꺼낸다.

   ⇒ STACK = [A, B]

9. 스택의 최상단 노드인 B에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 B 노드를 꺼낸다.

   ⇒ STACK = [A]

10. 스택의 최상단 노드인 A에 방문하지 않은 인접 노드 C, D가 있다. 이 중에서 가장 작은 노드인 C를 넣고 방문처리를 한다.

    ⇒ STACK = [A, C]

11. 스택의 최상단 노드인 C에 방문하지 않은 인접 노드 D, G가 있다. 이 중에서 가장 작은 노드인 D를 넣고 방문처리를 한다.

    ⇒ STACK = [A, C, D]

12. 스택의 최상단 노드인 D에 방문하지 않은 인접 노드 G, H가 있다. 이 중에서 가장 작은 노드인 G를 넣고 방문처리를 한다.

    ⇒ STACK = [A, C, D, G]

13. 스택의 최상단 노드인 G에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 G 노드를 꺼낸다.

    ⇒ STACK = [A, C, D]

14. 스택의 최상단 노드인 D에 방문하지 않은 노드 H가 있다. 따라서 H 노드를 스택에 넣고 방문 처리를 한다.

    ⇒ STACK = [A, C, D, H]

15. 스택의 최상단 노드인 H에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 H 노드를 꺼낸다.

    ⇒ STACK = [A, C, D]

16. 스택의 최상단 노드인 D에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 D 노드를 꺼낸다.

    ⇒ STACK = [A, C]

17. 스택의 최상단 노드인 C에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 C 노드를 꺼낸다.

    ⇒ STACK = [A]

18. 스택의 최상단 노드인 A에 방문하지 않은 인접 노드가 없다. 따라서 스택에서 A 노드를 꺼낸다.

    ⇒ STACK = [ ]

19. 결과적으로 노드의 탐색 순서(스택에 들어간 순서)는 다음과 같다.

    A → B → E → I → F → C → D → G → H

> DFS를 코드로 구현하면 다음과 같다.

```python
# DFS 함수 정의
def dfs(graph, v, visited):
    # 현재 노드를 방문 처리
    visited[v] = True
    print(v, end=' ')
    # 현재 노드와 연결된 다른 노드를 재귀적으로 방문
    for i in graph[v]:
        if not visited[i]:
            dfs(graph, i, visited)

# 각 노드가 연결된 정보를 리스트 자료형으로 표현(2차원 리스트)
graph = [
  [],
  [2, 3, 4], # [B, C, D]
  [1, 5, 6], # [A, E, F]
  [1, 4, 7], # [A, D, G]
  [1, 3, 7, 8], #[A, C, G, H]
  [2, 9], # [B, I]
  [2], # [B]
  [3, 4], #[C, D]
  [4], # [D]
  [5] # [E]
]

# 각 노드가 방문된 정보를 리스트 자료형으로 표현(1차원 리스트)
visited = [False] * 10

# 정의된 DFS 함수 호출
dfs(graph, 1, visited)

# 1 2 5 9 6 3 4 7 8
# A B E I F C D G H
```

## BFS

- Breadth First Search 너비 우선 탐색
- 가까운 노드부터 탐색하는 알고리즘
- DFS가 최대한 멀리 있는 노드를 우선으로 탐색하는 방식으로 동작했다면, BFS는 그 반대
- BFS 구현에서는 큐 자료구조를 이용하는 것이 정석

### BFS의 동작과정

1. 탐색 시작 노드를 큐에 삽입하고 방문 처리를 한다
2. 큐에서 노드를 꺼내 해당 노드의 인접 노드 중에서 방문하지 않은 노드를 모두 큐에 삽입하고 방문 처리를 한다
3. 2번의 과정을 더이상 수행할 수 없을 때까지 반복한다

> 아래 그림으로 BFS 동작과정 살펴보기

![dfs3](/assets/images/algorithm/dfs3.png)

1. 최상단 노드인 A를 스택에 삽입하고 방문 처리를 한다

   ⇒ QUEUE = [A]

2. 큐에서 노드 A를 꺼내고 방문하지 않은 인접 노드 B, C, D를 모두 큐에 삽입하고 방문 처리를 한다.

   ⇒ QUEUE = [B, C, D]

3. 큐에서 노드 B를 꺼내고 방문하지 않은 인접 노드 E, F를 모두 큐에 삽입하고 방문 처리를 한다.

   ⇒ QUEUE = [ C, D, E, F]

4. 큐에서 노드 C를 꺼내고 방문하지 않은 인접 노드 G를 큐에 삽입하고 방문 처리를 한다.

   ⇒ QUEUE = [ D, E, F, G]

5. 큐에서 노드 D를 꺼내고 방문하지 않은 인접 노드 H를 큐에 삽입하고 방문 처리를 한다.

   ⇒ QUEUE = [ E, F, G, H]

6. 큐에서 노드 E를 꺼내고 방문하지 않은 인접 노드 I를 큐에 삽입하고 방문 처리를 한다.

   ⇒ QUEUE = [ F, G, H, I]

7. 큐에서 노드 F를 꺼내고 방문하지 않은 인접 노드가 없으므로 무시한다.

   ⇒ QUEUE = [G, H, I]

8. 큐에서 노드 G를 꺼내고 방문하지 않은 인접 노드가 없으므로 무시한다.

   ⇒ QUEUE = [H, I]

9. 큐에서 노드 H를 꺼내고 방문하지 않은 인접 노드가 없으므로 무시한다.

   ⇒ QUEUE = [ I ]

10. 큐에서 노드 I를 꺼내고 방문하지 않은 인접 노드가 없으므로 무시한다.

    ⇒ QUEUE = [ ]

11. 남아있는 노드에 방문하지 않은 인접 노드가 없다. 따라서 모든 노드를 차례대로 꺼내면 된다.
12. 결과적으로 노드의 탐색 순서(큐에 들어간 순서)는 다음과 같다.

    A → B → C → D → E → F → G → H → I

> BFS를 코드로 구현하면 다음과 같다.

```python
from collections import deque

# BFS 함수 정의
def bfs(graph, start, visited):
    # 큐(Queue) 구현을 위해 deque 라이브러리 사용
    queue = deque([start])
    # 현재 노드를 방문 처리
    visited[start] = True
    # 큐가 빌 때까지 반복
    while queue:
        # 큐에서 하나의 원소를 뽑아 출력
        v = queue.popleft()
        print(v, end=' ')
        # 해당 원소와 연결된, 아직 방문하지 않은 원소들을 큐에 삽입
        for i in graph[v]:
            if not visited[i]:
                queue.append(i)
                visited[i] = True

# 각 노드가 연결된 정보를 리스트 자료형으로 표현(2차원 리스트)
graph = [
  [],
  [2, 3, 4], # [B, C, D]
  [1, 5, 6], # [A, E, F]
  [1, 4, 7], # [A, D, G]
  [1, 3, 7, 8], #[A, C, G, H]
  [2, 9], # [B, I]
  [2], # [B]
  [3, 4], #[C, D]
  [4], # [D]
  [5] # [E]
]

# 각 노드가 방문된 정보를 리스트 자료형으로 표현(1차원 리스트)
visited = [False] * 10

# 정의된 BFS 함수 호출
bfs(graph, 1, visited)

# 1 2 3 4 5 6 7 8 9
# A B C D E F G H I
```

# Ref.

- [이것이 취업을 위한 코딩테스트다 with 파이썬](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
- [Learning JavaScript Data Structures and Algorithms - The adjacency list](https://www.oreilly.com/library/view/learning-javascript-data/9781788623872/ef9a9b77-a6d4-480b-a4f4-77336f587b36.xhtml)

- [Learning JavaScript Data Structures and Algorithms - The adjacency matrix](https://www.oreilly.com/library/view/learning-javascript-data/9781788623872/8a7d3187-7c57-418c-a426-3aceab96f47f.xhtml)
