---
layout: post
title:  "Rails5 Model - Active Record 기본"
description: "rails Activerecord, model, find, find_by, sql, orm, query method, distinct, limit, join"
date:   2019-07-01 12:00:00 +0800
categories: RUBY
tags: Ruby Rails
lang: ko_KR
comments: true
---


Rails5에서 model을 개발하는데 필요한 Active Record에 대해서 정리하겠습니다.

<br>
> ##### 데이터 취득 기본
   
- {:.p-header} 기본 키를 이용해서 검색 - find 메소드
   
   가장 기본적인 find 메소드입니다.
   ~~~ruby
   find(keys)
   # keys:기본 키(배열로도 지정 가능)
   ~~~

   Rails : `User.find([2, 5, 10])`

   변환된 SQL : `SELECT users.* FROM users WHERE users.id IN (2, 5, 10)`

<br>
- {:.p-header} 임의의 키를 이용해서 검색 - find_by 메소드
   
   find 메소드에서 파생된 메소드로, 임의의 열을 키로 테이블을 검색해서 처음에 검색된 1건만 취득합니다.

   ~~~ruby
   find_by(key: value)
   # key : 검색할 필드명, value : 검색값
   ~~~

   Rails : `User.find_by(name: 'kim', city: 'hiroshima')`

   변환된 SQL : `SELECT users.* FROM users WHERE users.name = 'kim' AND users.city = 'hiroshima' LIMIT 1`



<br>
> ##### 보다 복잡한 조건을 이용해서 검색하기 - Query Method

Query Method를 이용하여, 보다 복잡한 조건식을 지정하거나 정렬, 그룹화, 범위 추출, 결합 등을 할 수 있습니다.

{:.table_width_600}
|---
| Method | 개요 |
|:-:|:-:|
| where | 필터링 조건 |
| not | 부정 조건 |
| or | or 조건 |
| order | 정렬 |
| reorder | 정렬식 덮어쓰기 |
| select | 열 지정 |
| distinct | 중복 없이 레코드 취득 |
| limit | 추출하고자 하는 레코드 수 제한 |
| offset | 추출하고자 하는 레코드의 시작점, limit와 같이 사용됨 |
| group | 특정 키를 이용하여 결과를 그룹핑 |
| having | GROUP BY에 추가 제약을 설정 |
| joins | 다른 테이블과 결합 |
| left_outer_joins | 다른 테이블과 left outer join |
| includes | 관련있는 모델을 한꺼번에 취득 |
| readonly | 취득한 오브젝트를 읽기 전용으로 |
| none | 빈 결과셋을 취득 |

<br/>
위와 같이 다양한 종류의 Query Method가 존재하며, 위의 메소드들을 사용하면 ActiveRecord::Relation 오브젝트가 리턴됩니다.


- {:.p-header} where 메소드
   
   사용법 1

   Rails : `User.where(city: ['hiroshima', 'seoul'])`

   변환된 SQL : `SELECT users.* FROM users WHERE users.city IN ('hiroshima', 'seoul')`

   <br/>
   사용법 2

   Rails : `User.where('city = ?', 'hiroshima')`

   변환된 SQL : `SELECT users.* FROM users WHERE users.city = 'hiroshima'`

<br/>
- {:.p-header} not 메소드
   
   Rails : `User.where.not(city: 'hiroshima')`

   변환된 SQL : `SELECT users.* FROM users WHERE users.city != 'hiroshima'`

<br/>
- {:.p-header} or 메소드 (Rails 5 이상)
   
   Rails : `User.where(city: 'hiroshima').or(User.where(name: 'kim'))`

   변환된 SQL : `SELECT users.* FROM users WHERE ( users.city = 'hiroshima' OR users.name = 'kim' )`
   
<br/>
- {:.p-header} order 메소드
   
   Rails : `User.where(city: 'hiroshima').order(name: :desc)`

   변환된 SQL : `SELECT users.* FROM users WHERE users.city = 'hiroshima' ORDER BY users.name DESC`

<br/>
- {:.p-header} select 메소드
   
   Active Record는 기본적으로 모든 열을 취득합니다.(SELECT * FROM ...)

   하지만 레코드의 모든 열이 필요하지 않는 경우도 많으므로, 취득하고자 하는 특정 열을 명시적으로 지정하는 것도 가능합니다.

   Rails : `User.where(city: 'hiroshima').select(:name, :type)`

   변환된 SQL : `SELECT users.name, users.type FROM users WHERE users.city = 'hiroshima'`

<br/>
- {:.p-header} distinct 메소드
   
   결과셋으로부터 중복된 행을 소거합니다.
   
   Rails : `User.select(:name).distinct`

   변환된 SQL : `SELECT DISTINCT users.name FROM users`

<br/>
- {:.p-header} none 메소드
   
   빈 결과셋을 취득합니다. 
   
   view에 빈 Relation(NullRelation)을 전달함으로써, 에러를 방지하고자 할 때 사용합니다.

   Rails : `User.none`

<br><br>
