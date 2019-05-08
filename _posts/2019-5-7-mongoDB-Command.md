---
layout: post
title:  "MongoDB 커맨드 및 메소드 정리"
description: "mongoDB에서 데이터를 조작하기 위해 사용하는 mongoDB Shell에서 사용하는 커맨드나 메소드를 정리하고자 합니다."
date:   2019-05-07 12:00:00 +0800
categories: DATABASE
tags: MongoDB NoSQL
lang: ko_KR
comments: true
---

MongoDB는 개발과 스케일링을 용이하게 하기 위해 설계된 오픈소스 도큐먼트 지향 데이터베이스 입니다. Key-Value store 방식을 사용하며 흔히 말하는 NoSQL 데이터베이스 중의 하나입니다.

MongoDB에 저장된 데이터를 조작하는 방법 중 하나로, 커맨드라인 인터페이스인 MongoDB Shell을 사용할 수 있습니다.

해당 포스트에서는 MongoDB Shell에서 사용할 수 있는 커맨드나 메소드에 대해서 알아보고자 합니다.


<br>
> ##### MongoDB의 구조

![이미지 없음](/assets/res/mongodb_shell1.png){: style="padding-left: 1rem" width="50%"}

위의 이미지와 같이 collection은 table, document는 record라고 생각하시면 됩니다.


<br>
> ##### DB 조작

- {:.p-header} DB 작성
  
    ~~~ javascript
    $ mongo dbname
    use dbname
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    use dbname으로 사용 DB 변경한다.

- {:.p-header} DB 삭제
  
    ~~~ javascript
    db.dropDatabase();
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} DB 리스트 확인
  
    ~~~ javascript
    show dbs
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}
  

<br>
> ##### Collection 조작

- {:.p-header} Collection 리스트 확인
  
    ~~~ javascript
    show collections
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} Collection 생성
  
    ~~~ javascript
    db.createCollection('MYCOL');
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    MYCOL이라는 Collection을 생성한다.

- {:.p-header} Collection 삭제
  
    ~~~ javascript
    db.MYCOL.drop();
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    MYCOL이라는 Collection을 삭제한다.

- {:.p-header} Collection명 변경
  
    ~~~ javascript
    db.MYCOL.renameCollection("MYCOL2");
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    MYCOL이라는 Collection의 이름을 MYCOL2로 변경한다.


<br>
> ##### Document 업데이트

- {:.p-header} Collection에 Document 추가
  
    ~~~ javascript
    db.MYCOL.insert( { name: 'kim', money: 999999 } );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    Key-Value 형식으로 Document(record)를 삽입한다.

- {:.p-header} money 필드 업데이트
    
    MongoDB에서는 기본적으로 처음에 매칭된 Document 만을 업데이트 합니다.
    {: style="font-weight: bold; color: brown;"}
  
    ~~~ javascript
    db.MYCOL.update(
        { name: 'kim' }, 
        { $set: { money: 9999999 } }
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    name필드의 값이 'kim'인 첫 번째 Document의 money필드의 값을 9999999 로 변경한다.

- {:.p-header} money 필드 삭제
    
    ~~~ javascript
    db.MYCOL.update(
        { name: 'kim' }, 
        { $unset: { money: "" } }
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} money 필드에 10 더하기
    
    ~~~ javascript
    db.MYCOL.update(
        { name: 'kim' }, 
        { $inc: { money: 10 } }
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} money 필드 이름을 mymoney로 변경
    
    ~~~ javascript
    db.MYCOL.update(
        { name: 'kim' }, 
        { $rename: { money: "mymoney" } }
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} 매칭되는 모든 Document의 업데이트
    
    ~~~ javascript
    db.MYCOL.update(
        { name: 'kim' }, 
        { $set: { money: 100000 } },
        { multi: true }
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    필드 name이 'kim'인 모든 Document의 money 필드의 값을 100000으로 변경한다.
    
- {:.p-header} Collection내의 모든 Document 삭제
    
    ~~~ javascript
    db.MYCOL.remove({});
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} Collection내의 특정 Document 삭제
    
    ~~~ javascript
    db.MYCOL.remove(
        { name: 'kim' }
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    필드 name이 'kim'인 모든 Document를 삭제한다.


<br>
> ##### Document 검색

- {:.p-header} 모든 Document 검색
  
    ~~~ javascript
    db.MYCOL.find();
    db.MYCOL.find( {} );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} 특정 Document 검색
  
    ~~~ javascript
    db.MYCOL.find( { name: 'kim' } );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    필드 name이 'kim'인 모든 Document를 검색한다.

- {:.p-header} 필드 money가 30보다 큰 모든 Document 검색
  
    ~~~ javascript
    db.MYCOL.find( 
        { money: { $gt: 30 } }
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}
   
- {:.p-header} 필드 name이 'kim'이 아닌 모든 Document 검색
  
    ~~~ javascript
    db.MYCOL.find( 
        { name: { $ne: 'kim' } }
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} 필드 name이 'kim' or 'lee' 인 모든 Document 검색
  
    ~~~ javascript
    db.MYCOL.find( 
        { name: { $in: ['kim', 'lee'] } }
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} AND 연산자를 이용한 검색
  
    ~~~ javascript
    db.MYCOL.find({
        $and: [
            { name  : 'kim' },
            { money : { $gt : 1000000  } }
        ]
    });
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} OR 연산자를 이용한 검색
  
    ~~~ javascript
    db.MYCOL.find({
        $or: [
            { name  : 'kim' },
            { money : { $lt : 1000000  } }
        ]
    });
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}


<br>
> ##### 그 외의 Document 조작


- {:.p-header} 취득할 필드를 지정
    
    위처럼 아무런 지정을 하지 않으면, 불필요한 필드를 포함한 모든 필드를 취득하게 된다. 
    
    이때 표시 : 1, 비표시 : 0 로 지정하여 필요한 필드만 지정할 수 있다.
  
    ~~~ javascript
    db.MYCOL.find(
        { name: { $in: ['kim', 'lee'] } }, 
        { money: 1 }
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    필드 name이 'kim' or 'lee' 인 모든 Document의 money 필드만 취득한다.

- {:.p-header} 검색되는 Document 개수 지정
  
    ~~~ javascript
    db.MYCOL.find( { name: 'kim' } ).limit(3);
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} Document 개수 획득
  
    ~~~ javascript
    db.MYCOL.find( { name: 'kim' } ).count();
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} Document 정렬
    
    오름차순 : 1, 내림차순 : -1
  
    ~~~ javascript
    db.MYCOL.find( { name: 'kim' } ).sort( { money: -1 } );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

- {:.p-header} Document 필드 복제
    
    $를 prefix로 사용하면, 특정 필드의 값을 참조한다.
    {: style="font-weight: bold; color: brown;"}
  
    ~~~ javascript
    db.MYCOL.aggregate(
        [
            { $addFields: {
                "name": "$client_id",
                "secret": "$client_secret"
            }},
            { "$out": "MYCOL" }
        ]
    );
    ~~~
    {: style="padding-left: 0rem; margin-top:-1rem;"}

    name, secret 필드를 생성해서 name 필드에는 client_id 필드의 값, secret 필드에는 client_secret 필드의 값을 할당한다.




<br><br>
