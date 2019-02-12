---
layout: post
title:  "Ruby 조건문 - 기초"
description: "Ruby의 조건문의 기본적인 문법에 대해서 알아보고자 합니다."
date:   2019-02-10 15:00:00 +0800
categories: RUBY
tags: Ruby Programming
lang: ko_KR
comments: true
---

새로운 회사에서 Ruby를 주로 사용하기 때문에, 처음으로 Ruby를 공부하고 있습니다. 

겸사겸사해서 공부한 내용들 블로그로 정리하고자 합니다. 


<br>
> ##### 조건문 - if문

기본적인 문법은 다음과 같습니다. 

~~~ ruby
if 조건문1 then
    # 조건문1이 참인 경우 동작
elsif 조건문2 then
    # 조건문2가 참인 경우 동작
else
    # 조건문1, 2 모두 거짓인 경우 동작
end
~~~

then은 생략 가능합니다. 

Ruby에서는 false와 nil이외는 모두 [참]으로 처리됩니다. 


<br>
> ##### 조건문 - case문

기본적인 문법은 다음과 같습니다. 

~~~ ruby
case 오브젝트
when 값1 then
    # 오브젝트와 값1이 일치하는 경우 동작
when 값2 then
    # 오브젝트와 값2가 일치하는 경우 동작
else
    # 오브젝트가 값1, 2와 일치하지 않는 경우 동작
end
~~~

case문에서도 then은 생략 가능합니다. 

when 절에서의 값은 [when 값1, 값2, 값3] 처럼 콤마로 구분해서 나열하는 것도 가능합니다. 


<br>
> ##### 그 외의 조건문

조건문을 사용하는 방법으로, if와 case문 외에 다음과 같은 방법도 있습니다.

~~~ ruby
# 삼항 연산자
# [구문] 조건식 ? 참인 경우 값 : 거짓인 경우 값
score = 80
result = score > 70 ? "pass" : "failed" # 조건식의 [참/거짓] 결과에 따라 좌측의 result에 값을 대입한다.
puts result
# 출력 : pass


# unless문 - if 문의 반대
# [구문]
# unless 조건식 then
#   조건식이 거짓인 경우 동작
# else
#   조건식이 참인 경우 동작
lang = "Ruby"

unless lang == "Ruby" then
    puts "#{lang} is not Ruby"
else
    puts "Enjoy Ruby toturial"
end
# 결과 : Enjoy Ruby toturial


# if수식, unless수식
# [구문] 참인 경우 동작 if 조건식
# [구문] 거짓인 경우 동작 unless 조건식
debug = true
num = 10
puts "num = #{num}" if debug 
# 결과 : num = 10
~~~






<br><br><br>
