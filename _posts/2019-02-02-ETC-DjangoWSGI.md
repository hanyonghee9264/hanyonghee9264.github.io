---
layout: post
title:  "[Django WSGI 개념]"
subtitle:   "wsgi"
categories: etc
tags: etc
comments: true
---

## WSGI

**호출 가능한 어플리케이션**
즉, 내가 작성한 코드와 어플리케이션 서버가 통신하는데 사용됨

- WSGI 서버는 설정을 읽어 application의 경로를 가져옴

	> 즉, runserver 명령은 WSGI_APPLICATION 설정에서 경로를 가져옴



## 설정 모듈 구성
개발 환경과 배포 환경을 나누고 싶으면 `DJANGO_SETTINGS_MODULE` 환경 변수를 이용

만약 이 변수가 설정되어 있지 않다면 기본 wsgi.py는 mysite.settings를 가리킴
> 여기서 mysite는 프로젝트 이름

이것이 `runserver`가 기본적으로 설정 파일을 탐색하는 방법

----
## Project 구성
```django
Nginx
	location / => include	=> uwsgi_params
	=> uWSGI
	   uwsgi.ini
	   		wsgi => config.wsgi.production
	   		=> Django
	   		   config.wsgi.production => os.environ.setdefault()
	   		   		= export DJANGO_SETTINGS_MODULE=production과 같은 효과
	   		   		= ENV DJANGO_SETTINGS_MODULE production
```
