---
layout: post
title:  "Python + Selenium을 이용하여 chrome 자동화 - 페이지 조작 및 스크린샷 관련 정리"
description: "selenium, web driver, google, python, chrome, click, sleep"
date:   2019-06-01 12:00:00 +0800
categories: PYTHON
tags: Scraping python selenium
lang: ko_KR
comments: true
---


Python, Chrome, Selenium을 이용하여 페이지 조작과 스크린샷 관련하여 기본적인 내용을 정리하겠습니다.


<br>
> ##### 페이지 기본 조작 관련

특정 페이지 획득

~~~python
driver.get("<획득하고자 하는 url>")
~~~
{: style="line-height: 0;"}

뒤로가기
  
~~~python
driver.back()
~~~
{: style="line-height: 0;"}
  
앞으로 가기
  
~~~python
driver.forward()
~~~
{: style="line-height: 0;"}

페이지 갱신
  
~~~python
driver.refresh()
~~~
{: style="line-height: 0;"}

현재 페이지의 url 획득
  
~~~python
driver.current_url
~~~
{: style="line-height: 0;"}

현재 페이지 타이틀 획득
  
~~~python
driver.title
~~~
{: style="line-height: 0;"}

페이지 소스 획득
  
~~~python
driver.page_source
~~~
{: style="line-height: 0;"}

현재 페이지 닫기
  
~~~python
driver.close()
~~~
{: style="line-height: 0;"}

모든 페이지 닫기
  
~~~python
driver.quit()
~~~
{: style="line-height: 0;"}


<br>
> ##### 페이지 대기 관련

ajax 를 많이 사용하여 작성된 페이지에 대해서, 요소 추가 등의 타이밍을 맞추기 위해서는 요소의 존재나 상태를 획득할 필요가 있습니다.

- {:.p-header} 조건을 지정하여 대기
  
   아래의 모듈을 기본적으로 import 하여 조작 가능합니다.

   ~~~python
   from selenium.webdriver.common.by import By
   from selenium.webdriver.support.ui import WebDriverWait
   from selenium.webdriver.support import expected_conditions as EC
   ~~~

   최대 대기 시간 지정
   ~~~python
   wait = WebDriverWait(driver, 10) # 10초
   ~~~

   페이지 타이틀이 표시될 때까지 대기
   ~~~python
   wait.until(EC.title_contains("<PAGE TITLE>"))
   ~~~

   요소를 클릭하여, innerText가 "문자열" & class 속성이 "myclass"인 요소가 표시될때까지 대기
   ~~~python
   element.click()
   wait.until(
      EC.text_to_be_present_in_element((By.CLASS_NAME, "myclass"), "문자열")
   )
   ~~~

   class속성이 "myclass" 인 요소가 visible이 될때까지 대기하고 요소 클릭
   ~~~python
   a = wait.until(
      EC.visibility_of_element_located((By.CLASS_NAME, "myclass"))
   )
   a.click()
   ~~~

   <br>
   그 밖의 다양한 함수가 존재합니다.

   * 조작할 요소 지정
  
      `presence_of_element_located` 

      `visibility_of_element_located` 

      `presence_of_all_elements_located` 

      `frame_to_be_available_and_switch_to_it` 

      `invisibility_of_element_located` 

      `element_to_be_clickable` 

      `element_located_to_be_selected` 

   * 조작할 요소와 값을 지정

      텍스트 지정

      `text_to_be_present_in_element`

      `text_to_be_present_in_element_value`

      셀렉션 상태를 boolean으로 지정

      `element_located_selection_state_to_be` 

<br>
- {:.p-header} 폴링시간 지정
  
   요소가 바로 발견되지 않을 경우 대기 시간을 지정할 수 있습니다.

   `driver.implicitly_wait(10)`


- {:.p-header} time.sleep() 사용
  
   스크래핑 하는 사이트 서버에 무리를 가하지 않기 위해서 time.sleep() 함수를 사용할 필요가 있습니다.


<br>
> ##### 특정 html 요소까지 스크롤

   ~~~python
   element = driver.find_element_by_id("ID")
   actions = new Actions(driver)
   actions.move_to_element(element)
   actions.perform()
   ~~~


<br>
> ##### 스크린샷 관련

   스크린샷을 하기 위해서 사용 가능한 함수를 정리하겠습니다.

- {:.p-header} 페이지 사이즈 관련
  
   페이지를 특정 사이즈로 변경
   
   `driver.set_window_size(1280, 720)`

   페이지를 최대 사이즈로 변경 (headless 모드에서는 동작 안함)

   `driver.maximize_window()`

   페이지 사이즈 획득

   `driver.get_window_size()`

   미리 페이지 사이즈를 지정해서 chrome을 실행

   ~~~python
   options.add_argument('--window-size=800, 600')

   # 최대 사이즈로 chrome 실행
   options.add_argument('--start-maximized')  

   # 전체 화면 표시로 chrome 실행
   options.add_argument('--start-fullscreen')  
   ~~~

- {:.p-header} 스크롤바 제거
  
   `driver.execute_script("document.body.style.overflow='hidden';")`

- {:.p-header} 스크린샷
  
   파일 이름을 지정하여 스크린샷

   `driver.save_screenshot('screenshot.png')`

   스크린샷을 파일이 아닌 메모리에 저장

   ~~~python
   # 메모리에 저장을 해서 바이너리 데이터를 파일 오브젝트로 변환하거나 할 수 있습니다.
   driver.get_screenshot_as_png()
   driver.get_screenshot_as_base64()
   ~~~

   <br>

   python scraping에 관한 포스트 모음 : <a href="{{site.url}}/tags#scraping_cap" target="_blank">Tags - Scraping</a>
   {: style="font-weight: bold; color: brown; text-align: center;"}

<br><br>
