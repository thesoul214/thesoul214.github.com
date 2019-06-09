---
layout: post
title:  "Ruby 클래스 - 확장"
description: "Ruby의 클래스, 메소드에 대해 좀 더 자세한 내용을 알아보자."
date:   2019-02-18 22:00:00 +0800
categories: RUBY
tags: Ruby
lang: ko_KR
comments: true
---

클래스나 오브젝트를 확장하는 수단에 대해서 알아보겠습니다. 

<br>
> ##### 클래스 상속

루비에서는 클래스(부모 클래스)를 상속해서 새로운 클래스(자식클래스)를 작성하는 것이 가능합니다. 자식 클래스에서는 부모 클래스의 메소드를 사용할 수 있습니다. 

정의는 다음과 같습니다. 
~~~ ruby
class 클래스명 < 부모 클래스
    # 클래스 확장 코드
end
~~~

<br>
기본적으로 부모 클래스를 생략한 모든 클래스는 Object클래스를 상속받고 있는 것으로 취급됩니다. 자식 클래스에서는 부모 클래스의 메소드를 오버라이드 할 수 있습니다. 
~~~ ruby
class SuperClass
    def initialize(name = "kim")
        @name = name
    end

    def morning(hour)
        print "Good Morning ", @name, ", It is ", hour.to_i, "am(in SuperClass)¥n"
    end

    def hello
        print "Hello ", @name, ".¥n"
    end

    def evening
        print "Good Evening ", @name, "(SuperClass)¥n"
    end
end

class ChildClass < SuperClass
    def morning(hour)           # morning 메소드를 오버라이드
        super                   # 부모 클래스의 메소드를 호출
    end                         # 파라미터를 지정하지 않아도 자동으로 부모 클래스에 파라미터를 전달한다. 

    def hello                   # hello 메소드를 오버라이드
        print "Hello ",  @name, "(ChildClass).¥n"
    end

    def bye
        print "Good Bye ", @name, "(ChildClass).¥n"
    end
end

# 실행 :
# s = ChildClass.new("Anyone")
# s.morning(7)
# s.hello
# s.evening
# s.bye

# 결과
# Good Morning Anyone, It is 7am(in SuperClass)
# Hello Anyone (ChildClass)
# Good Evening Anyone (SuperClass)
# Good Bye Anyone (ChildClass)
~~~
루비에서는 단일 상속만 가능하지만, Mix-in을 사용하면 다양한 기능을 클래스에서 사용할 수 있습니다. 


<br>
> ##### 이미 정의되어 있는 클래스의 확장(오픈 클래스)

루비에서는 이미 정의되어 있는 클래스에 메소드를 추가하는 것이 가능합니다. 

정의는 다음과 같습니다. 
~~~ ruby
class 기존 클래스
    def 메소드
        # 메소드 정의
    end
end
~~~


다음의 샘플코드는 이미 정의되어 있는 클래스 String에 문자열로부터 단어수를 계산하는 메소드를 추가한 예입니다. 
~~~ ruby
class String
    def count_word
        return self.split(/¥s+/).size
    end
end

# 실행 :
# "Hello, Ruby World".count_word

# 결과 :
# 3
~~~


<br>
> ##### 오브젝트에 메소드 정의하기

루비에서는 클래스가 아닌 오브젝트에 메소드를 정의하는 것이 가능합니다. 해당 메소드가 정의된 오브젝트에서만 사용가능하며, 다른 오브젝트에서는 사용이 불가합니다. 


기본적인 정의는 다음과 같습니다. 
~~~ ruby
def 오브젝트명.메소드명
    # 메소드 정의
end
~~~


다음은 예제 코드입니다.
~~~ ruby
def Foo
end

foo1 = Foo.new
foo2 = Foo.new

def foo1.sing_method
    puts "only foo1 can call this method"
end

foo1.sing_method
foo2.sing_method # foo1 오브젝트에 정의되어 있는 메소드이므로 호출 불가.

# 결과 :
# only foo1 can call this method
# 에러! 
~~~


