---
layout: post
title:  "[Django Celery amqp error]"
subtitle:   "Django Celery error"
categories: django
tags: deploy
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## Celery-tutorial
`celery-tutorial` 가이드를 보며 진행하던 중 오류가 계속 발생함

![Celery-error-01](/assets/img/django/celery-error01.png)

**`consumer: Cannot connect to amqp://guest:**@127.0.0.1:5672//:`**

- `celery`가 동작은 했지만 위와 같은 에러가 2초 간격으로 발생함
- `amqp`에 연결할 수 없다는 에러
	- `celery`의 `task`를 받아주는 `broker`가 없다는 뜻

> 해결하기 위해 rabbitmq 를 실행
>
> 즉, redis-server가 켜져있는 상태에서 celery 구동
