---
layout:     post
title:      "Selenium 으로 Crawling 하기 "
subtitle:   "Selenium 으로 슈퍼 크롤러 되기."
date:       2017-10-03 02:03:00
author:     "MinJun"
header-img: "img/tags/ComputerScience1-bg.jpg"
comments: true
tags: [Python]
---


## 준비물 

- Python3, Selenium, ChromeWebDriver(다른 브라우져에서 사용하는 해당 브라우져 드라이브를 사용하면 됩니다. 저는 chrome드라이브를 사용했습니다.)

**[how to install Python](/img/posts/How-to-install-python.pdf)**

## Selenium 사용 환경 구성 하기

 - Selenium 설치
 
**터미널에서**
>
> pip install selenium 
> 

 - WebDriver

Selenium은 webdriver라는 것을 통해 디바이스에 설치된 브라우저들을 제어할 수 있습니다.(사실 크롬을 사용하는것은 별로 좋지 않다, 여러가지 제약이 있기 때문이다(로봇 검색이 엄격합니다.. 때문에 로봇에 관대한(?) 브라우저를 찾아봅시다) fireFox나 등등..구글링을 통해서 robot 보안이 좋지않은(?) 브라우저를 금방 찾을수 있다)

**Chorme 드라이버 다운로드 링크이다. 링크 선택후, 자신의 OS 선택하고 설치해주자**
<https://sites.google.com/a/chromium.org/chromedriver/downloads>


## 실습 

쉬운것 부터 해보자, 네이버에 자동으로 로그인 할수 있게 만들어보자.

*저는 환경구성을 잘못한 탓인지, IDLE 에서는 import 가 되지 않았는데, jupyter notebook 에서는 잘 실행됬습니다..*

Selenium은 webdriver api를 통해서 브라우저를 제어 합니다. 우선 webdriver를 import를 해줍니다.

```python

import os

from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC 
from selenium.webdriver.common.by import By

# driver 를 설정합니다.
driver = webdriver.Chrome()

# api 가 처음 접근하는 url 을 설정합니다.
# 네이버 로그인 페이지로 접근합니다. 자세한 url은, 크롬 `command + option + i` 키를 누르면 개발자 모드로 변경되어서, 사용하고 싶은 부분의 HTML을 긁어올수 있습니다.
driver.get('https://nid.naver.com/nidlogin.login')

# url 접근후 3초간 기다려줍니다. 이유는 명령어에 접근하는 시간이랑, 실제로 코드가 적용되는 시간이 차이가 있어서, 컴퓨터가 더 빠르면(?) 다음 명령어가 씹히는 경우도 있습니다.
driver.implicitly_wait(3)

# 'naver_id' 에 naverID 를, 'password' 에 비밀번호를 입력하면, 자동으로 robot이 입력을 해준다. 

driver.find_element_by_name('id').send_keys('naver_id')
driver.find_element_by_name('pw').send_keys('password')

# 로그인 버튼을 클릭 해줍니다.

driver.find_element_by_xpath('//*[@id="frmNIDLogin"]/fieldset/input').click()




```

 - 네이버 접속 화면(위의 URL 을 한번 확인해 보자.)
 
![screen](/img/posts/naverLogin.jpg)

> 위의 코드를 작성하고 실행 하면, 자동으록 로그인 하는것을 확인 할수 있습니다.



## 응용

 - 실시간 검색어에 이름올려보기
 
> Chrome 가 아닌, 다른 브라우저를 사용하는것을 추천합니다. Chrome 로 하게되면, robot 필터에 걸러지는것을 확인 하실수 있습니다. 


```python

import os
import time
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC 
from selenium.webdriver.common.by import By


driver = webdriver.Chrome()
driver.get("https://www.naver.co.kr/")
btn = 0

# 웹드라이브

# naver 에서 검색값 id = query
query = driver.find_element_by_id("query") 
 
# 검색창에 적어 줍니다.  
query.send_keys("MJ.Dev") 

# 검색하기 전 5초를 기다린다 
ff_driver.implicitly_wait(5)

# 초기 검색 버튼.
ff_driver.find_element_by_id("search_btn").click()

# 반복 검색 
while btn < 100 :
	  btn += 1
	  
	  # robot 의 움직임이 5초 멈춤
    ff_driver.implicitly_wait(5)
    
    # 코드의 흐름이 다음줄이 되면 10초가 멈춘후, 다음코드 실행
    time.sleep(10)
    
    # 새로운 검색창 검색값 id = bt_search
    ff_driver.find_element_by_class_name("bt_search").click()


```

> 원리는, 약 10초씩 같은 이름을 검색하게 만들었습니다. 사실 이렇게 사용하면, 같은 이름을 검색해서 robot 필터에 걸립니다... 피하는 방법에 대해서 자세하게 적고 싶지만.. 뭔가 적으면 안될것 같은(?) 생각에 작성하지는 않았습니다. 조금만 고민 하면서 사용하면, Selenium은 막강한 도구가 될것 같습니다..! 








