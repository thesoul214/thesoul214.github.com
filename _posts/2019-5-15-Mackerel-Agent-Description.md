---
layout: post
title:  "mackerel-agent checks 플러그인"
description: "서버의 리소스를 시각화 하거나 감시할 수 있는 서비스인 Mackerel. 감시하고 싶은 호스트에 설치하여 리소스나 성능을 쉽게 파악할 수 있게 해주는 mackerel-agent에 모니터링 기능으로 추가하는 checks 플러그인에 대해서 알아보겠습니다."
date:   2019-05-15 12:00:00 +0800
categories: SERVER
tags: Monitoring Mackerel
lang: ko_KR
comments: true
---


감시하고 싶은 호스트에 설치하여 리소스나 성능을 쉽게 파악할 수 있게 해주는 mackerel-agent와는 별도로, 추가적으로 설치하여 프로세스나 로그를 감시할 수 있게 해주는 checks 플러그인에 대해서 정리하고자 합니다. 

checks 플러그인을 정상적으로 설정하면, Mackerel Host화면의 Monitors부분에 설정한 플러그인이 표시됩니다.

{:.p-footnote}
이전 글 : <a href="{{site.url}}/server/2019/05/13/Mackerel-Basic.html" target="_blank">Mackerel - 서버 리소스 시각화 및 감시 서비스 기초</a>


<br>
> ##### 설치

checks 플러그인은 mackerel-agent 설치시에 표준 기능으로 제공되지 않기 때문에, mackerel-agent와는 별도로 설치해 주어야 합니다.

mackerel-agent가 설치된 호스트에 로그인하여 이하의 커맨드를 실행하여 설치할 수 있습니다.

~~~ 
$ sudo yum install mackerel-check-plugins
~~~
{: style="padding-left: 1rem; margin-top:1rem; margin-bottom:2rem;"}


설치를 완료하면 하기의 checks 플러그인이 호스트에 설치되어 사용할 수 있는 상태가 됩니다. 

설치된 플러그인은 /usr/bin/에서 확인 가능합니다. 

~~~
check-aws-cloudwatch-logs
check-aws-sqs-queue-size
check-cert-file
check-disk
check-elasticsearch
check-file-age
check-file-size
check-http
check-jmx-jolokia
check-ldap
check-load
check-log
check-mailq
check-masterha
check-memcached
check-mysql
check-ntpoffset
check-postgresql
check-procs
check-redis
check-smtp
check-solr
check-ssh
check-ssl-cert
check-tcp
check-uptime
~~~

위의 플러그인을 이용하기 위해서는 /etc/mackerel-agent/mackerel-agent.conf 에 설정을 해주어야 합니다. 

설정을 해주면 mackerel-agent만으로는 감시가 불가능한 항목을 감시하여 alert를 설정하는 것이 가능해 집니다.


<br>
> ##### checks 플러그인 감시 설정

하기의 파일을 편집하여 설정을 할 수 있습니다. 

~~~shell
/etc/mackerel-agent/mackerel-agent.conf
~~~

디폴트로 mackerel의 apikey 값만 설정이 되어 있고, 나머지는 모두 주석처리된 것을 확인할 수 있습니다.

해당 포스트에서는 checks 플러그인을 이용하여 포트, 프로세스, 로그를 감시하는 방법을 알아보겠습니다.
  
~~~shell
apikey = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"

# Mackerel 화면에 호스트 이름 대신에 표시되는 이름
display_name = "Mackerel-Test-Ins"

# /var/log/mackerel-agent.log에 상세한 로그를 기록할지 여부
verbose = true

# Agent가동시 부여할 서비스와 롤 설정
roles = [ "john-production-service:john-production-web" ]

# Agent가동시 호스트의 상태를 정의
[host_status]

# Agent가동시 Mackerel상의 상태가 working 이 되도록 설정
on_start = "working"

# Agent 중지시 Mackerel상의 상태가 poweroff 가 되도록 설정
on_stop  = "poweroff"


# checks 플러그인 설정 - 포트 감시
# [mackerel-check_port-http]라는 감시명으로 Mackerel 화면의 monitors에  표시됩니다.
[plugin.checks.mackerel-check_port-http]

# 역치값 설정
# localhost의 80번 포트에 대해 응답속도가 10초인 경우에는 Warning、20초인 경우에는 Critical이 되도록 설정
# localhost부분에 별도의 서버 IP주소를 입력함으로써 외부에서 감시하는 것도 가능
command = "check-tcp -H localhost -p 80 -w 10 -c 20"

# Alert 재알림 설정
# 60분마다 Alert 발생
notification_interval = 60

# 최대 실행 횟수 설정
# 3회 이상 역치를 넘을 경우 Mackerel에 Alert
max_check_attempts = 3

# 감시 간격 설정
# 디폴트는 1분, 하기의 경우 5분
check_interval = 5


# checks 플러그인 설정 - 프로세스 감시 1
# [mackerel-check_proc-sshd]라는 감시명으로 Mackerel 화면에 표시됩니다.
[plugin.checks.mackerel-check_proc-sshd]

# 프로세스 감시 설정
# 루트 유저로 실행되고 있는 sshd프로세스의 동작 여부를 감시
command = "check-procs --pattern sshd --user root"
notification_interval = 60
max_check_attempts = 3
check_interval = 5


# checks 플러그인 설정 - 프로세스 감시 2
# [mackerel-check_proc-httpd]라는 감시명으로 Mackerel 화면에 표시
[plugin.checks.mackerel-check_proc-httpd]

# httpd 프로세스를 감시
command = "check-procs --pattern httpd --user apache"
notification_interval = 60
max_check_attempts = 3
check_interval = 5


# checks 플러그인 설정 - 로그 감시
# [mackerel_log-messages]라는 감시명으로 Mackerel 화면에 표시
[plugin.checks.mackerel_log-messages]

# /var/log/messages 파일에 ERROR 문자열이 검출될 경우 Mackerel에 Alert
command = "check-log --file /var/log/messages --pattern ERROR --return"
~~~

위와 같이 설정을 하였으면, 하기의 커맨드로 mackerel-agent를 restart합니다.

~~~
sudo service mackerel-agent restart
~~~

정상적으로 설정될 경우, Mackerel Host화면의 Monitors부분에 설정한 플러그인이 표시됩니다.


<br>

mackerel에 관한 포스트 모음 : <a href="{{site.url}}/tags#mackerel_cap" target="_blank">Tags - Mackerel</a>
{: style="font-weight: bold; color: brown; text-align: center;"}
<br><br>
