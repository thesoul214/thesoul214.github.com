---
layout: post
title:  "ssh 점프호스트 (경유서버를 거쳐서 목적서버에 액세스하기)"
description: "ssh, aws, ec2, linux, mac, proxycommand, ~/.ssh/config, id_rsa, id_rsa.pub, 공개키, 비밀키, 점프호스트, 경유서버, 목적서버, 시큐리티, public key, private key"
date:   2019-06-26 12:00:00 +0800
categories: SERVER
tags: Aws Ssh
lang: ko_KR
comments: true
---


목적서버에 로그인 하고자 할 때, 보안상 로컬에서 목적서버에 직접적으로 액세스 하지 않고 중간에 경유서버를 거쳐 액세스 하는 방법을 점프호스트라고 합니다.

해당 기능을 간단하게 사용하기 위한 로컬에서의 셋팅 방법에 대해 적어두고자 합니다(MAC OS).

공개키/비밀키에 대한 기본적인 구조를 알고 있는 것을 전제로 합니다.

<br>
> ##### ssh 설정 파일 구조

{:.table_width_800}
|---
| 파일명 | 역할 |
|:-:|:-:|
| ~/.ssh/config | ssh 설정 파일 |
| ~/.ssh/id_rsa | 비밀키(public key) |
| ~/.ssh/id_rsa.pub | 공개키(private key) |
| ~/.ssh/authorized_keys | 공개키 모음(공개키의 내용이 텍스트로 나열되어 있음) |
| ~/.ssh/known_hosts | 과거에 사용한 서버 리스트 |


<br>
> ##### 목적

로컬에서 커맨드 한줄로 경유서버를 거쳐 최종 목적서버까지 액세스 할 수 있도록 설정하는 것입니다.

<br/>
로컬 : Macbook
{: style="text-align: center;"}

서버 A : 경유서버 (유저명 : username1, 도메인 : aaa.com)
{: style="text-align: center;"}

서버 B : 목적서버 (유저명 : username2, 도메인 : bbb.com)
{: style="text-align: center;"}
<br/>

위와 같은 구조인 경우, 점프 호스트를 이용하지 않으면 (로컬 -> 서버 A -> 서버 B)

~~~shell
# 로컬에서 하기의 커맨드로 경유서버에 접속
ssh username1@aaa.com

# 경유서버에서 하기의 커맨드로 목적서버에 접속
ssh username2@bbb.com
~~~
위와 같은 방법으로 로컬에서 목적서버에 액세스 할 수 있지만, 커맨드를 여러번 입력해야 하므로 귀찮습니다.(목적서버나 경유서버가 복수인 경우)

그래서 로컬에서 "ssh B" 라고 커맨드를 입력하면 바로 목적서버에 액세스 할 수 있도록 설정하겠습니다.(로컬 -> 서버 B)

<br>
> ##### 셋팅

로컬의 ~/.ssh/config 파일을 다음과 같이 정의합니다.

~~~shell
# 경유서버
Host A
HostName aaa.com
User username1
IdentityFile ~/.ssh/id_rsa


# 목적서버
Host B
HostName bbb.com
User username2
ProxyCommand ssh -W %h:%p A
IdentityFile ~/.ssh/id_rsa
ServerAliveInterval 300
~~~

주의 : 비밀키는 로컬에만 가지고 있어야 하며, 서버에 공유해서는 안됩니다.
{: style="font-weight: bold; color: brown; text-align: center;"}

<br>
위에서 설정한 각각의 변수에 대해서 알아보겠습니다.

{:.table_width_800}
|---
| 변수 | 의미 |
|:-:|:-:|
| Host | 서버 닉네임 |
| HostName | 서버 주소 |
| User | 서버에 접속가능한 유저명 |
| ProxyCommand | B에 접속할 시 일단 A를 경유하라는 뜻 |
| IdentityFile | 사용할 비밀키의 위치 지정 |
| ServerAliveInterval | 300초마다 시그널을 보내서 통신이 자동으로 끊기는 것을 방지 |


<br>
> ##### 결과

위와 같이 셋팅을 끝내면, 다음과 같은 방법으로 접속이 가능해집니다.

로컬에서 경유서버에 로그인

~~~shell
ssh A
~~~

로컬에서 경유서버를 거쳐 목적서버에 로그인

~~~shell
ssh B
~~~

<br><br>
