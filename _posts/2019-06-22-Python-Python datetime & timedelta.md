---
layout: post
title:  "[Python datetime & timedelta]"
subtitle:   "python django datetime timedelta "
categories: python
tags:
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## Python datetime

### datetime 모듈
- 날짜 및 시간을 사용하게 해주는 라이브러리
- `import datetime` 으로 모듈 호출
- `datetime.now()` => 현재 시각 표시

### timedelta 클래스
- 파이썬에 `datetime` 모듈에 `timedelta`
- 날짜, 시간 연산에 사용
	- `timedelta`는 시간의 양을 나타내고 그 시간의 양은 다른 시간 및 날짜 시간 객체와의 연산이 가능함

**실습**

```django
# ./manage.py shell 테스트
import datetime

now = timezone.now()
now
>>> datetime.datetime(2019, 6, 19, 6, 56, 25, 648789, tzinfo=<UTC>)

day = datetime.timedelta(days=1)

now + day
>>> datetime.datetime(2019, 6, 20, 6, 56, 25, 648789, tzinfo=<UTC>)
```
