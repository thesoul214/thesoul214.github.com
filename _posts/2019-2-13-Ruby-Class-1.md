---
layout: post
title:  "Ruby 클래스 - 기초1"
description: "Ruby의 클래스에 대해서 알아보고자 합니다."
date:   2019-02-13 20:00:00 +0800
categories: RUBY
tags: Ruby Programming
lang: ko_KR
comments: true
---

클래스 기술 방법과 클래스 내의 변수나 메소드에 대해서 설명하겠습니다. 


<br>
> ##### 클래스 정의

클래스는 오브젝트의 동작(형태)을 정의한 모형입니다. 클래스를 인스턴스화 함으로써 오브젝트가 생성됩니다. 

기본적인 문법은 다음과 같습니다. 
~~~ ruby
class 클래스명
    클래스 정의
end
~~~

클래스 명은 무조건 대문자로 시작해야 합니다. 

"클래스명.new"를 선언함으로써, 인스턴스화를 통해 오브젝트를 생성할 수 있습니다.

<br/>
샘플 코드는 다음과 같습니다. 
~~~ ruby
class Greeting
    def hello(str = "Hello")
        puts "Hello #{str}"
    end
end
# 명령어 : 
# g = Greeting.new  -> 오브젝트 생성
# g.Hello("World")  -> 메소드 실행
# 결과 :
# Hello World
~~~


<br>
> ##### 클래스 상의 변수

- 변수의 종류
  
    루비에서는 총 4가지 종류의 변수가 있습니다. 변수들은 각각 스코프(유효범위)가 다르며, 명명 규칙에 따라 종류를 구별합니다. 

    {:.ruby_class_1}
    |---
    | 변수명 | 스코프 |명명규칙 |
    |:-:|:-:|:-:|
    | 로컬 변수 | 선언한 위치로부터 블록/메소드/클래스/모듈의 마지막 부분까지 | |
    | 글로벌 변수 | 프로그램 전체, 프로그램 어디에서든지 참조 가능 | $변수명 |
    | 인스턴스 변수 | 변수를 정의한 해당 객체 내부(퍼블릭이 아니다) | @변수명 |
    | 클래스 변수 | 클래스 내 전체(해당 클래스의 모든 객체가 공유 가능) | @@변수명 |
    

    <br>
    인스턴스 변수에 관해 코드로 살펴 보겠습니다.
    ~~~ ruby
    class Greeting
        def setName(name = "Ruby")
            @name = name # @가 한개이므로 name은 인스턴스 변수이다.
        end

        def hello
            print "Hello ", @name, "¥n"
        end
    end
    # 실행 :
    # g1 = Greeting.new -> 오브젝트 생성
    # g1.setName("Kim") -> g1에 이름 설정
    # g2 = Greeting.new -> 오브젝트 생성
    # g2.setName("Lee") -> g2에 이름 설정

    # g1.hello -> Hello Kim 출력
    # g2.hello -> Hello Lee 출력
    ~~~
    위와 같이 인스턴스 변수는 각 오브젝트 별(g1, g2)로 다른 값을 저장하고 있다. 

    <br>
    다음은 위의 샘플코드에서 인스턴스 변수를 클래스 변수로 변경한 코드입니다. 
    ~~~ ruby
    class Greeting
        def setName(name = "Ruby")
            @@name = name # @가 두개이므로 name은 클래스 변수이다.
        end

        def hello
            print "Hello ", @@name, "¥n"
        end
    end
    # 실행 :
    # g1 = Greeting.new -> 오브젝트 생성
    # g1.setName("Kim") -> g1에 이름 설정
    # g2 = Greeting.new -> 오브젝트 생성
    # g2.setName("Lee") -> g2에 이름 설정

    # g1.hello -> Hello Lee 출력
    # g2.hello -> Hello Lee 출력
    ~~~
    위와 같이 클래스 변수는 오브젝트 간에 공유되는 변수이므로, g2.setName("Lee")를 실행할 때 g1에서 저장한 @@name이 덮어 쓰여집니다. 

<br>
- 인스턴스 변수의 액세스 메소드(접근자)
    
    클래스내에서 사용되는 인스턴스 변수는 클래스 외부에서 직접적으로 조회/변경하는 것이 불가능 합니다. 그래서 인스턴스 변수를 외부에서 조회/변경하기 위해서는, 액세스 메소드(Getter, Setter)를 이용해야 합니다. 

    루비에서는 인스턴스 변수에의 접근을 용이하게 하기 위해서, 액세스 메소드를 자동적으로 생성하는 방법이 미리 준비되어 있습니다. 
    
    클래스 내에서 다음과 같은 메소드를 호출하는 것이 가능합니다. 

    {:.ruby_class_2}
    |---
    | 메소드명 | 기능 |
    |:-:|:-:|
    | attr_reader | 조회만 가능 |
    | attr_writer | 변경만 가능 |
    | attr_accessor | 조회/변경 가능 |
    

    <br>
    샘플코드입니다. 
    ~~~ ruby
    class Greeting
        attr_accessor :name # 액세스 메소드 정의
    end

    # 실행 :
    # g = Greeting.new
    # g.name = "KIM"
    # puts g.name
    # 결과 : 
    # KIM
    ~~~
    위의 코드에서 알수 있듯이, 인스턴스 변수에 값을 할당하기 위해 굳이 setName과 같은 메소드를 정의해 주지 않아도 됩니다. 


<br><br><br>
