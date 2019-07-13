---
layout: post
title:  "내가 사는 지역의 신축 아파트 리스트를 구글맵으로 표시하기"
description: "scraping, python, google maps, personal, mongodb, csv, venv, googlemaps import csv, map data"
date:   2019-07-13 12:00:00 +0800
categories: PERSONAL
tags: GoogleMap Scraping Python
lang: ko_KR
comments: true
---


자신이 사는 지역의 신축 아파트 리스트를 부동산 사이트에서 스크래핑해서 데이터를 모은 후, 최종적으로 구글 맵에 보기 쉽게 표시한 개인용 프로젝트 입니다.

<br>
> ##### 전체적인 흐름

   1. 로컬(macos)에서 venv를 이용하여 python 가상환경을 준비
   
   2. 부동산 사이트를 스크래핑 하는 python 코드 실행
   
   3. 스크래핑된 자료가 로컬의 MongoDB에 저장됨
   
   4. mongoShell을 이용하여 MongoDB의 데이터 일부(가격, 위치, 홈페이지 등)를 CSV로 출력
   
   5. 구글 맵에 해당 CSV를 임포트해서 표시


   
<br>
> ##### 구글 맵

{::nomarkdown}
<iframe src="https://www.google.com/maps/d/u/0/embed?mid=1l5MvmZjsO6SX1eopWy-OGzUK7wfYZJIe" width="800" height="600"></iframe>
{:/nomarkdown}

1~2개월에 한번씩 업데이트 할 계획



<br><br>
