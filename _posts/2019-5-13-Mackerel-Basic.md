---
layout: post
title:  "Mackerel - 서버 리소스 시각화 및 감시 서비스 기초"
description: "서버의 리소스를 시각화 하거나 감시할 수 있는 서비스인 Mackerel에 대해서 알아보고자 합니다."
date:   2019-05-13 12:00:00 +0800
categories: SERVER
tags: Monitoring
lang: ko_KR
comments: true
---

Mackerel이란, 서버의 각종 하드웨어나 어플리케이션 소프트웨어의 성능을 실시간으로 감시할 수 있는 SaaS형 서버 감시 서비스입니다.

Mackerel-agent를 감시하고 싶은 서버(이하, 호스트)에 설치하면, Mackerel 화면에서 호스트의 리소스나 성능을 손쉽게 확인할 수 있습니다.

{:.p-footnote}
홈페이지 : https://mackerel.io/

업무중에 필요해서 메모용으로 작성하고 있는 내용이라, 설명이 많이 부족할 수도 있습니다.

<br>
> ##### 기초 용어 정리

- {:.p-header} Hosts
  
  호스트에 Mackerel-agent를 인스톨해서 가동시키면, Mackerel 화면의 Host항목에 자동으로 나타납니다.


- {:.p-header} Services
  
  복수의 호스트들을 통합한 그룹을 의미합니다.

  관련된 호스트들을 알기 쉽게 모아서 관리하는 것이 가능합니다.

  서비스를 작성한 후에, 밑에서 설명할 role을 작성할 수 있습니다.

- {:.p-header} Role
  
  서비스에 있는 호스트의 세부적인 역할을 의미합니다.

  예를 들어 Web서버가 10대 있다고 할 때, 10대의 호스트 각각을 Web이라는 role로 묶어서 관리하는 것이 가능합니다.

- {:.p-header} Host Metrics
  
  그래프화 가능한 호스트의 지표(메트릭스)를 의미합니다.

  Mackerel-Agent를 호스트에 설치함으로써 자동으로 수집되는 지표에는 CPU사용률이나 메모리 정보등이 있습니다.

- {:.p-header} Service Metrics
  
  그래프화 가능한 서비스의 지표(메트릭스)를 의미합니다.

  자주 사용되는 Service Metrics로는 서버의 응답시간, Http 응답 코드 등이 있습니다.

- {:.p-header} Monitors
  
  호스트에서 수집되는 지표들을 역치값을 이용하여 감시 기능을 설정하는 것이 가능합니다.

  지표값이 역치를 넘을 경우, slack/webhook/email 등으로 알림을 보내는 것이 가능합니다.

- {:.p-header} Alerts
  
  Monitors에서 감시하고 있는 리소스가 역치를 넘을 경우 Alert가 발생합니다.

  언제 어떤 호스트나 서비스에서 Alert가 발생하였는지, 그리고 현재도 진행중인지 종료되었는지를 보는 것이 가능합니다.


<br>
> ##### Mackerel-Agent-Plugins

Mackerel-Agent를 호스트에 설치함으로써, 기본적으로 loadavg, cpu, memory, disk, interface, filesystem 의 지표를 수집하는 것이 가능합니다.

그리고 추가적으로 플러그인을 설치하면 다른 지표들을 감시하는 것도 가능해 집니다.

github에 공개되어 있는 플러그인의 리스트는 하기의 url에서 확인할 수 있습니다.

https://github.com/mackerelio/mackerel-agent-plugins
  
<br>

{:.p-footnote}
관련 글 : <a href="{{site.url}}/server/2019/05/15/Mackerel-Agent-Description.html" target="_blank">mackerel-agent checks 플러그인</a>

<br>
