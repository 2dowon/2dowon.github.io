---
layout: default
title: 정규표현식 (Regular Expressions)
last_modified_date: 2021-01-24 23:01:26
parent: Python
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">정규표현식 (Regular Expressions)</div>

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

정규표현식(Regular Expressions)은 복잡한 문자열을 처리할 때 사용하는 기법으로, 파이썬만의 고유 문법이 아니라 문자열을 처리하는 모든 곳에서 사용된다. 즉, 꼭 필요한 것은 아니지만 정규표현식을 알고 있다면 훨씬 더 간결하고 직관적인 코드를 작성할 수 있다.

정규표현식을 사용하기 위해서는 제일 먼저 re 모듈을 import 해야한다. re 모듈은 파이썬이 설치될 때 자동으로 설치되는 기본 라이브러리로 설치할 필요는 없다.

```python
import re
```

# 정규표현식에서 사용하는 메타문자

```
. & $ * ? { } [ ] \ | ( )
```

- **문자클래스 `[ ]`**

  : [ 와 ] 사이의 문자들과 매치. [ ] 사이에 하이픈(-)을 사용하면 두 문자 사이의 범위를 의미

- **Dot `.`**

  : 줄바꿈 문자인 `\n` 을 제외한 모든 문자와 매치됨을 의미

- **반복 `*`**

  : 바로 앞에 있는 문자가 0부터 무한대로 반복될 수 있다는 의미

- **반복 `+`**

  : 바로 앞에 있는 문자가 최소 1번 이상 반복될 때 사용

- **반복 `?`**

  : {0,1}을 의미 ⇒ 있거나 없거나

- **반복 `{m,n}`**

  : { } 메타 문자를 이용하면 반복 횟수를 고정 가능

  - `{m, n}` m번부터 n번까지
  - `{n}` n번 반복
  - `{m, }` m번 이상 / `{ ,n}` n번 이하를 의미

- **`|`**

  : or과 동일한 의미로 사용

- **`^`**

  : 문자열의 맨 처음과 일치함을 의미

- **`&`**

  : ^ 메타 문자와 반대의 경우로 문자열의 끝과 매치함을 의미

# 대표문자 (Meta sequence)

## 숫자 대표문자

- `\d`는 숫자를 대표하는 정규표현식
- 이때 d는 digit을 뜻한다

## 문자 대표문자

- `\w`는 글자를 대표하는 정규표현식
- `\w`는 `a, b, c, 가, 나, 다, 1, 2`와 같은 문자와 숫자를 포함한다.
- 특수문자는 포함하지 않지만, `_`(언더스코어)는 포함한다.

## 기타 대표문자

- `\s` 공백 문자(스페이스, 탭, 뉴라인)
- `\S` 공백 문자를 제외한 문자
- `\D` 숫자를 제외한 문자
- `\W` 글자 대표 문자를 제외한 글자들(특수문자, 공백 등)

# 횟수 정하기 (Quantifier)

## 하나 이상

- `\d` 나 `\w` 는 숫자나 글자 한 글자만 찾는다.
- 이때 한 개 이상의 연결된 숫자나 글자를 찾고 싶다면 `+` 를 이용하면 된다.
- `+`는 하나 혹은 그 이상 연결된이라는 뜻
- `\d+`는 하나 혹은 그 이상 연결된 숫자를 의미

## 0개 이상

- 만약 나올 수도 있고, 나오지 않을 수도 있는 경우 즉 0개 이상을 찾고 싶다면 `*` 를 이용하면 된다.
- `\d*`는 0개 이상의 연결된 숫자를 의미
- 이때 만약 자연수인 수를 찾고 싶다면 자연수가 0으로 시작할 수 없으므로 `[1-9]\d*`로 표현 가능

## 있거나 없거나

- `?`는 '있거나 없거나'라는 뜻

> 010-1234-5678 형태로 되어있는 전화번호를 찾는 정규표현식

- 따라서 `-?`는 -가 있거나 없다를 의미

```python
regex = r'\d+-?\d+-?\d+'

search_target = '''Luke Skywarker 02-123-4567 luke@daum.net
다스베이더 070-9999-9999 darth_vader@gmail.com
princess leia 010 2454 3457 leia@gmail.com'''

import re
result=re.findall(regex,search_target)
print(result)

# ['02-123-4567', '070-9999-9999', '010', '2454', '3457']
```

> 010 1234 5678처럼 공백으로 구분된 전화번호를 찾는 경우도 추가

- - 또는 (공백)이 있거나 없다는 조건은 `[- ]?`로 표현

```python
regex = r'\d+[- ]?\d+[- ]?\d+'

search_target = '''Luke Skywarker 02-123-4567 luke@daum.net
다스베이더 070-9999-9999 darth_vader@gmail.com
princess leia 010 2454 3457 leia@gmail.com'''

import re
result=re.findall(regex,search_target)
print(result)
# ['02-123-4567', '070-9999-9999', '010 2454 3457']
```

## n번

- `{숫자}`는 `숫자`번 반복한다는 뜻
- 예를 들어 `\d{2}`는 숫자가 연속 두 번 나온다는 뜻

## n~m번

- `{숫자1, 숫자2}`는 숫자1부터 숫자2까지 반복한다는 뜻
- 예를 들어, `\w{2,3}`는 문자가 2 ~ 3번 나온다는 뜻

> 다양한 형식의 전화번호를 찾는 정규표현식

```python
regex = r'\d{2,3}[- ]?\d{3,4}[- ]?\d{4}'

search_target = '''이상한 전화번호 0030589-5-95826
Luke Skywarker 02-123-4567 luke@daum.net
다스베이더 070-9999-9999 darth_vader@gmail.com
princess leia 010 2454 3457 leia@gmail.com'''

import re
result=re.findall(regex,search_target)
print(result)
# ['02-123-4567', '070-9999-9999', '010 2454 3457']
```

# 고르기

## 몇 개 중에 고르기

- 대괄호`[ ]` 안에 글자를 넣으면 해당 글자를 모두 선택 가능
- 예를 들어, 알파벳 중에 소문자 모음(a,e,i,o,u)만 고르고 싶다면 `[aeiou]`

## 범위에서 고르기

- `[범위시작-범위끝]` 은 범위 시작부터 범위 끝까지를 모두 선택하라는 의미
- 예를 들어 소문자 알파벳만 모두 고르고 싶다면 `[abcdefghijklmnopqrlstuvwxyz]`처럼 대괄호 안에 소문자를 모두 나열할 수도 있지만, 간단히 `[a-z]`를 써도 된다
- 알파벳 모두 ⇒ `[a-zA-Z]`
- 한글 단어 ⇒ `[가-힣]`
- 숫자 ⇒ `[0-9]`
- **TIP**. 연속된 영어 소문자 찾기 ⇒ `[a-z]+`

# Ref.

- [점프 투 파이썬](https://wikidocs.net/1669)
- [프로그래머스 정규표현식 강의](https://programmers.co.kr/learn/courses/11)
