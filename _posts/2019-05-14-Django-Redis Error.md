---
layout: post
title:  "[RDB version error]"
subtitle:   "Django redis rdb "
categories: django
tags: redis rdb
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## redis Can’t handle RDB format version 9 Fatal error loading the DB: Invalid argument error

### 에러 메세지
![Redis-rdb](/assets/img/django/redis-rdb-error.png)
- 일반적으로 낮은 버전은 높은 버전의 rdb와 호환되지 않는다.
- 내 `Docker`의 `redis` 버전이 높기 때문에 서버 `redis`와 공유로 인한 문제

**해결**

- 로컬에서 `build`하는 과정에서 `dump.rdb`라는 파일이 존재했었음
- 이 파일을 삭제한 후 `redis-server`를 하니 다시 정상적으로 돌아가는 것을 확인

> Docker build를 할 경우 불필요한 파일이나 사전에 gitignore를 통해 해결하자
