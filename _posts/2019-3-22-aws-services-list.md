---
layout: post
title:  "자주 사용되는 AWS 서비스들 간단 정리"
description: "클라우드에서 독보적인 위치를 지키고 있는 AWS의 서비스들에 관해 간단하게 알아보고자 합니다. Amazon EC2, Amazon Lightsail, AWS Lambda, AWS Elastic Beanstalk, Amazon S3, Amazon RDS, Amazon Athena등등.."
date:   2019-03-22 22:00:00 +0800
categories: SERVER
tags: Aws
lang: ko_KR
comments: true
---

AWS에는 수 많은 서비스들이 있고, 현재 회사에서 모든 서버와 인프라를 AWS로 이전하려고 하고 있기 때문에 이번 기회에 AWS를 전체적으로 살펴 보려고 합니다. 

AWS란 Amazon Web Services의 약어로써, 아마존에서 제공하는 클라우드 서비스를 의미합니다. 

각각의 서비스를 카테고리별로 나누어서 알아 보겠습니다. 

<br>
> #### Computing

- {:.p-header} Amazon EC2
  
  정식명칭은 Amazon Elastic Compute Cloud이고, AWS의 laaS의 한 종류입니다. 
  
  다양한 종류의 스펙을 가진 가상 머신을 생성하여 실행 가능하므로, 하드웨어에 투자할 필요없이 어플리케이션을 개발하고 배포할 수 있습니다. 그리고 EC2를 사용함으로써 서버의 규모를 언제든지 확장/축소할 수 있기 때문에 서버 트래픽을 예측해야 하는 필요성이 줄어듭니다.

  흔히 말하는 클라우드 서버를 생각하시면 됩니다.

- {:.p-header} Amazon Lightsail
  
  VPS(Virtual Private Server, 가상 사설 서버) 서비스입니다. 
  
  EC2와 요금 플랜이 다르고, 거의 설정할 것 없이 Wordpress나 Redmine등이 설치된 서버를 가동하는 것이 가능합니다. 

- {:.p-header} AWS Lambda
  
  서버를 관리하지 않고도 어플리케이션 코드를 실행할 수 있게 해주는 서비스입니다. 요즘 많이 들어보는 serverless로써, 운영상 대부분의 책임을 AWS가 지게 하는 것이 가능합니다. 
  
  즉 serverless 어플리케이션을 구축함으로써 개발자는 서버나 런타임 관리/조작등을 신경쓰지 않고 제품 개발에 집중할 수 있습니다. 
  
  해당 코드가 호출될 때에만 실행되며, 사용한 컴퓨팅 시간에 대해서만 요금을 지불하면 되고 코드가 실행되지 않을 때는 요금이 부담되지 않습니다. 

  지원하는 언어로는 현재 Node.js, Java, Go, C#, Python이 있습니다. 

- {:.p-header} AWS Batch
  
  배치 처리를 손쉽게 실행하는 서비스입니다. 
  
  배치 작업을 위한 서버가 따로 필요하지 않고 코드 실행이 가능하기 때문에 Lambda와 비슷하게 보이지만, Lambda에는 900초라는 하나의 요청당 최대 실행 시간 제한이 있기 때문에 시간이 오래 걸리는 로직이나 복잡한 처리는 AWS Batch로 처리하는 것이 좋습니다. 
  
  처리는 잡(job)이라는 단위로 등록합니다. 

- {:.p-header} AWS Elastic Beanstalk
  
  AWS의 PaaS의 일종으로, 어플리케이션을 신속하게 배포(deploy)하고 관리하기 위한 서비스입니다.

  어플리케이션을 업로드 하기만 하면 로드밸런싱, 모니터링등에 대한 세부 정보를 자동으로 처리해 줍니다. 


<br>
> #### Storage

- {:.p-header} Amazon S3
  
  정식명칭은 Amazon Simple Storage Service로, 연간 99.99%의 가용성과 99.999999999%의 내구성을 제공하도록 설계된 객체 스토리지 서비스입니다. 
  
  단순한 웹 서비스 인터페이스를 사용하여 웹에서 언제 어디서나 원하는 양의 데이터를 저장하고 검색할 수 있습니다. 

- {:.p-header} Amazon EFS
  
  정식명칭은 Amazon Elastic File System으로, AWS 클라우드 서비스와 온프레미스(On-Premise) 리소스에서 사용할 수 있는 간단하고 확장 가능한 Linux기반 파일 시스템을 제공합니다. 
  
  최대 수천개의 EC2 인스턴스로부터 동시에 액세스 하는 것이 가능하며, 페타바이트 단위까지 자동적으로 확장/축소 가능합니다. 

  {:.p-footnote}
  :: 온프레미스 :: 전통적인 소프트웨어 사용방식으로, 소프트웨어 등의 솔루션을 클라우드와 같은 원격 환경이 아닌 자체적으로 보유한 서버에 직접 설치해 운영하는 방식을 의미합니다. 

- {:.p-header} AWS Storage Gateway
  
  온프레미스(On-Premise) 어플리케이션을 클라우드 기반의 스토리지에 연결하는 서비스로서, 기존의 환경과 AWS 스토리지 인프라를 원활하고 안전하게 통합할 수 있게 해줍니다. 

  파일기반, 볼륨 기반, 테이프 기반 스토리지 솔루션을 제공합니다. 


<br>
> #### Database