<br>
> ##### 라이브러리 로드(require)

루비에서는 라이브러리를 로드하는 것이 가능합니다. 로드 가능한 파일로는 Ruby스크립트(.rb)와 Ruby확장 라이브러리(.so, .o, .dll등...)가 있습니다. 

라이브러리의 확장자가 .rb나 .so등의 경우에는 확장자를 생략하는 것이 가능합니다. 그리고 로드를 할 파일이 require을 사용하는 파일과 같은 디렉토리에 있는 경우, "./파일명" 이라고 기술함으로써 해당 파일을 로드하는 것이 가능합니다. 
~~~ ruby
--- library.rb --- 
class Foo
    def hello
        puts "hello"
    end
end

--- main.rb ---
require "./library"

foo = Foo.new
foo.hello

# 결과 :
# hello
~~~


<br>
> ##### 모듈(module)

모듈이란 메소드나 정수를 모아둔 파일을 의미합니다. 클래스와 비슷하지만, 인스턴스화와 상속이 불가능하다는 점이 다릅니다. 

주로 이름 공간(namespace)의 제공이나 Mix-in을 사용하여 클래스를 확장할 시 이용합니다. 

<br>
모듈 내에 메소드를 정의하는 기본적인 구문은 다음과 같습니다. 
~~~ ruby
module 모듈명

    def self.메소드명
        # 코드
    end

    def 모듈명.메소드명
        # 코드
    end
end
~~~
모듈내의 클래스나 정수에 접근할 경우에는 "모듈명::"을 각각의 선두에 붙여줍니다. 

모듈내의 메소드에 접근할 경우에는 "모듈명.메소드명"이라고 사용합니다. 


다음은 샘플코드입니다. 
~~~ ruby
module M1
    class Foo
        def print
            puts "M1's print method."
        end
    end
end

module M2
    class Foo
        def print
            puts "M2's print method."
        end
    end
end

module M3
    Const = 100
    def M3.bar
        puts "M3's bar method"
    end
end

m1 = M1::Foo.new
m1.print
m2 = M2::Foo.new
m2.print

print "M3 Const : ", M3::Const, "¥n"
M3.bar

# 결과 :
# M1's print method.
# M2's print method.
# M3 Const : 100
# M3's bar method
~~~
Foo클래스나 print메소드가 서로 다른 모듈에 정의되어 있으므로 각각 다른 클래스나 메소드로 취급됩니다. 


<br>
> ##### Mix-in

Mix-in이란, 모듈을 사용하여 클래스를 확장하는 기능입니다. 

기본적으로 ruby의 클래스는 단일 상속만 가능하지만, 이 Mix-in을 사용함으로써 다중 상속과 같은 기능을 구현하는 것이 가능합니다. 

모듈을 클래스 내에서 사용하기 위해서는, include메소드를 사용합니다. 

include를 사용함으로써, 모듈 내의 메소드를 include한 클래스의 인스턴스 메소드처럼 호출하여 사용하는 것이 가능해 집니다. 모듈 내의 Mix-in용 메소드를 정의할 때에, "모둘명."이나 "self."를 붙일 필요가 없습니다.

~~~ruby
module Foo
    def module_method
        puts "hello"
    end
end

class Bar
    include Foo     # 모듈을 include
end

bar = Bar.new
bar.module_method

# 결과 :
# hello
~~~

<br>
또한 extend메소드를 사용함으로써, 모듈내의 메소드를 클래스 메소드로써 추가하거나 임의의 오브젝트에 추가하는 것이 가능합니다.
~~~ruby
module Foo
    def module_method
        puts "hello"
    end
end

class Bar
    extend Foo
end

Bar.module_method

str = "abcd"
str.extend.Foo
str.module_method

# 결과 :
# hello
# hello
~~~

<br>

Ruby에 관한 포스트 모음 : <a href="{{site.url}}/tags#ruby_cap" target="_blank">Tags - Ruby</a>
{: style="font-weight: bold; color: brown; text-align: center;"}

<br><br>
