---
layout: default
title: 정적 페이지 크롤링
last_modified_date: 2020-11-25 00:11:23
parent: Web Crawling
grand_parent: Python
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">정적 페이지 크롤링</div>

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

# 정적 페이지 크롤링하기

웹 크롤링을 하는 경우 대부분은 정적 페이지를 크롤링하는 경우가 많다. 이때 정적 페이지는 말 그대로 움직이지 않는 페이지를 말한다. 버튼을 누르는 등의 동적인 행동이 없는 경우에 정적 페이지라 하고 정적 페이지를 크롤링하기 위해서는 주로 requests와 BeautifulSoup 를 많이 사용한다.

## 웹 페이지의 HTML 소스 갖고 오기

웹 페이지의 HTML 소스를 가져오기 위해서는 requests와 BeautifulSoup 라이브러리가 필요하다.

### requests 라이브러리

[Requests: HTTP for Humans™ - Requests 2.25.0 documentation](https://requests.readthedocs.io/en/master/)

> 기본 예시

```python
import requests

html = requests.get("https://www.google.co.kr")
html
html.text[0:100]

# '<!doctype html><html itemscope="" itemtype="http://schema.org/WebPage" lang="ko"><head><meta content'
```

- 접속이 잘 된 경우에는 `<Response [200]>` 을 확인할 수 있다
- 그 외 HTTP 상태코드는 [이 곳](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)에서 확인할 수 있다
- `request.get()` 을 이용해 가져온 html 소스 뒤에 `.text` 를 붙이면 소스 코드를 확인할 수 있다

### BeautifulSoup 라이브러리

> 기본 예시

```python
from bs4 import BeautifulSoup

# 테스트용 html 코드
html = **"""**<html><body><div><span>**\**
        <a href=http://www.naver.com>naver</a>**\**
        <a href=https://www.google.com>google</a>**\**
        <a href=http://www.daum.net/>daum</a>**\**
        </span></div></body></html>**"""**

# soup = BeautifulSoup(html, 'html.parser')
soup = BeautifulSoup(html, 'lxml')
soup

# <html><body><div><span> <a href="http://www.naver.com">naver</a> <a href="https://www.google.com">google</a> <a href="http://www.daum.net/">daum</a> </span></div></body></html>
```

### parsing 파싱

HTML 코드를 분석하기 위해서는 HTML 코드 구문을 이해하고 요소별로 HTML 코드를 분류해야 한다. HTML 소스를 처리하는 파서(parser)에는 html.parser와 lxml이 있다. lxml이 html.parser에 비해 좀 더 빠르기 때문에 lxml을 쓰는 것을 추천한다.

```python
soup = BeautifulSoup(html, 'html.parser')
soup = BeautifulSoup(html, 'lxml')
```

### prettify()

HTML 소스를 HTML 구조의 형태로 예쁘게 확인하고 싶을 때 사용한다.

```python
print(soup.prettify())

<html>
 <body>
  <div>
   <span>
    <a href="http://www.naver.com">
     naver
    </a>
    <a href="https://www.google.com">
     google
    </a>
    <a href="http://www.daum.net/">
     daum
    </a>
   </span>
  </div>
 </body>
</html>
```

## 원하는 요소 찾기

requests와 BeautifulSoup를 통해 html을 가져오고 parsing을 통해 html 코드를 분류했다면 이제는 크롤링하고자 하는 요소를 찾아야 한다. 이를 찾기 위해서는 크롬 브라우저를 기준으로 오른쪽 버튼 -> 검사 버튼을 이용해 개발자 도구에 접근해야 한다. 그래서 크롤링하고자 하는 요소의 ID, class 등 그 요소의 구조, 특징을 찾아 find, selector 를 이용하면 크롤링할 수 있다.

### find

- `**soup.find("태그")**`
  : HTML 소스코드에서 해당 "태그"가 있는 **첫 번째 요소를 찾아서** 반환

      ```python
      soup.find("a")

      # <a href="http://www.naver.com">naver</a>
      ```

- `**soup.find("태그").get_text()**`
  : HTML 소스코드에서 해당 "태그"가 있는 첫 번째 요소 하나만 찾아서 **텍스트 부분만** 추출할 때 이용

      ```python
      soup.find("a").get_text()

      # naver
      ```

- `**soup.find_all("태그")**`
  : HTML 소스코드에서 해당 "태그"가 있는 모든 요소를 찾아서 **리스트로 반환**

      ```python
      soup.find_all("a")

      # [<a href="http://www.naver.com">naver</a>,
      #  <a href="https://www.google.com">google</a>,
      #  <a href="http://www.daum.net/">daum</a>]
      ```

- 여러 태그에서 텍스트 부분만 추출하기 ⇒ for문 이용

  ```python
  site_names = soup.find_all('a')
  for site_name in site_names:
      print(site_name.get_text())

  # naver
  # google
  # daum
  ```

- class나 id 속성을 이용해 찾고 싶을 때

  ```python
  a = soup.find('태그', {"class":"class_속성값"})
  b = soup.find('태그', {"id":"id_속성값"})
  ```

### CSS 선택자(selector)

- **태그 안에 속성이 class인 경우 `태그.class_속성값`
  속성이 id인 경우 `태그#id_속성값`**
- `soup.select("태그 및 속성")`

  : HTML 소스코드에서 해당 "태그 및 속성"이 있는 모든 요소를 찾아서 **리스트로 반환**

  ```python
  soup2.select("body h1")
  # [<h1>책 정보</h1>]
  ```

- `soup.select_one("태그 및 속성")`

  : HTML 소스코드에서 해당 "태그 및 속성"이 있는 **첫 번째 요소를 찾아서** 반환

- `soup.select_one("태그 및 속성").text`

  : HTML 소스코드에서 해당 "태그 및 속성"이 있는 첫 번째 요소 하나만 찾아서 **텍스트 부분만** 추출할 때 이용

- 여러 태그에서 텍스트 부분만 추출하기 ⇒ for문 이용

  ```python
  site_names = soup.select('a')
  for site_name in site_names:
      print(site_name.text)

  # naver
  # google
  # daum
  ```

✅ 꼭 알아두기!

- `>` 부모-자식

- `띄어쓰기` 부모-자손

⇒ **CSS 선택자**에서는 **띄어쓰기가 하위요소**라는 뜻

⇒ 띄어쓰기가 더 큰 개념 (`>` 는 바로 아래줄, `띄어쓰기`는 더 아래줄까지 포함)

ex.

- `soup.select("html body a")`

  soup에서 html 안에 있는 body 안에 있는 a 태그를 선택

- `soup.select('#fruits1 > span.name')`

  soup에서 html 안에 있는 fruits1이라는 아이디 바로 아래에 있는 span태그 중에서 클래스가 name인 요소를 선택

- `a.portal` 같은 줄에 쓰여져 있다
- `a .portal` 다른 줄에 쓰여져 있다

### 텍스트만 깔끔하게 추출하기 위해서

- `strip()`
  : 텍스트 앞 뒤로 개행문자가 있는 경우에 사용

      ```python
      a = "\nabc\n"
      print(a)

      #
      # abc
      #

      b = a.strip()
      print(b)
      # abc
      ```

- `replace("old", "new")`

  : old를 new로 바꿔준다

  ```python
  a = "a,b,c"
  a.replace(",", "")
  # "abc"
  ```

## 정적 웹 페이지에서 이미지 가져오기

### 하나의 이미지 내려받기

```python
import requests

# 이미지 주소 가져오기
url = 'https://www.python.org/static/img/python-logo.png'
html_image = requests.get(url)

import os

image_file_name = os.path.basename(url)

# 이미지를 다운받을 폴더 경로 지정하기
folder = 'C:/myPyCode/download'

# 이미지를 다운받을 폴더가 없는 경우에는 새 폴더 생성하기
if not os.path.exists(folder):
    os.makedirs(folder)

# 이미지 경로 지정하기 = 폴더 경로 + 이미지 파일 이름
image_path = os.path.join(folder, image_file_name)

# 이미지 데이터를 1000000 바이트씩 나눠서 내려받고 파일에 순차적으로 저장
chunk_size = 1000000
for chunk in html_image.iter_content(chunk_size):
    imageFile.write(chunk)
imageFile.close()

# 파일 잘 다운받았는지 확인하기
os.listdir(folder)

```

- 이미지를 한 번에 다운받으면 오류가 났을 때 문제가 생길 수 있기 때문에 나눠서 다운받는다
  chunk_size는 네트워크 속도에 따라 결정하면 된다

### reshot에서 이미지 여러 개 내려받기

```python
import requests
from bs4 import BeautifulSoup
import os

# 원하는 이미지들이 있는 url에서 HTML 소스 가져오기
URL = 'https://www.reshot.com/search/animal'
html_reshot_image = requests.get(URL).text
soup_reshot_image = BeautifulSoup(html_reshot_image, "lxml")

# 다운받고자 하는 이미지 요소들을 선택하기
reshot_image_elements = soup_reshot_image.select('a img')

# 이미지 주소에 해당하는 것만 추출하기
# 이미지 태그 중 세 번째에 해당하는 주소만 가져옴
# => 여러개의 이미지를 다운받기 위해서는 이때 for문을 이용하기
reshot_image_url = reshot_image_elements[2].get('src')

html_image = requests.get(reshot_image_url)

# 이미지를 다운받을 폴더 경로 지정하기
**f**older = "C:/myPyCode/download"

# 이미지를 다운받을 폴더가 없는 경우에는 새 폴더 생성하기
if not os.path.exists(folder):
    os.makedirs(folder)

# os.path.basename(URL)는 웹사이트나 폴더가 포함된 파일명에서 파일명만 분리하는 방법
imageFile = open(os.path.join(folder, os.path.basename(reshot_image_url)), 'wb')

# 이미지 데이터를 1000000 바이트씩 나눠서 저장하는 방법
chunk_size = 1000000
for chunk in html_image.iter_content(chunk_size):
    imageFile.write(chunk)
imageFile.close()

```
