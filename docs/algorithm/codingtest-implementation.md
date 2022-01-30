---
layout: default
title: 구현(Implementation)
parent: 이것이 취업을 위한 코딩테스트다 with 파이썬
grand_parent: Algorithm
permalink: /docs/algorithm/python-book/implementation/
---

| [이것이 취업을 위한 코딩테스트다 with 파이썬](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)를 읽고 정리한 내용입니다.

# 구현 Implementation

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

구현은 머릿속에 있는 알고리즘을 소스코드로 바꾸는 과정이다. 구현 유형에는 완전 탐색, 시뮬레이션이 있다. 완전 탐색은 모든 경우의 수를 주저 없이 다 계산하는 해결 방법이고, 시뮬레이션은 문제에서 제시한 알고리즘을 한 단계씩 차례대로 직접 수행해서 해결해야 한다.

사실 딱 구현이라고 봤을 때, 무슨 문제든 어떻게 풀지 생각하고 그것을 코드로 옮겨야 하기 때문에 쉽다고 생각했다. 그런데 막상 구현 문제를 풀어보니 모든 경우의 수를 다 고려해서 계산해야한다는 점이 생각보다 까다로웠다.

그리고 코딩 테스트의 경우 시간과 메모리 제한 조건이 있다. 메모리 제한에서는 1,000만 이상인 리스트가 있으면 풀 수 없게 되는 경우도 있으므로 조심하자. 또한 보통 1초의 시간 제한에서는 2,000만번의 연산을 수행할 수 있다고 한다. 사실 아직은 문제를 푸는 것이 더 우선순위라 시간과 메모리 제한 조건은 크게 고려하지 않고 있는데, 그래도 문제를 풀다 보면 종종 시간 초과가 되기 때문에 이 점을 유의해야겠다.

## 예제 문제

### 상하좌우 (p.110)

- A는 N X N 크기의 정사각형 공간 위에 있다.
- 가장 왼쪽 위 좌표는 (1, 1)이고, 가장 오른쪽 아래 좌표는 (N, N)이다.
- A는 상(U), 하(D), 좌(L), 우(R) 방향으로 이동할 수 있다.
- 시작 좌표는 항상 (1, 1)이다.
- N X N 크기의 정사각형 공간을 벗어나는 움직임은 무시된다.

> 5 X 5 크기의 공간에서 R, R, R, U, D, D순으로 움직였을 때 좌표 ⇒ (3, 4)

```python
def solution(n) :
    data_list = ["R", "R", "R", "U", "D", "D"]
    loc = [1, 1]

    for i in data_list :
        if i == "R" :
            loc[1] = loc[1] + 1
        elif i == "L" :
            if loc[1] > 1 :
                loc[1] = loc[1] - 1
        elif i == "U" :
            if loc[0] > 1 :
                loc[0] = loc[0] - 1
        else : # "D"
            loc[0] = loc[0] + 1

    x, y = loc[0], loc[1]
    print(x, y)

solution(5)
# 3 4
```

- 시작 좌표를 loc = [1, 1]로 설정한다. y좌표가 첫 번째 값, x좌표가 두 번째 값이다.

  ⇒ 이게 생각보다 헷갈린다.

- **모든 이동을 루프로 돌면서 하나씩 다 수행해야 한다**
- R이면 오른쪽 이동이므로 x좌표의 값이 1 증가해야 하므로 loc의 두 번째 값에 1을 더한다.
- 이렇게 L이면 두 번째 값에서 1을 빼고, U면 첫 번째 값에서 1을 빼고, D면 첫 번째 값에서 1을 더한다.
- 실제로 좌표를 그려보면 훨씬 이해가 빠르다.
- 모든 루프를 다 돈 후, 최종적으로 loc의 첫 번째와 두 번째 값을 리턴하면 된다.

### 시각 (p.113)

- 정수 N이 입력되면 00시 00분 00초부터 N시 59분 59초까지의 모든 시각 중에서 3이 하나라도 포함되는 모든 경우의 수를 구하는 프로그램

> 5를 입력하면 00시 00분 00초부터 5시 59분 59초까지의 모든 시각 중에서 3이 하나라도 포함되는 경우의 수를 출력 ⇒ 11475

