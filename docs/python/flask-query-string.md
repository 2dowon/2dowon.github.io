---
layout: default
title: query string 값에 따른 html 렌더링
last_modified_date: 2021-02-03 12:02:15
parent: Flask
grand_parent: Python
---

# query string 값에 따른 html 렌더링

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

query string 값에 따른 html 렌더링할 때는 해당 url로 route하는 것은 의미가 없다. query string은 route에 영향을 주지 않기 때문이다. 그렇다면 어떻게 할 수 있을지에 앞서 query string과 route 개념을 간단히 정리하고 아래에서 마저 정리하려고 한다.

## query string

- query string 사용자가 입력 데이터를 전달하는 방법 중의 하나이다.
- URL에서 `?` 이후에 `=`으로 연결된 key, value 값을 query string라고 한다.
- 예를 들어서, 네이버에 강아지라고 검색을 하면 [https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=강아지](https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=강아지라는) 라는 URL을 얻을 수 있다. 이 때 search.naver? 뒤에 있는 모든 부분이 query string이다.
- query string을 flask에서 이용하기 위해서는 `request.args()` 를 이용해야 한다. request.args는 url파라미터의 값을 key=value 쌍으로 가지고 있는 딕셔너리이다.

## route

- 적절한 목적지를 찾아주는 기능
- URL 을 해당 URL 에 맞는 기능과 연결해 줌

  EX. http://0.0.0.0:5000/hello ⇒ http://0.0.0.0:5000 서버에서 hello라는 목적지에 맞는 함수를 호출

```python
@app.route("/hello")
def hello():
    return "<h1>Hello World!</h1>"
```

# query string 값에 따른 html 렌더링

Flask로 만든 웹페이지에서 내부 링크를 통해 `/?order_by=new` URL로 이동했을 때, 해당 URL과 관련된 html을 렌더링하려고 한다. 이 때 `@app.route("/?order_by=new")` 로 route하는 것은 의미가 없다.

**URL에서 `?` 이후로는 query string이라고 하는데, 이는 route에 영향을 주지 않아서 다 "/" 으로 인식하기 때문이다. 따라서 Flask에서 query string을 이용하기 위해서는 request.args를 사용해야 한다.**

request.args는 url파라미터의 값을 key=value 쌍으로 가지고 있는 딕셔너리이다. `request.args.get("key")` 를 통해 해당 key의 value값을 얻을 수 있다.

`/?order_by=new` URL에서 order_by=key, new=value이므로 `request.args.get('order_by')` 를 통해 해당 key인 order_by의 value값인 new를 얻을 수 있다.

> 기본적으로는 index.html을 보여주지만, `/?order_by=new` URL로 이동하면 nex_indx.html을 보여주고 싶다면

```python
from flask import request

@app.route("/")
def home():
    order = request.args.get('order_by')
    if order:
        if order == "new":
						return render_template("new_index.html")
		return render_template("index.html")
```

- home은 기본적으로 index.html을 렌더링한다.
- 만약 URL이 `/?order_by=new` 라면 `request.args.get('order_by')`를 통해 얻은 값이 `new` 이기 때문에 new.html을 렌더링한다.

# Ref.

- [Query String 쿼리스트링이란?](https://velog.io/@pear/Query-String-%EC%BF%BC%EB%A6%AC%EC%8A%A4%ED%8A%B8%EB%A7%81%EC%9D%B4%EB%9E%80)

- [파이썬 플라스크(flask)에서 url 파라미터로 값 입력받기!](https://tariat.tistory.com/755)
