---
layout: post
title:  "Ruby 클래스 - 기초2"
description: "Ruby의 클래스, 메소드에 대해서 알아봅시다."
date:   2019-02-18 20:00:00 +0800
categories: RUBY
tags: Ruby
lang: ko_KR
comments: true
---

루비 클래스 내에서 사용되는 메소드들에 대해서 알아 보겠습니다. 

<br>
> ##### 클래스에서 사용되는 메소드

- 메소드의 종류
   
   루비에서 메소드로는, 함수형 메소드/인스턴스 메소드/클래스 메소드가 있습니다(일본어 자료로 번역했기 때문에 어감이 이상할 수도 있습니다ㅠ ).

   함수형 메소드란, 간단히 이야기하면 커널 모듈에 정의되어 있는 메소드로써 프로그램 전체 모든 클래스에서 호출이 가능한 클래스를 의미합니다. print, sleep과 같은 함수들이 있습니다. 

   인스턴스 메소드란, "오브젝트.메소드명" 과 같은 형식으로 호출이 가능한 메소드를 의미합니다. 
   클래스내에서 메소드를 정의하면, 기본적으로 해당 클래스의 인스턴스 메소드로 정의됩니다. 

   마지막으로 클래스 메소드란, "클래스.메소드명"과 같은 형식으로 호출이 가능한 메소드를 의미합니다. "." 대신에 "::"를 사용하는 것도 가능합니다. ( 보통 혼동을 줄이기 위해, "클래스::메소드명()" 과 같은 형식으로 사용함 )

   <br>
   다음의 샘플 코드는, 자신의 클래스에 대해 클래스 메소드를 정의하는 샘플입니다. 
   ~~~ ruby
   class 클래스명
       def self.메소드명
           # 코드
       end

       def 클래스명.메소드명
           # 코드
       end
   end
   ~~~
   위와 같이 두가지 방법으로 정의 가능합니다. 
   

   <br>
   클래스에서 공통으로 사용하는 기능을 클래스 메소드로 정의합니다. 
   
   다음의 코드는 오브젝트 Person이 생성된 횟수를 리턴해주는 메소드를 클래스 메소드로 정의해 주는 코드입니다. 
   ~~~ ruby
   class Person
       @@count = 0

       def initialize() # new로 자동적으로 호출되는 메소드
           @@count += 1 # 생성될때마다 카운트업
       end

       def self.count
           return @@count
       end
   end

   # 실행 
   # person1 = Person.new
   # person2 = Person.new
   # person3 = Person.new
   # print Person.count, " people ¥n"

   # 결과
   # 3 people
   ~~~

<br>
- 메소드의 접근제어(Access Scope)
   
   다른 언어들과 마찬가지로, 루비에서도 메소드에 대한 접근제어자를 이용하는 것이 가능합니다. private, protected, public 3 종류가 있습니다. 기본 접근자는 public입니다. 

   {:.table_width_700}
   |---
   | 접근제어 | 설명 |
   |:-:|:-:|
   | private | 동일 클래스와 서브 클래스에서만 이용 가능. 동일 클래스의 서로 다른 오브젝트에서 호출 불가. |
   | protected | 동일 클래스와 서브 클래스에서만 이용 가능. 동일 클래스의 서로 다른 오브젝트에서 호출 가능. |
   | public | 모든 오브젝트에서 이용 가능(제한 없음) |

   <br>
   다음과 같이 정의합니다. 
   ~~~ ruby
   class Foo
       def pub_method       # public 메소드(디폴트)
           # 코드
       end

       protected            # protected 선언, 해당 선언 밑으로 선언되는 메소드는 자동적으로 protected로 인식된다.
           def protected_method    
           end
        
       private              # private 선언, 해당 선언 밑으로 선언되는 메소드는 자동적으로 private로 인식된다.
           def private_method
           end
   end 
   ~~~

   <br>
   다음은 접근제어를 사용한 샘플 코드입니다. 
   ~~~ ruby
   class AccessTest
       def invoke_private_method       # public 메소드
           private_method
       end

       private              # private 메소드
           def private_method
               puts "invoked private method"
           end
   end 

   # 실행 : 
   # test = AccessTest.new
   # test.invoke_private_method
   # test.private_method

   # 결과 :
   # invoked private method
   # 에러!
   ~~~


<br>

Ruby에 관한 포스트 모음 : <a href="{{site.url}}/tags#ruby_cap" target="_blank">Tags - Ruby</a>
{: style="font-weight: bold; color: brown; text-align: center;"}


<br><br><br>
