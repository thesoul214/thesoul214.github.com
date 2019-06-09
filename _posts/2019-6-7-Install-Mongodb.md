---
layout: post
title:  "로컬에 MongoDB 설치 메모 (MacOS)"
description: "mongodb, macos, osx, nosql, brew install, mongodb 설치, mongodb 사용법, 몽고DB"
date:   2019-06-09 12:00:00 +0800
categories: DATABASE
tags: MongoDB NoSQL
lang: ko_KR
comments: true
---


MacOS에 Homebrew를 이용해서 MongoDB를 설치하는 순서와 간단한 메모입니다.


<br>
> ##### 설치

- {:.p-header} Homebrew로 인스톨
   
   패키지 업데이트 후, MongoDB 인스톨하기 
  
   ~~~shell
   $ brew update
   $ brew install mongodb
   ~~~

   인스톨 중에 에러가 나는 경우 brew doctor 명령어를 이용해서 에러 조사 후 수정할 수 있습니다.


- {:.p-header} 설치 확인
   
   정상적으로 인스톨 되었는지 확인하기
   ~~~shell
   $ mongo --version

   # 결과 : 
   MongoDB shell version v4.0.3
   git version: 7ea530946fa7880364d88c8d8b6026bbc9ffa48c
   allocator: system
   modules: none
   build environment:
      distarch: x86_64
      target_arch: x86_64
   ~~~


- {:.p-header} 설정 파일
   
   설정 파일은 /usr/local/etc/mongod.conf 에서 확인 가능합니다.

   로그와 데이터를 저장하고자 하는 위치를 path, dbPath로 지정 가능합니다.

   ~~~shell
   systemLog:
      destination: file
      path: /usr/local/var/log/mongodb/mongo.log
      logAppend: true
   storage:
      dbPath: /usr/local/var/mongodb
   net:
      bindIp: 127.0.0.1
   ~~~


- {:.p-header} MongoDB 실행 및 종료
   
   brew를 이용해서 서비스 형태로 MongoDB를 실행할 수 있습니다.

   이렇게 서비스 형태로 실행할 경우, 위의 mongod.conf에서 설정한 대로 환경을 구성하게 됩니다.

   정상적으로 실행되면 path, dbPath로 지정한 디렉토리에 관련 파일이 자동 생성됩니다.
   ~~~shell
   $ brew services start mongodb
   ~~~

   정상적으로 실행 되었는지 확인하기
   ~~~shell
   $ ps -ef | grep mongo

   # 결과 :
   501 61303     1   0  4:32AM ??         0:00.82 /usr/local/opt/mongodb/bin/mongod --config /usr/local/etc/mongod.conf
   ~~~

   하기의 명령어를 이용해서 MongoDB를 종료할 수 있습니다.
   ~~~shell
   $ brew services stop mongodb
   ~~~

   
<br>
> ##### 관리 유저 등록과 인증 기능 설정

- {:.p-header} 관리 유저 등록
   
   ~~~shell
   # MongoDB에 접속
   $ mongo

   # DB 리스트 확인
   $ show dbs

   # admin DB 사용
   $ use admin
   # 결과 :
   # switched to db admin

   # 관리 유저 등록
   $ db.createUser({
       user:"admin", 
       pwd:"XXXXXXXX", 
       roles:[{role:"root", db:"admin"}]
     })
   # 결과 :
   #Successfully added user: {
   #   "user" : "admin",
   #   "roles" : [
   #      {
   #         "role" : "root",
   #         "db" : "admin"
   #      }
   #   ]
   #}

   # 생성된 유저 확인
   $ db.getUsers()
   # 결과 :
   #[
   #   {
   #      "_id" : "admin.admin",
   #      "user" : "admin",
   #      "db" : "admin",
   #      "roles" : [
   #         {
   #            "role" : "root",
   #            "db" : "admin"
   #         }
   #      ],
   #      "mechanisms" : [
   #         "SCRAM-SHA-1",
   #         "SCRAM-SHA-256"
   #      ]
   #   }
   #]
   ~~~


- {:.p-header} 인증 기능 설정
   
   인증 기능을 설정하기 위해서 설정파일을 수정해 주어야 합니다.
   ~~~shell
   # 설정 파일에 진입
   $ sudo vi /usr/local/etc/mongod.conf

   # 설정파일 제일 밑에 하기의 구문을 추가
   security:
      authorization: enabled

   # MongoDB 재시작
   $ brew services restart mongodb
   ~~~

- {:.p-header} 인증 기능 확인 및 MongoDB에 접속
   
   인증 기능을 설정한 MongoDB에 접속하는 방법은 두가지가 있습니다.

   1. 생성한 관리 유저를 통해서 MongoDB에 접속하기
   2. MongoDB에 접속한 후, 생성한 관리 유저로 로그인하기
   
   <br>
   첫 번째 방법
   ~~~shell
   # admin 유저로 admin DB에 접속하기
   $ mongo -u "admin" -p "XXXXXXXX" --authenticationDatabase "admin"
   ~~~

   <br>
   두 번째 방법
   ~~~shell
   # MongoDB에 접속
   $ mongo

   # admin DB 사용
   $ use admin

   # admin 유저로 로그인(이용)
   $ db.auth("admin", "XXXXXXXX" )
   # 1이 return되면 로그인 성공
   ~~~

   첫 번째 방법을 다른 유저들과 같이 사용하는 서버에서 사용하게 될 경우, 비밀번호가 노출되므로 사용하지 않는 것이 좋습니다.

   <br>

   MongoDB에 관한 포스트 모음 : <a href="{{site.url}}/tags#mongodb_cap" target="_blank">Tags - MongoDB</a>
   {: style="font-weight: bold; color: brown; text-align: center;"}


<br><br>