- {:.p-header} Amazon RDS
  
  정식명칭은 Amazon Relational Database Service로, AWS RDS를 사용하면 클라우드에서 관계형 데이터베이스를 간편하게 설정하고 운영할 수 있습니다. 
  
  Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle, Microsoft SQL Server 6개의 DB엔진 중에 선택 가능합니다. 

- {:.p-header} Amazon DynamoDB
  
  key-value형태의 NoSQL 데이터 베이스 서비스입니다. 
  
  어떤 규모에서도 일관되게 10 밀리초 미만의 응답 시간을 제공합니다. AWS Management Console을 사용하여 리소스 사용률과 성능 측정치를 모니터링 할 수 있습니다.

- {:.p-header} Amazon DocumentDB (MongoDB 호환 데이터베이스)
  
  완전관리형 문서 지향 데이터 베이스 서비스입니다. 
  
  어플리케이션 코드나 드라이버는 MongoDB와 같은 것을 사용하는 것이 가능합니다. 


<br>
> #### Networking & Contents Delivery

- {:.p-header} Amazon VPC
  
  정식명칭은 Amazon Virtual Private Cloud로, AWS 클라우드에 가상 네트워크를 작성하기 위한 서비스입니다. 
  
  VPC는 AWS 클라우드의 다른 가상 네트워크와 논리적으로 분리되어 있고, Amazon EC2 인스턴스와 같은 AWS 리소스를 VPC에서 실행할 수 있습니다. 

  서브넷, Elastic IP, 보안 그룹, 게이트웨이, 라우팅 테이블과 같은 기능을 이용할 수 있습니다.  

- {:.p-header} Amazon CloudFront
  
  정적 혹은 동적 웹 컨텐츠를 짧은 지연 시간과 빠른 전송 속도로 전송하는 고속 컨텐츠 전송 네트워크(CDN) 서비스입니다. 
  
  정적 컨텐츠를 캐싱하면 최종 사용자가 웹 사이트를 방문했을 때 빠르고 안정적인 환경을 제공하는 데 필요한 성능과 규모를 확보할 수 있습니다. 

  HTTP/HTTPS를 경유하는 웹 컨텐츠와 RTMP를 사용한 미디어 파일의 스트리밍이 지원됩니다. 

- {:.p-header} Amazon Route 53
  
  DNS(Domain Name System)서버 서비스입니다. 
  
  사용자의 요청을 Amazon EC2 인스턴스, Amazon S3 버킷 등 AWS에서 실행되는 인프라에 효과적으로 연결 가능합니다. 그리고 반대로 사용자를 AWS 외부의 인프라로 라우팅 하는데도 이용 가능 합니다. 
  
  리소스의 상태 체크, 다양한 라우팅 규칙, 레코드 타입등이 지원됩니다. 

- {:.p-header} Amazon API Gateway
  
  RESTful API를 작성, 게시, 유지 관리, 배포, 모니터링 가능한 완전관리형 서비스입니다. 
  
  웹 사이트, Lambda함수 그리고 그 밖의 AWS서비스를 End Point로써 이용 가능합니다. 

  Swagger파일에 의한 import/export도 지원됩니다. 


<br>
> #### Development Tool

- {:.p-header} AWS CodeDeploy
  
  어플리케이션 배포 자동화 서비스입니다. 소프트웨어 배포를 자동화 함으로써 오류가 발생하기 쉬운 수동 작업을 제거할 수 있습니다. 
  
  S3나 Github, Bitbucket에 저장되어 있는 컨텐츠를 EC2나 온프레미스 서버, Lambda등에 배포 가능합니다. 

- {:.p-header} AWS CodePipeline
  
  소스 코드의 빌드, 테스트, 배포, 승인을 시각화/자동화 하는 지속적 전달 서비스(Continuous Delivery Service)입니다.

  각 스테이지(빌드, 테스트)에서 액션(CodeCommit, CodeBuild등과 연결하는 서비스)을 지정함으로써, 릴리즈 프로세스를 구축하는 것이 가능합니다. 


<br>
> #### Mornitoring & Management

- {:.p-header} Amazon CloudWatch
  
  AWS 리소스 혹은 AWS에서 실행되고 있는 어플리케이션을 모니터링하기 위한 서비스입니다. 
  
  지표- metric(CPU사용률, 네트워크 I/O등)이나 로그를 수집해서, SNS나 AutoScaling과 연계하는 알람을 작성할 수 있습니다. 

  또한 AWS 리소스의 변경 이벤트를 감시해서, Lambda등의 타겟에 대해 실시간으로 통지하는 것이 가능합니다. 

- {:.p-header} AWS CloudFormation
  
  AWS 오케스트레이션 서비스입니다. 
  
  JSON이나 YAML로 작성된 설정 템플릿 파일을 이용하여, AWS의 각 리소스의 프로비저닝과 구성을 담당합니다. 

<br>
> #### Analysis

- {:.p-header} Amazon Athena
  
  표준 SQL을 이용해서 S3 데이터를 분석가능한 인터랙티브 쿼리 서비스입니다. 

  CSV, JSON, ORC, Avro등의 다양한 표준 데이터 포맷 데이터를 분석할 수 있으며, 자동적으로 병렬 실행됩니다. 

- {:.p-header} Amazon Elasticsearch Service
  
  완전관리형 Elasticsearch 클라우드 서비스입니다. 
  
  Kibana나 Logstash가 지원되며, S3, Kinesis, DynamoDB와 통합도 가능합니다. 
  
  
<br><br>
