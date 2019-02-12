---
layout: post
title:  "Ruby 메소드 - 기초"
description: "Ruby의 메소드에 대해서 알아보고자 합니다."
date:   2019-02-12 20:00:00 +0800
categories: RUBY
tags: Ruby Programming
lang: ko_KR
comments: true
---

유저가 독자적으로 메소드를 정의하는 방법에 대해서 설명하겠습니다. 


<br>
> ##### 메소드 정의

기본적인 문법은 다음과 같습니다. 
~~~ ruby
def 메소드명 (파라미터, ...)
    동작
end
~~~
파라미터의 디폴트 값을 지정해 주기 위해서는 "메소드명(파라미터=값)"과 같은 형태로 선언해 줍니다. 

메소드 호출은 "메소드명(파라미터)"와 같은 형태로 가능합니다. 여기서 ()는 생략 가능합니다. 

<br/>
샘플 코드는 다음과 같습니다. 
~~~ ruby
def hello(name = "Ruby")
    msg = "Hello, " + name + ".¥n"
    puts msg
end
# 호출 : hello()
# 결과 : Hello, Ruby.

# 호출 : hello("Developer")
# 결과 : Hello, Developer.
~~~

<br/>
메소드에서 리턴값을 정의해 주는 것도 가능합니다. 

샘플 코드는 다음과 같습니다. 
~~~ ruby
def sum(a, b)
    return a + b
end
# 호출 : sum(2, 3)
# 결과 : 5
~~~

<br/>
파라미터의 갯수가 호출시마다 달라질 경우에는 마지막 파라미터의 앞에 * 을 정의함으로써, 마지막 파라미터를 가변으로 입력받을 수 있게 하는 것도 가능합니다. 해당 파라미터는 메소드 안에서 배열로써 취급됩니다.

그리고 복수의 파라미터를 가지는 메소드를 호출 시, 파라미터를 배열로 전달하는 것도 가능합니다. 
~~~ ruby
# 파라미터 앞에 *를 추가한 경우
def sum1(*nums)
    result = 0
    # nums는 배열로 취급
    nums.each { |num|
        result += num
    }
    return result
end
# 호출 : sum1(1, 2, 3)
# 결과 : 6

# 호출 : sum1(1, 2, 3, 4, 5)
# 결과 : 15


# 복수의 파라미터를 가지는 메소드
def sum2(a, b, c)
    return a + b + c
end
# 호출 : sum2([1,2,3])
# 결과 : 6
~~~

<br>
마지막으로 메소드에서 복수의 리턴값을 동시에 돌려주는 것도 가능합니다.
~~~ ruby
def multiple_return(a, b)
    return a+b, a-b
end
# 호출 : 
# return1, return2 = multiple_return(20, 10)
# puts return1
# puts return2

# 결과 : 
# 30
# 10
~~~


<br>
> ##### 코드 블록을 메소드의 파라미터로 전달하기

메소드 호출시 코드 블록을 파라미터로 넘겨주는 것도 가능합니다. 메소드를 호출할 때, 메소드 뒤에 "{}"를 붙임으로써 지정 가능합니다. 

호출된 메소드 내부에서는, yield를 선언한 곳에서 해당 코드 블록이 실행됩니다. 

yield에 파라미터를 전달하는 것도 가능한데, 이때 해당 파라미터는 코드 블록에 선언된 변수에 할당됩니다. 

<br>
샘플 코드
~~~ ruby
def block1
    if block_given?
        yield
    else
        puts "no block"
    end
end
# 호출 : block1{puts "Ruby"}
# 결과 : Ruby

# 호출 : block1
# 결과 : "no block"


def block2(last)
    for i in 0..last
        yield i
        # yield에 파라미터를 지정하였으므로, | |안에 선언된 x에 i값이 할당된다.
    end
end
# 호출 : block2(3){|x| puts x}
# 결과 : 
# 0
# 1
# 2
# 3
~~~


<br><br><br>
