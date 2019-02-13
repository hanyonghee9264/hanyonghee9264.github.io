---
layout: post
title:  "[Docker Logging 및 eb ssh 실시간 로그확인]"
subtitle:   "docker logging & eb ssh logging"
categories: aws
tags: deploy
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## Docker Logging
`logging` 을 `fomatter`나 `handler` 등 코드에 직접 지정해줘도 되지만

그렇게 한다면 소스 코드가 복잡해지고 유지 보수하기 힘들어지며 가독성이 떨어진다.

**이러한 이유로** `settings.py(production.py)`에 `Logging`을 지정하여 사용하면 편리해진다.

---
### Logging

```logging
# 로그폴더 생성
LOG_DIR = os.path.join(ROOT_DIR, '.log')
# 로그 폴더가 존재하지 않으면 폴더 생성
if not os.path.exists(LOG_DIR):
    os.makedirs(LOG_DIR, exist_ok=True)

LOGGING = {
    'version': 1,
    'formatters': {
        'default': {
            'format': '[%(levelname)s] %(name)s (%(asctime)s)\n\t%(message)s'
        },
    },
    'handlers': {
        'file_error': {
            'class': 'logging.handlers.RotatingFileHandler',
            'level': 'ERROR',
            'filename': os.path.join(LOG_DIR, 'error.log'),
            'formatter': 'default',
            'maxBytes': 10485760,
            'backupCount': 10,
        },
        'file_info': {
            'class': 'logging.handlers.RotatingFileHandler',
            'level': 'INFO',
            'filename': os.path.join(LOG_DIR, 'info.log'),
            'formatter': 'default',
            'maxBytes': 10485760,
            'backupCount': 10,
        },
        # console에 StreamHandler는
        # 터미널창에 표시
        'console': {
            'class': 'logging.StreamHandler',
            'level': 'INFO',
        },
    },
    'loggers': {
        'django': {
            'handlers': [
                'file_error',
                'file_info',
                'console',
            ],
            'level': 'INFO',
            'propagate': True,
        }
    }
}


```

> 참고 [Django 공식문서](https://docs.djangoproject.com/en/2.1/topics/logging/)

----

## Docker에서 logging

- `docker run --rm -it -p 7000:80 --name <별칭> <docker_image>`
- `docker exec <별칭> tail /srv/project/.log/error.log`
	- `tail` 을 통해 로그가 쌓인 기록을 확인할 수 있음
- `docker exec <별칭> tail -f /srv/project/.log/error.log`
	- `-f` 를 추가해주면 실시간으로 `error` 로그를 확인 할 수 있음
- `docker exec <별칭> tail -f /srv/project/.log/error.log /srv/project/.log/info.log`
	- `tail` 뒤에 `error` 로그 이외에 `info` 로그 2가지를 실시간으로 확인 가능

----

## EB ssh 명령어를 통한 로그 실시간 확인

- `--command` 를 통해 실시간 확인 가능

	> SSH 세션을 시작하는 대신 지정된 인스턴스에서 셀 명령을 실행

- `eb ssh --command 'pwd'` : 작업중인 디렉토리 경로 확인
- `eb ssh --command 'sudo docker ps'` : `docker run` 상태인 컨테이너 확인
- `eb ssh --command 'sudo docker ps -q` : 실행중인 컨테이너 ID 만 표시
- `eb ssh --command 'sudo docker exec $(sudo docker ps -q) tail -f /srv/project/.log/error.log /srv/project/.log/info.log'` : 실시간으로 `error` 로그 및 `info` 로그 확인 가능