```python
def solution(n) :
    time = [0,0,0]
    answer = 0

    for i in range(n+1) :
        time = [i, 0, 0]
        for j in range(60) :
            time = [i, j, 0]
            for k in range(60) :
                time = [i, j, k]
                time_str = str(i) + str(j) + str(k)
                if '3' in list(time_str) :
                    answer = answer + 1

    return answer

solution(5)
# 11475
```

- 처음에는 000000부터 시작해 N0000를 구해서 그 안에서 3이 포함되는 경우의 수를 셌는데, 그 경우 59분 59초를 넘는 경우가 생기기 때문에 적합하지 않았다
- 이 문제는 **시, 분, 초로 나눠서 총 3개의 반복문**을 이용하면 계산할 수 있다.
- 시간은 주어지기 때문에 n만큼 반복하면 되고, 분이랑 초는 0부터 59까지 반복하면 된다.
- 3개의 반복문 안의 끝에서 숫자에 3이 있다면 하나씩 횟수를 세면 된다.

## 실전 문제

### 왕실의 나이트 (p.115)

- 왕실 정원은 8 X 8 좌표 평면이다
- 왕실 정원의 특정한 한 칸에는 나이트가 서 있다. 나이트는 아래처럼 2가지 경우로 이동할 수 있다.
  1. 수평으로 두 칸 이동한 뒤에 수직으로 한 칸 이동하기
  2. 수직으로 두 칸 이동한 뒤에 수평으로 한 칸 이동하기
- 왕실 정원의 행 위치는 1부터 8로 표현하고, 열 위치는 a부터 h로 표현한다
- 만약 나이트가 좌표 평면을 벗어나게 되면 이동할 수 없다
- 이동할 수 있는 경우의 수를 출력

> a1에 위치한 나이트가 이동할 수 있는 경우의 수 ⇒ 2

```python
def solution(n) :
    steps = [(-2, -1), (-1, -2), (1, -2), (2, -1), (2, 1), (1, 2), (-1,2 ), (-2, 1)]
    rows = ["1", "2", "3", "4", "5", "6", "7", "8"]
    columns = ["a", "b", "c", "d", "e", "f", "g", "h"]
    loc = list(n)
    loc_r = rows.index(loc[1]) + 1
    loc_c = columns.index(loc[0]) + 1
    count = 0

    for i in steps :
        next_r = loc_r + i[1]
        next_c = loc_c + i[0]
        if 1 <= next_r <= 8 and 1 <= next_c <= 8 :
            count = count + 1

    return count

solution("a1")
```

- 이동할 수 있는 모든 경우의 수를 체크한다.
  ⇒ (-2, -1), (-1, -2), (1, -2), (2, -1), (2, 1), (1, 2), (-1,2 ), (-2, 1)
- 입력받은 위치인 a1을 하나씩 분리해서 리스트에 넣기 위해 리스트로 형 변환
- index로 rows, colums에서 해당 위치를 찾아서 1을 더해준다. 위치는 1부터 시작하는데, index는 0부터 시작하기 때문.
- 이동할 수 있는 모든 경우의 수를 모두 반복하는데, 이때 이동한 위치가 8X8 범위 안에 있다면 횟수를 세고, 벗어난다면 무시

> 책 풀이 - 아스키 코드 활용 👍

```python
# 현재 나이트의 위치 입력받기
input_data = input()
row = int(input_data[1])
column = int(ord(input_data[0])) - int(ord('a')) + 1 # 아스키 코드를 활용

# 8가지 방향에 대하여 각 위치로 이동이 가능한지 확인
steps = [(-2, -1), (-1, -2), (1, -2), (2, -1), (2, 1), (1, 2), (-1,2 ), (-2, 1)]

result = 0
for step in steps :
    # 이동하고자 하는 위치 확인
    next_row = row + step[0]
    next_column = column + step[1]
    # 해당 위치로 이동이 가능하다면 카운트 증가
    if 1 <= next_row <= 8 and 1 <= next_column <= 8 :
        result = result + 1

print(result)
```

