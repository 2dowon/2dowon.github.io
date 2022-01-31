---
layout: default
title: List, Set, Dict 자료형에 따른 시간 복잡도(Big-O)
last_modified_date: 2021-02-09 12:02:41
parent: Python
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">List, Set, Dict 자료형에 따른 시간 복잡도(Big-O)</div>

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

[백준 1920번](https://www.acmicpc.net/problem/1920) 문제를 풀다가 거의 똑같은 코드임에도 불구하고, 자료형에 따라 결과가 달라진다는 사실을 알고 자료형에 따른 시간 복잡도를 알아봐야겠다는 생각이 들었다. (백준 1920번 문제에서 list를 탐색했을 때는 시간 초과였지만, list에서 set 자료형으로 형변환 후 탐색했을 때는 통과했다.)

[파이썬의 주요 함수와 메서드의 시간 복잡도를 정리한 페이지](https://www.ics.uci.edu/~pattis/ICS-33/lectures/complexitypython.txt)를 찾아서 확인해본 결과, 삽입(insert, pop), 제거(delete, remove), 탐색(check ==, ≠), 포함 여부 확인(containment)의 경우 **List** 자료형에서는 모두 시간 복잡도 `$O(N)$`을 가지고 있고, **Set과 Dict**는 모두 `$O(1)$`의 시간 복잡도를 갖고 있다는 것을 확인할 수 있다.

따라서 삽입, 제거, 탐색, 포함 여부 확인과 같은 연산이 자주 필요하다면 List 자료형보다 Set이나 Dict 자료형을 사용하는 것이 시간 복잡도 측면에서 더 좋을 것이다.

## List

| Operation     | Example       | Big-O       | Notes                              |
| ------------- | ------------- | ----------- | ---------------------------------- |
| Index         | l[i]          | O(1)        |
| Store         | l[i] = 0      | O(1)        |
| Length        | len(l)        | O(1)        |
| Append        | l.append(5)   | O(1)        | mostly: ICS-46 covers details      |
| Pop           | l.pop()       | O(1)        | same as l.pop(-1), popping at end  |
| Clear         | l.clear()     | O(1)        | similar to l = []                  |
| Slice         | l[a:b]        | O(b-a)      | l[1:5]:O(l)/l[:]:O(len(l)-0)=O(N)  |
| Extend        | l.extend(...) | O(len(...)) | depends only on len of extension   |
| Construction  | list(...)     | O(len(...)) | depends on length of ... iterable  |
| check ==, !=  | l1 == l2      | O(N)        |
| Insert        | l[a:b] = ...  | O(N)        |
| Delete        | del l[i]      | O(N)        | depends on i; O(N) in worst case   |
| Containment   | x in/not in l | O(N)        | linearly searches list             |
| Copy          | l.copy()      | O(N)        | Same as l[:] which is O(N)         |
| Remove        | l.remove(...) | O(N)        |
| Pop           | l.pop(i)      | O(N)        | O(N-i): l.pop(0):O(N) (see above)  |
| Extreme value | min(l)/max(l) | O(N)        | linearly searches list for value   |
| Reverse       | l.reverse()   | O(N)        |
| Iteration     | for v in l:   | O(N)        | Worst: no return/break in loop     |
| Sort          | l.sort()      | O(N Log N)  | key/reverse mostly doesn't change  |
| Multiply      | k\*l          | O(k N)      | 5*l is O(N): len(l)*l is O(N\*\*2) |

## Set

| Operation      | Example       | Big-O            | Notes                                                      |
| -------------- | ------------- | ---------------- | ---------------------------------------------------------- |
| Length         | len(s)        | O(1)             |
| Add            | s.add(5)      | O(1)             |
| Containment    | x in/not in s | O(1)             | compare to list/tuple - O(N)                               |
| Remove         | s.remove(..)  | O(1)             | compare to list/tuple - O(N)                               |
| Discard        | s.discard(..) | O(1)             |
| Pop            | s.pop()       | O(1)             | popped value "randomly" selected                           |
| Clear          | s.clear()     | O(1)             | similar to s = set()                                       |
| Construction   | set(...)      | O(len(...))      | depends on length of ... iterable                          |
| check ==, !=   | s != t        | O(len(s))        | same as len(t); False in O(1) if the lengths are different |
| <=/<           | s <= t        | O(len(s))        | issubset                                                   |
| />=/>          | s >= t        | O(len(t))        | issuperset s <= t == t >= s                                |
| Union          | s             | t                | O(len(s)+len(t))                                           |
| Intersection   | s & t         | O(len(s)+len(t)) |
| Difference     | s - t         | O(len(s)+len(t)) |
| Symmetric Diff | s ^ t         | O(len(s)+len(t)) |
| Iteration      | for v in s:   | O(N)             | Worst: no return/break in loop                             |
| Copy           | s.copy()      | O(N)             |

## Dict

| Operation      | Example     | Big-O       | Notes                           |
| -------------- | ----------- | ----------- | ------------------------------- |
| Index          | d[k]        | O(1)        |
| Store          | d[k] = v    | O(1)        |
| Length         | len(d)      | O(1)        |
| Delete         | del d[k]    | O(1)        |
| get/setdefault | d.get(k)    | O(1)        |
| Pop            | d.pop(k)    | O(1)        |
| Pop item       | d.popitem() | O(1)        | popped item "randomly" selected |
| Clear          | d.clear()   | O(1)        | similar to s = {} or = dict()   |
| View           | d.keys()    | O(1)        | same for d.values()             |
| Construction   | dict(...)   | O(len(...)) | depends # (key,value) 2-tuples  |
| Iteration      | for k in d: | O(N)        | all forms: keys, values, items  |

# Ref.

- [complexitypython](https://www.ics.uci.edu/~pattis/ICS-33/lectures/complexitypython.txt)

- [TimeComplexity - Python Wiki](https://wiki.python.org/moin/TimeComplexity)

- [[Python] 파이썬 자료형 및 연산자의 시간 복잡도(Big-O) 총 정리](https://chancoding.tistory.com/43)

- [알고리즘의 분석, 공간 복잡도, 시간복잡도](https://debugdaldal.tistory.com/158)

- [파이썬 자료형 별 주요 연산자의 시간 복잡도 (Big-O)](https://wayhome25.github.io/python/2017/06/14/time-complexity/)
