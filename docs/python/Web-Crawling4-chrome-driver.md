---
layout: default
title: Chrome driver
last_modified_date: 2020-11-28 00:11:23
parent: Web Crawling
grand_parent: Python
---

# Chrome driver

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

# Chrome driver 크롬 드라이버

동적 페이지를 크롤링하기 위해서는 chromedriver가 필요하다. chromedriver는 [여기](https://chromedriver.chromium.org/downloads)에서 본인의 크롬 브라우저에 맞는 버전으로 로컬디스크(c:)에 설치하면 된다. 하지만 chromedriver를 활용하는 프로그램을 만들어서 배포할 때는 보통 chromedriver를 코드 자체에서 자동으로 설치하는 것이 좋다.

## chromedriver 자동 설치하기

아래 코드를 이용하면 chromedriver를 수동으로 설치하지 않아도 된다.

```python
!pip install chromedriver_autoinstaller
```

```python
# 웹 드라이버 자동 설치 및 동작 확인

from selenium import webdriver as wd
import chromedriver_autoinstaller
import time

path = chromedriver_autoinstaller.install()
driver = wd.Chrome(path)

# 접속할 url
url = "https://www.naver.com"

# 접속 시도
driver.get(url)

# 홈페이지 뜰 때까지 대기
time.sleep(1)

# 웹 드라이버 닫기
driver.close()
# driver.quit()
```

## chromedriver가 화면에 보이지 않게 하고 싶은 경우

chromedriver를 실행하게 되면 크롬이 팝업창으로 뜨면서 작업을 수행하는 과정을 실시간으로 보여주게 되는데 만약 이 과정을 보고 싶지 않거나 보여주고 싶지 않은 경우에는 **headless** 옵션을 사용하면 된다.

⚠️ 단, 자동 로그인 등 크롬 창에서 무언가를 해야하는 경우에는 이 방법을 쓰면 확인할 수가 없다.

```python
from selenium import webdriver

options = webdriver.ChromeOptions()
options.add_argument('headless')
options.add_argument('window-size=1920x1080')
options.add_argument("disable-gpu")
# 혹은 options.add_argument("--disable-gpu")

driver = webdriver.Chrome('c:/chromedriver.exe', options=options)
```
