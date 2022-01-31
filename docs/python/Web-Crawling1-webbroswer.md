---
layout: default
title: web broswer
last_modified_date: 2020-11-25 00:11:23
parent: Web Crawling
grand_parent: Python
---

# web broswer

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

# 웹 스크레이핑 / 크롤링

## 웹 스크레이핑(Web Scraping) / 웹 크롤링(Web Crawling) 이란

웹 스크레이핑은 컴퓨터 소프트웨어 기술을 활용해 웹 사이트 내에 있는 정보를 추출하는 것을 말한다. 웹 크롤링도 웹 스크레이핑과 비슷하지만, 웹 크롤링은 가능한 웹 사이트의 전체의 내용을 추출하는 것이라면 웹 스크레이핑은 웹 사이트의 내용을 가져와서 특정 데이터를 추출하는 것을 말한다. 굳이 비교하자면 이렇게 말할 수 있지만, 보통은 두 개념을 거의 같은 뜻으로 사용하는 것 같다.

## webbrowser 모듈

webbrowser 모듈을 이용하면 url을 통해 웹사이트에 접속할 수 있다. 하지만 웹 크롤링을 할 때는 보통은 url을 열기 위해 chromedriver를 사용하기 때문에 개인적으로 크롤링을 위해 webbroswer를 쓴 기억은 거의 없는 것 같다.

- 네이버에 접속하기

```python
import webbrowser

url = "www.naver.com"
webbrowser.open(url)
```

- 네이버에서 검색하기

```python
import webbrowser

naver_search_url = "http://search.naver.com/search.naver?query="
search_word = '파이썬'
url = naver_search_url + search_word

webbrowser.open_new(url)
```

- 네이버에서 여러 단어를 검색하기

```python
import webbrowser

google_url = "http://www.google.com/search?&q="
search_words = ['파이썬', "python web scraping", "python webbrowser"]

for search_word in search_words:
    webbrowser.open_new(google_url+search_word)
```
