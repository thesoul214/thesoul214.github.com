---
layout: post
title:  "Python + Selenium을 이용하여 chrome 자동화 - 기본 사용법 및 html 요소 조작 관련 정리"
description: "selenium, web driver, google, python, chrome"
date:   2019-05-30 12:00:00 +0800
categories: PYTHON
tags: Scraping python selenium
lang: ko_KR
comments: true
---


Python, Chrome, Selenium을 이용하여 html 태그 취득, 마우스 클릭 등의 UI조작과 관련된 기본적인 내용을 정리하겠습니다.


<br>
> ##### headless 모드 기본

- {:.p-header} headless 샘플 코드

   ~~~python
   from selenium import webdriver

   options = webdriver.ChromeOptions()
   options.add_argument('--headless')
   driver = webdriver.Chrome(options=options)

   driver.get('https://www.mansion-review.jp/shinchiku/prefecture/34.html')
   print(driver.title)
   # 広島県の新築マンションランキング　69物件|新築マンションレビュー
   driver.quit()
   ~~~

   webdriver 옵션으로 headless를 지정하면, chrome 동작이 눈에 보이지 않고 백그라운드에서 동작하게 됩니다. 

<br>
> ##### html 요소 취득 방법

- {:.p-header} By CSS
  
   ~~~html
   <span class="rli_title">
      <a target="_blank" href="https://www.mansion-review.jp/mansion/707867.html">
         シティタワー広島
      </a>
   </span>
   ~~~
   
   획득한 html에 위와 같은 태그가 있는 경우
   ~~~python
   for name in driver.find_elements_by_css_selector("span.rli_title"):
      print(name.text)
   ~~~
   find_elements_by_css_selector 함수로 해당 태그를 지정할 수 있습니다.

   .text 함수로는 innerText값을 획득 가능합니다.

<br>
- {:.p-header} By ID
  
   ID로 html 태그를 얻어오고자 할 때는 하기의 함수를 사용할 수 있습니다.

   ~~~python
   from selenium.webdriver.common.by import By

   driver.find_element(by=By.ID, value="<취득하고자 하는 요소 ID>")
   driver.find_element_by_id("<취득하고자 하는 요소 ID>")
   ~~~

   또한 element 대신에 elements를 사용함으로써 해당하는 모든 html 태그를 리스트 형으로 취득할 수도 있습니다.
   ~~~python
   driver.find_elements(by=By.ID, value="<취득하고자 하는 요소 ID>")
   driver.find_elements_by_id("<취득하고자 하는 요소 ID>")

   # 취득한 첫번째 요소의 innerText를 획득하기
   driver.find_elements(by=By.ID, value="<취득하고자 하는 요소 ID>")[0].text
   ~~~


<br>
- {:.p-header} By class name & tag name
  
   클래스명을 이용
   ~~~python
   driver.find_elements_by_class_name("<취득하고자 하는 요소 CLASS>")
   ~~~

   태그명을 이용
   ~~~python
   driver.find_element_by_tag_name("<취득하고자 하는 요소 TAG>")
   ~~~

   find_element함수를 이용
   ~~~python
   driver.find_elements(By.CLASS_NAME, "<취득하고자 하는 요소 CLASS>")
   driver.find_element(By.TAG_NAME, "<취득하고자 하는 요소 TAG>")
   ~~~


<br>
- {:.p-header} By name
  
   html의 name속성을 이용하여 요소를 찾을 수 있습니다.
   ~~~python
   driver.find_element_by_name("<취득하고자 하는 요소의 name 값>")
   driver.find_element(By.NAME, "<취득하고자 하는 요소의 name 값>")
   ~~~


<br>
- {:.p-header} By Link Text & By Partial Link Text
  
   요소의`<a href=....></a>`사이의 텍스트를 사용해서 요소를 찾을 수 있습니다.
   ~~~python
   # "페이지 검색"이라는 텍스트가 있는 요소 검색
   driver.find_elements_by_link_text("페이지 검색")

   # "검색"이라는 텍스트가 포함되어 있는 요소 검색
   driver.find_elements_by_partial_link_text("검색")
   ~~~


<br>
> ##### html 요소 상태 확인

   ~~~python
   element.is_displayed()
   element.is_enabled()
   element.is_selected()
   ~~~


<br>
> ##### 유저 입력 (클릭이나 선택)

- {:.p-header} 마우스 클릭

   취득한 요소에 대해 .click() 함수를 사용하여 클릭 가능


- {:.p-header} submit

   기본적으로 취득한 요소에 대해 click() 함수를 사용하여 submit 버튼을 클릭하면 됩니다. 

   혹은 form 내부에 있는 요소에 대해 element.submit() 함수를 이용하여 submit이 가능합니다.


- {:.p-header} 문자열 입력

   send_keys() 함수를 이용하여 text feild나 text area에 문자열을 입력합니다.

   send_keys() 함수를 이용하기 위해서는 from selenium.webdriver.common.keys import Keys 를 선언해줄 필요가 있습니다.

   ~~~python
   from selenium.webdriver.common.keys import Keys

   element.send_keys("<입력하고자 하는 문자열>")

   # 특수키 입력
   element.send_keys(Keys.RETURN)

   # 문자열 입력 후 특수키 입력
   element.send_keys("文字列", Keys.RETURN)

   # 요소 클리어
   element.clear() 
   ~~~
  

- {:.p-header} select 요소 내의 option 선택

   select = Select(driver.find_element....) 를 이용하여 select 요소를 취득할 수 있습니다.

   ~~~python
   select = Select(driver.find_element....)

   # 선택하기 
   select.select_by_index(index)
   select.select_by_visible_text("text")
   select.select_by_value(value)

   # 선택 해제하기
   select.deselect_by_index(index)
   select.deselect_by_visible_text("text")
   select.deselect_by_value(value)
   select.deselect_all() # 모든 선택 해제

   # 선택된 옵션 리스트
   select.all_selected_options

   # 선택 가능한 옵션 리스트
   select.options
   ~~~

   <br>

   python scraping에 관한 포스트 모음 : <a href="{{site.url}}/tags#scraping_cap" target="_blank">Tags - Scraping</a>
   {: style="font-weight: bold; color: brown; text-align: center;"}


<br><br>
