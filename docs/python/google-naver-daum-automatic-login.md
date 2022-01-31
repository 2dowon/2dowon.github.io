---
layout: default
title: 구글, 네이버, 다음 자동 로그인
last_modified_date: 2020-11-29 22:11:80
parent: Python
---

# 구글, 네이버, 다음 자동 로그인

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

# 구글,네이버,다음 자동로그인

자동로그인 코드를 만들기 위해서는 chromedriver와 selenum이 필요하다. 먼저 chromedriver는 [여기](https://chromedriver.chromium.org/downloads)에서 본인의 크롬 브라우저에 맞는 버전으로 로컬디스크(c:)에 설치하면 된다. selenum은 `!pip install selenium` 코드로 파이썬 환경에서 설치하면 된다.

자동 로그인을 하기 위해서는 다음으로 넘어가기 위해 중간 중간 클릭이 필요하다. selenium으로 클릭을 하기 위해서는 `XPath` 를 알아야 한다. XPath는 개발자 도구에서 클릭하고자 하는 요소를 선택한 후 오른쪽 버튼을 눌러 copy → copy XPath 를 누르면 XPath를 알 수 있다. 그 후 아래 코드를 이용하면 버튼을 자동으로 클릭할 수 있다.

```python
driver.find_element_by_xpath('xpath 복사해온 것').click()
```

# 구글 자동 로그인

구글을 자동 로그인하기 위해서는 **반드시** 2단계 인증을 풀고 진행해야 한다. 2단계 인증이 있는 계정의 경우, 아래 코드로 자동로그인을 할 수 없다. 구글 2단계 인증을 사용 중지 하는 법은 [여기](https://support.google.com/accounts/answer/1064203?co=GENIE.Platform%3DDesktop&hl=ko)에서 확인할 수 있다.

### 구글 웹 페이지에 접속하는 경우

```python
from selenium import webdriver
from time import sleep

driver = webdriver.Chrome('c:/chromedriver')
url = 'https://www.google.com/'

driver.implicitly_wait(3)

driver.get(url)

google_email = "구글 계정 이메일"  # 본인 이메일 계정을 입력해주세요
google_pw = "구글 계정 비밀번호" # 본인 비밀번호 입력해주세요

driver.find_element_by_xpath('//*[@id="gb_70"]').click()
driver.find_element_by_xpath('//input[@type="email"]').send_keys(google_email)
driver.find_element_by_xpath('//*[@id="identifierNext"]').click()
sleep(3)
driver.find_element_by_xpath('//input[@type="password"]').send_keys(google_pw)
driver.find_element_by_xpath('//*[@id="passwordNext"]').click()
sleep(2)
```

### 구글 로그인 창에 접속하는 경우

```python
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
import time

driver = webdriver.Chrome('C:/chromedriver')
driver.implicitly_wait(3)
driver.get('https://accounts.google.com/signin/v2/identifier?hl=ko&passive=true&continue=https%3A%2F%2Fwww.google.com%2F&ec=GAZAAQ&flowName=GlifWebSignIn&flowEntry=ServiceLogin')

driver.find_element_by_id('identifierId').send_keys('구글 계정 이메일') # 본인 이메일 계정을 입력해주세요
driver.find_element_by_xpath('//*[@id="identifierNext"]/div/button/div[2]').click()

time.sleep(1)

driver.find_element_by_name('password').send_keys('구글 계정 비밀번호') # 본인 비밀번호 입력해주세요
driver.find_element_by_xpath('//*[@id="passwordNext"]/div/button').send_keys(Keys.ENTER)

# driver.close()
```

# 네이버 자동 로그인

네이버 자동 로그인은 구글과 동일하게 진행할 경우, 보안을 위해서 자동입력방지 에러가 발생한다. 이를 해결하기 위해서는 자바 스크립트를 이용하면 된다.

```python
from selenium import webdriver

driver = webdriver.Chrome('c:/chromedriver')
url = 'https://www.naver.com/'
driver.get(url)

driver.find_element_by_xpath('//*[@id="account"]/a').click()

naver_id = "네이버 아이디"  # 본인 아이디를 입력해주세요
naver_pw = "네이버 비밀번호" # 본인 비밀번호 입력해주세요

# 일반적으로 사람이 작성하는 형태 => 네이버 보안에서 막힌다
# driver.find_element_by_id("id").send_keys(naver_id)
# driver.find_element_by_id("pw").send_keys(naver_pw)

# 네이버 보안에서 자바 스크립트를 통해서 보안을 뚫습니다 / 해킹이 될 수 있는 요소
driver.execute_script("document.getElementsByName('id')[0].value=\'"+naver_id+"\'")
driver.execute_script("document.getElementsByName('pw')[0].value=\'"+naver_pw+"\'")

driver.find_element_by_xpath('//*[@id="log.login"]').click()
```

### `pyperclip` 이용해서 자동로그인하기

pyperclip은 복사, 붙여넣기 클립보드 기능 사용을 위한 모듈이다. pyperclip을 사용하면 텍스트를 복사하고, 복사한 테스트를 붙여넣을 수 있기 때문에 아이디와 비밀번호를 코드에 직접 적지 않고, 따로 input으로 받아서 코드를 실행할 때마다 적을 수 있다.

```python
# pyperclip 설치하기
!pip install pyperclip
```

```python
from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.keys import Keys

# from bs4 import BeautifulSoup
import pyperclip
import time

def clipboard_input(self, user_xpath, user_input):
    pyperclip.copy(user_input) # input을 클립보드로 복사
    driver.find_element_by_xpath(user_xpath).click() # element focus 설정
    ActionChains(driver).key_down(Keys.CONTROL).send_keys('v').key_up(Keys.CONTROL).perform() # ctrl + v 전달

driver=webdriver.Chrome('C:\chromedriver.exe')

driver.get("https://nid.naver.com/nidlogin.login")
IDxPath='//*[@id="id"]'
PasswordxPath='//*[@id="pw"]'

# input으로 따로 입력을 받을 예정이므로 미리 적지 않아도 됩니다
ID=input("네이버 아이디: ")
Password=input("네이버 비밀번호: ")

clipboard_input(driver, IDxPath, ID)
clipboard_input(driver,PasswordxPath,Password)
driver.find_element_by_xpath('//*[@value="로그인"]').click()
```

# 다음 자동 로그인

### 다음 아이디를 통해 로그인하는 경우

```python
from selenium import webdriver
from time import sleep

driver = webdriver.Chrome('c:/chromedriver')
url = 'https://www.daum.net/'

driver.implicitly_wait(3)

driver.get(url)

daum_id = "다음 아이디" # 본인 아이디를 입력해주세요
daum_pw = "다음 비밀번호" # 본인 비밀번호를 입력해주세요

driver.find_element_by_xpath('//*[@id="inner_login"]/a[1]').click()
sleep(3)
driver.find_element_by_xpath('//input[@type="email"]').send_keys(daum_id)
driver.find_element_by_xpath('//input[@type="password"]').send_keys(daum_pw)
sleep(3)
driver.find_element_by_xpath('//*[@id="loginBtn"]').click()
```

### 카카오 로그인을 하는 경우

```python
from selenium import webdriver

driver = webdriver.Chrome('c:/chromedriver.exe')
url = 'https://accounts.kakao.com/login?continue=https%3A%2F%2Flogins.daum.net%2Faccounts%2Fksso.do%3Frescue%3Dtrue%26url%3Dhttps%253A%252F%252Fwww.daum.net%252F'
driver.get(url)

kakao_id = input('카카오 아이디를 입력하세요 : ')
kakao_pw = input('카카오 비밀번호를 입력하세요 : ')

# 다음 보안에서 자바 스크립트를 통해서 보안을 뚫습니다. / 해킹이 될 수 있는 요소
# 이 코드가 동작하는 조건은 리캡차가 동작하지 않는 홈페이지로 올라 왔을 때는 이 코드로 됨

driver.execute_script("document.getElementsByName('email')[0].value=\'"+kakao_id+"\'")
driver.execute_script("document.getElementsByName('password')[0].value=\'"+kakao_pw+"\'")

driver.find_element_by_xpath('//*[@id="login-form"]/fieldset/div[8]/button[1]').click()
```

### 위 코드로 자동 로그인을 하는 과정에서 '**로봇이 아닙니다**' 버튼이 생길 경우

```python
from selenium import webdriver
from selenium.webdriver.common.action_chains import ActionChains
from selenium.webdriver.common.keys import Keys

# from bs4 import BeautifulSoup
import pyperclip
import time

def clipboard_input(self, user_xpath, user_input):
    pyperclip.copy(user_input) # input을 클립보드로 복사
    driver.find_element_by_xpath(user_xpath).click() # element focus 설정
    ActionChains(driver).key_down(Keys.CONTROL).send_keys('v').key_up(Keys.CONTROL).perform() # ctrl + v 전달

driver=webdriver.Chrome('C:\chromedriver.exe')

driver.get("https://accounts.kakao.com/login?continue=https%3A%2F%2Flogins.daum.net%2Faccounts%2Fksso.do%3Frescue%3Dtrue%26url%3Dhttps%253A%252F%252Fwww.daum.net%252F")
IDxPath='//*[@id="id_email_2"]'
PasswordxPath='//*[@id="id_password_3"]'

# input으로 따로 입력을 받을 예정이므로 미리 적지 않아도 됩니다
ID=input("카카오 아이디: ")
Password=input("카카오 비밀번호: ")

clipboard_input(driver, IDxPath, ID)
clipboard_input(driver,PasswordxPath,Password)
driver.find_element_by_xpath('//*[@id="login-form"]/fieldset/div[8]/button[1]').click()
```
