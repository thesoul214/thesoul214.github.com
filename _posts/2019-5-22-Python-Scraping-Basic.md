---
layout: post
title:  "Python과 Beautiful Soup로 웹 스크래핑"
description: "웹 스크래핑중 가장 기본인 python Beautiful Soup를 이용하는 방법에 대해서 알아보자."
date:   2019-05-22 12:00:00 +0800
categories: PYTHON
tags: Scraping python beautifulsoup4
lang: ko_KR
comments: true
---


python 라이브러리 중 하나로, 스크래핑에 특화된 모듈인 beautifulsoup4에 대해서 알아보려고 합니다.

사용하는 python 버전은 3.7.1 입니다.


<br>
> ##### 기본 사용법

- {:.p-header} BeautifulSoup 오브젝트 생성
  
   웹사이트 URL에서 스크래핑을 하고자 하는 경우에는 requests를 이용합니다.

   ~~~python
   import requests
   from bs4 import BeautifulSoup
   
   url = "https://www.mansion-review.jp/shinchiku/prefecture/34.html"
   
   res = requests.get(url)
   res.encoding = res.apparent_encoding

   bs = BeautifulSoup(res.text, "html.parser")
   ~~~

   위의 코드는 url에서 취득한 문서를 html로 파싱한 오브젝트를 bs에 할당한 것입니다.

<br>
- {:.p-header} 간단한 html태그 취득 방법
  
   find_all 메소드를 사용해서 HTML에서 A태그 전체를 취득합니다.
   ~~~python
   bs.find_all("a")
   ~~~
   find_all로 취득한 오브젝트 <class 'bs4.element.ResultSet'> 는 list처럼 취급할 수 있습니다.

   태그 전체가 아닌 첫번째 요소만을 취득하기 위해서는 find를 사용할 수 있습니다.
   ~~~python
   bs.find("a")
   bs.a
   ~~~
   
   태그가 HTML에 존재하지 않는 경우에는 None이 리턴됩니다.

<br>
- {:.p-header} 취득한 태그 정보
  
   취득한 태그의 속성 획득하기
   ~~~python
   bs.a.get("href")
   ~~~

   취득한 태그의 문자열 획득하기
   ~~~python
   bs.a.string
   ~~~

<br>
- {:.p-header} 조건을 추가해서 태그 정보 취득
  
   `<a href="/link" class="link">` 와 같은 class가 link고 href가 /link인 a 태그 취득하기
   {: style="margin-left: -1rem;"}
   ~~~python
   bs.find_all("a", class_="link", href="/link") # class_ 주의
   bs.find_all("a", attrs={"class": "link", "href": "/link"})
   bs.find_all(attrs={"class": "link", "href": "/link"})
   ~~~

<br>
- {:.p-header} css 셀렉터를 이용해서 태그 취득
  
   find_all 대신에 select를 사용하면 css셀렉터를 사용해서 태그를 취득할 수 있습니다.
   ~~~python
   bs.select("#link1")
   bs.select('a[href^="http://"]')
   ~~~

<br>
- {:.p-header} 태그 속성 편집
  
   태그에 속성 추가하기
   ~~~python
   a = bs.find("a")
   a["target"] = "_blank"
   ~~~

   html 태그 제거하기
   ~~~python
   html = '''
    <div>
        <a href="/link">spam</a>
    </div>
    '''

    bs = BeautifulSoup(html)
    bs.div.a.unwrap()

    bs.div
    # 결과 : <div>spam</div>
    ~~~
    반대로 새로운 태그를 생성하기 위해서는 wrap 함수를 사용하면 됩니다.

<br><br>
