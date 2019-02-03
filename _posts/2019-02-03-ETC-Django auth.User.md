---
layout: post
title:  "[Django auth.User]"
subtitle:   "auth.user"
categories: etc
tags: etc
comments: true
---

### auth.User

- 문자열
- 앞은 애플리케이션명
- 뒤는 모델 클래스명
- Django가 기본으로 제공하는 User 클래스
- 커스텀마이징을 하지 않으면 createsuperuser도 auth.User로 생긴다



##### `settings.AUTH_USER_MODEL`

auth.User라 적지 않고 위처럼 적는 이유는? User클래스를 커스텀할 수도 있고 그렇지 않을 수도 있다. 이 모두를 대비를 위해 그렇게 쓴다. 그래서 settings에 있는 값을 쓰면 모델 클래스를 고민할 필요없다.
