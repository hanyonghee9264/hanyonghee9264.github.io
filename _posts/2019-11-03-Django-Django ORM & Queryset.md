---
layout: post
title:  "[Django ORM & Queryset]"
subtitle:   "django orm queryset"
categories: Django
tags: Django ORM Queryset
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.


## Django ORM & Queryset

### Django ORM
**Object Relational Mapping**

- `Django` 데이터베이스는 종류와 독립적인 형태로 객체화 한다.
	- 객체(object)의 관계(relational)를 연결해주는 것
- 높은 생산성(빠른 개발속도 및 기간)으로 개발가능
- `Model Class`를 통해 객체를 만들고 이 객체를 통해 `DB`에 접근할 수 있음

 > SQL을 몰라도 사용이 가능. 다만 SQL도 추후 공부

### Queryset

```
>>> Post.objects.all() # Post 모델의 모든 데이터 가져오기 => Django ORM
>>> <QuerySet [<Post: 박보영>, <Post: 아이유>]>
```
- 전달받은 모델의 객체 목록
- objects: `Model Manager`로 `DB`와 `Django Model`사이를 연결
	- `objects`를 사용하여 다수의 데이터를 가져오는 함수를 사용할 때 반환되는 객체
	- 데이터를 읽고, 필터를 걸거나 정렬 가능
