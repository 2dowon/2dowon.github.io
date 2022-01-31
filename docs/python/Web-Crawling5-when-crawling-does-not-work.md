---
layout: default
title: 크롤링이 안될 때
last_modified_date: 2020-11-29 22:11:39
parent: Web Crawling
grand_parent: Python
---

# 크롤링이 안될 때

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

# 크롤링이 막혀있는 경우

간혹 웹사이트 중에서 크롤링을 막아 놓은 사이트들이 있다. 개인적으로 크롤링 연습을 위해 멜론 사이트를 크롤링하는데 계속 되지 않아서 requests로 url을 가져오자 `<Response [406]>` 상태코드를 확인할 수 있었다. [HTTP 상태코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)를 확인해보면 406은 Not Acceptable로 사용자 에이전트에서 정해준 규격에 따르지 않아 사이트에 접근할 수 없는 경우이다. 따라서 이 경우에 크롤링을 하기 위해서는 `User-agent` 헤더를 보내야 한다. 나는 로봇이 아니라 유저정보를 갖고 있는 사람일껄? 이라고 알려주는 역할을 한다.

```python
headers = {'User-Agent':'Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'}
url ='url 주소'
html = requests.get(url, headers = headers).text
```

- url를 입력하기 전에 미리 헤더를 보내야 한다
- requests로 url을 가져올 때 url 주소 외에도 `headers = headers` 를 써야 한다
