---
layout: post
title:  "[Myproject-coffeecalorie Redis 및 Celery-beat 사용]"
subtitle:   "Django celery-beat redis "
categories: django
tags: celery-beat redis
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## 개인프로젝트[Coffeecalorie]

### Celery 사용 [Celery-beat]

- 비동기적으로 실시간 업무를 처리 하기 위해 `Celery-beat` 를 사용
	- 일정 기간 원하는 날짜에 맞춰 크롤링을 해야하기 때문에(신메뉴 업데이트 문제... 등)

> 개인적으로 `django-crontab`을 사용하려 했지만 굉장히 오랜 기간을 삽질하며 문제에 직면했기 때문에 대안으로..


프로젝트 구조
![Myapp-Celery-structure](/assets/img/django/myproject-celery-structure.png)

1. Celery - Redis 설치
> Mac OS

	- `Homebrew` 를 통해 굉장히 간단하게 설치 가능

		`brew install redis`

	- `redis-server`의 정상동작이 궁금하면?

		`redis-server` 켜두고 새 터미널을 열어 `redis-cli`를 입력 후 `ping` 이라고 쳤을 때 `pong`이라고 나오면 정상동작


2. `Django`에 `celery-beat` 및 `celery-results` 설치

	```
	pip install django-celery-beat
	pip install django-celery-results

	# 설치 후 Django settings.py INSTALLED_APPS 에 추가
	INSTALLED_APPS = [
		'django_celery_beat',
	    'django_celery_results',
	    ...
	]
	```
**여기서 잠깐**

	구글링을 통해 참고하던 중 `CELERY`의 브로커 URL 이나 다른 기본 셋팅 값을 `settings.py`에 명시하라 했지만 안됐음

	> 해당 celery.py 에 넣어주니 정상동작..

3. Celery App 설정

	config 아래 `celery.py` 생성

	```django
	import os

	from celery import Celery
	from celery.schedules import crontab

	# Django의 세팅 모듈을 Celery의 기본으로 사용하도록 등록
	os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'config.settings')
	app = Celery(
	    'config',
	    broker='redis://localhost:6379/0',
	)

	# 문자열로 등록한 이유는 Celery Worker가 자식 프로세스에
	# configuration object를 직렬화하지 않아도 된다는 것 때문
	# namespace = 'CELERY'는 모든 celery 관련한 configuration key가
	# 'CELERY_' 로 시작해야함을 의미함
	app.config_from_object('django.conf:settings', namespace='CELERY')

	# task 모듈을 모든 등록된 Django App config에 load함
	app.autodiscover_tasks()

	app.conf.update(
	    BROKER_URL='redis://localhost:6379',
	    CELERY_TASK_SERIALIZER='json',
	    CELERY_ACCEPT_CONTENT=['json'],
	    CELERY_RESULT_SERIALIZER='json',
	    CELERY_TIMEZONE='Asia/Seoul',
	    CELERY_ENABLE_UTC=False,
	    CELERYBEAT_SCHEDULE={
	        'add-first-of-every-month': {
	            'task': 'coffeeshop.tasks.starbucks_crawling',
	            'schedule': crontab(hour='*', minute='*'),
	            'args': (),
	        },
	    }
	)


	# bing=True 옵션을 주게 되면
	# 아래 함수의 인자에 self가 추가됨 ==> 'self'는 관용적으로 쓰임
	# 이 앱의 실행시켰을때에 대한 정보가 담겨져옴

	@app.task(bind=True)
	def debug_task(self):
	    print('Request: {0!r}'.format(self.request))

	```

4. `__init__.py` 수정

	```django
	from .celery import app as celery_app

	# Django가 시작할 때 shared_task가 이 앱을 이용할 수 있도록
	# app이 항상 import

	__all__ = ('celery_app',)
	```
5. `tasks.py` 생성

	```django
	from coffeeshop.crawling.starbucks import 	Starbucks
	from config import celery_app


	@celery_app.task
	def starbucks_crawling():
	    Starbucks.get_coffee_info()
	    print('종료')
	```