- [아스키 코드](https://ko.wikipedia.org/wiki/ASCII)를 활용하는 것은 진짜 생각도 못했는데, 다음에 알파벳이 등장하면 그때는 꼭 아스키 코드를 생각해봐야겠다.

### 게임 개발 (p.118)

- 게임의 맵은 N X M 크기의 직사각형이다. 맵의 각 칸은 (A, B)로 나타낼 수 있다.
- 각각의 칸은 육지(0) 또는 바다(1)이다
- 캐릭터는 동(1)서(2)남(3)북(0) 중 한 곳을 바라본다
- 캐릭터는 상하좌우로 움직일 수 있고 바다로 되어 있는 공간에는 갈 수 없다.
- 캐릭터는 다음 매뉴얼로 움직인다.
  1. 현재 위치에서 현재 방향을 기준으로 왼쪽 방향부터 차례대로 갈 곳을 정한다
  2. 캐릭터의 왼쪽 방향에 아직 가보지 않은 칸이 있다면, 왼쪽 방향으로 회전한 다음 왼쪽으로 한 칸 전진한다. 가보지 않은 칸이 없다면 왼쪽 방향으로 회전만 하고 1단계로 돌아간다.
  3. 네 방향 모두 이미 가본 칸이거나 바다라면 바라보는 방향을 유지한 채로 한 칸 뒤로 가고 1단계로 돌아간다. 만약 뒤쪽 방향이 바다라서 뒤로 갈 수 없다면 움직임을 멈춘다.
- 캐릭터가 방문한 칸의 총 수를 출력하자

> 4 X 4의 맵에서 캐릭터는 (1,1) 칸에 있고 북쪽(0)을 바라보고 있다.
> 맵 ⇒ 첫 줄 : 1 1 1 1, 둘째 줄 : 1 0 0 1, 셋째 줄 : 1 1 0 1, 넷째 줄 : 1 1 1 1 일때
> ⇒ 총 3칸을 움직인다

```python
# N, M을 공백으로 구분하여 입력받기
n, m = map(int, input().split())

# 방문한 위치를 저장하기 위한 맵을 생성하여 0으로 초기화
d = [[0] * m for _ in range(n)]
# 현재 캐릭터의 X좌표,Y좌표, 방향을 입력받기
x, y, direction = map(int, input().split())
d[x][y] = 1 # 현재 좌표 방문 처리

# 전체 맵 정보를 입력받기
array = []
for i in range(n) :
    array.append(list(map(int, input().split())))

# N(북), E(동), S(남), W(서)에 따른 이동 방향
dx = [-1, 0, 1, 0]
dy = [0, 1, 0, -1]

# 왼쪽으로 회전
def turn_left():
    global direction
    direction = direction - 1
    if direction == -1 :
        direction = 3

# 시뮬레이션 시작
count = 1
turn_time = 0
while True:
    # 왼쪽으로 회전
    turn_left()
    nx = x + dx[direction]
    ny = y + dy[direction]
    # 회전한 이후 정면에 가보지 않은 칸이 존재하는 경우 이동
    if d[nx][ny] == 0 and array[nx][ny] == 0 :
        d[nx][ny] = 1
        x = nx
        y = ny
        count = count + 1
        turn_time = 0
        continue
    # 회전한 이후 정면에 가보지 않은 칸이 없거나 바다인 경우
    else :
        turn_time = turn_time + 1
    # 네 방향 모두 갈 수 없는 경우
    if turn_time == 4 :
        nx = x - dx[direction]
        ny = y - dy[direction]
        # 뒤로 갈 수 있다면 이동하기
        if array[nx][ny] == 0:
            x = nx
            y = ny
        # 뒤가 바다로 막혀있는 경우
        else :
            break
        turn_time = 0

print(count)
```

- 방향을 설정해서 이동하는 문제 유형에서는 dx, dy라는 별도의 리스트를 만들어 방향을 정하는 것이 효과적
- 방문한 위치를 저장하기 위해서 2차원 리스트를 0으로 초기화하여 생성
- 맵 정보를 입력하기 위한 2차원 리스트도 따로 만들기
- 캐릭터의 방향을 알려주는 direction 변수도 input으로 받는다. turn_left 함수에서 direction 변수를 사용하기 위해 global로 선언한다. ⇒ direction 변수가 함수 바깥에서 선언된 전역변수이기 때문

# Ref.

- [이것이 취업을 위한 코딩테스트다 with 파이썬](https://www.hanbit.co.kr/store/books/look.php?p_code=B8945183661)
