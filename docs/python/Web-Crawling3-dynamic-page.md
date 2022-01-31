---
layout: default
title: 동적 페이지 크롤링
last_modified_date: 2020-11-26 00:11:23
parent: Web Crawling
grand_parent: Python
---

# 동적 페이지 크롤링

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

# 동적 페이지 크롤링 하기

동적 페이지를 크롤링하기 위해서는 chromedriver와 selenum이 필요하다. 먼저 chromedriver는 [여기](https://chromedriver.chromium.org/downloads)에서 본인의 크롬 브라우저에 맞는 버전으로 로컬디스크(c:)에 설치한다. selenum은 `!pip install selenium` 코드로 파이썬 환경에서 설치하면 된다.

## 페이지에 접속하기

`driver.get(URL)`

- 특정 URL에 접속하는 명령

```python
from selenium import webdriver

driver = webdriver.Chrome('c:/chromedriver')

url = 'https://www.naver.com/'
driver.get(url) # chromedriver로 네이버 홈페이지에 접속
```

`driver.close()`

- chromedriver 창이 알아서 꺼지기를 원할 때 사용

`implicitly_wait(n)`

- n초 이내에 들어오면 켜달라는 뜻
- 위치가 중요하다! `get(url)` 전에 써야 한다

```python
from selenium import webdriver

driver = webdriver.Chrome('c:/chromedriver')

url = 'https://www.naver.com/'
driver.implicitly_wait(3)
driver.get(url)
```

`time.sleep(n)`

- n초 이후에 화면이 들어오도록
- 페이지가 로드되는 시간을 따로 주지 않으면 페이지가 로딩되기 전에 코드를 실행해서 제대로 동작하지 않는 경우가 생긴다

```python
from selenium import webdriver
import time # time 라이브러리 임포트하기

driver = webdriver.Chrome('c:/chromedriver')

url = 'https://www.naver.com/'
time.sleep(3)
driver.get(url)
```

## 웹 페이지에서 HTML 가져오기

```python
html = driver.page_source
```

- 해당 웹 페이지의 html 을 가져올 수 있다

- html을 가져온 후에는 정적 페이지에서 동일하게 BeautifulSoup 라이브러리의 `find`와 `select`를 이용해 추출하고자 하는 요소를 선택하면 된다. `find` 와 `select`를 통해 어떻게 추출하는지는 [여기](https://2dowon.netlify.app/python/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EC%9B%B9-%ED%81%AC%EB%A1%A4%EB%A7%812---%EC%A0%95%EC%A0%81-%ED%8E%98%EC%9D%B4%EC%A7%80-%ED%81%AC%EB%A1%A4%EB%A7%81/) 참고!

- ✅ Selenium을 이용해 동적 페이지를 반복해서 크롤링할 경우에는 페이지가 계속 변화하므로 `driver.page_source` 를 이용해야 변한 페이지의 정보를 크롤링해올 수 있다
