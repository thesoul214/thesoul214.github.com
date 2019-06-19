---
layout: post
title:  "Python(Pymongo)로 CRUD 처리"
description: "mongodb, 몽고DB, python, crud, nosql, create, update, delete, read, insert, select, DB, pymongo"
date:   2019-06-18 12:00:00 +0800
categories: DATABASE PYTHON
tags: MongoDB Pymongo Python
lang: ko_KR
comments: true
---


python으로 MongoDB를 조작하기 위한 라이브러리인 pymongo에 대해서 알아보겠습니다.

<br>
> ##### 데이터베이스에 접속하기

1. Mongo Client를 이용하여 DB에 연결
   
   ~~~python
   from pymongo import MongoClient

   # localhost에 설치한 mongodb에 디폴트 port인 27017 포트로 접속.
   client = MongoClient('localhost', 27017)

   # 인증기능을 설정한 경우 아이디와 비번을 지정해 주어야 함.
   client["admin"].authenticate("admin", "XXXXX")
   ~~~

2. 데이터베이스에 접속
   
   ~~~python
   # test 데이터베이스에 접속
   db = client.test
   ~~~   

3. collection 사용
   
   ~~~python
   # person collection 
   col = db.person
   ~~~  


<br>
> ##### CREATE(C)
  
mongodb는 데이터를 json object 형태로 저장합니다. 

collection이 존재하지 않는 경우, insert 메소드는 새로운 collection을 자동으로 생성합니다. 

또한 모든 insert 메소드를 사용할 경우, mongodb가 자동으로 _id를 필드를 생성하여 ObjectID를 할당해 줍니다.

1. insert_one()
   
   단수의 document를 collection에 입력합니다.

   ~~~python
   col.insert_one({
     name: "kim",
     salary: 400
   })
   ~~~

2. insert_many()
   
   복수의 documents를 collection에 입력합니다.

   ~~~python
   col.insert_many([
     { name: "kim", salary: 400},
     { name: "lee", salary: 300},
     { name: "choi", salary: 500}
   ])
   ~~~

3. insert()
   
   단수 혹은 복수개의 documents를 collection에 입력합니다.

   ~~~python
   # single document
   col.insert({ name: "kim", salary: 400})

   # array of documents
   col.insert([{ name: "kim", salary: 400}, { name: "lee", salary: 300}])
   ~~~


<br>
> ##### READ(R)

두 가지의 메소드를 이용해서 collection에서 documents를 읽어올 수 있습니다.

1. find()
   
   모든 documents를 읽어옵니다. default로 cursor object가 리턴됩니다.
   ~~~python
   col.find()
   # <pymongo.cursor.Cursor object at 0x7f8fc1853890>
   ~~~

2. find_one()
   
   해당되는 첫번째 document를 읽어옵니다.

   ~~~python
   col.find_one()
   # {u'salary': 400, u'_id': ObjectId('57611a711aa303032ad5ba9b'), u'name': u'kim'}
   ~~~

<br>
위의 find메소드들의 조건은 다음과 같이 지정해 줄 수 있습니다.

~~~python
# object를 리턴
col.find_one({"name": "kim"})

# cursor object를 리턴
col.find({"name": "kim"})
~~~

상기는 name 필드가 kim인 document를 읽어오는 코드입니다.


<br>
> ##### UPDATE(U)

기본적으로 UPDATE 관련 메소드들은 다음과 같은 syntax를 가집니다.

`<method_name>(filter, update, upsert=False)`

- `filter : update 하기 위한 document를 검색하는 조건`

- `update : update 내용`

- `upsert(optional) : true인 경우, filter에서 지정한 조건에 매칭되는 document가 없으면 데이터를 새롭게 입력합니다.`


<br>
update() 메소드를 제외한 나머지는 UpdateResult Object가 리턴됩니다.

~~~python
col.update_one({"name":"kim"}, {"$set":{"salary":1000}})
# <pymongo.results.UpdateResult object at 0x7f8fbe7fb910>

col.update_many({"name":"kim"}, {"$set":{"salary":1000}})
# <pymongo.results.UpdateResult object at 0x7f8fbe7fb7d0>

col.update({"name":"kim"}, {"$set":{"salary":1000}})
# {'updatedExisting': False, u'nModified': 0, u'ok': 1, u'n': 0}

col.replace_one({"name":"kim"}, {"salary":1000})
# <pymongo.results.UpdateResult object at 0x7f8fbe7fb910>
~~~

상기는 name 필드가 kim인 document의 salary 필드를 1000으로 업데이트 하는 코드입니다.


<br>
> ##### DELETE(D)

document를 삭제할 수 있는 두 가지의 메소드는 DeleteResult object를 리턴합니다.

기본 syntax

`<method_name>(condition)`

~~~python
col.delete_one({"name":"kim"})
# <pymongo.results.DeleteResult object at 0x7f8fbe7fba00>

col.delete_many({"name":"kim"})
# <pymongo.results.DeleteResult object at 0x7f8fbe7fb960>
~~~

상기는 name 필드가 kim인 document를 삭제하는 코드입니다.


<br>

MongoDB에 관한 포스트 모음 : <a href="{{site.url}}/tags#mongodb_cap" target="_blank">Tags - MongoDB</a>
{: style="font-weight: bold; color: brown; text-align: center;"}

<br><br>
