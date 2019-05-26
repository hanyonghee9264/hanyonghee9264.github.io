---
layout: post
title:  "[Djangogirls staticfiles 및 URL]"
subtitle:   "Djangogirls staticfiles url "
categories: django
tags: djangogirls staticfiles
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## DJANGOGIRLS

### STATICFILES 경로

```
request -> runserver -> (Django) -> urlresolver(config.urls)
	r'^$'		-> blog.views.post_list
	r'^admin/'	-> [admin]
```

1. 개발모드에서 (settings.DEBUG가 True)
2. settings.STATIC_URL에 해당하는 request는
3. settings.STATICFILES_DIRS리스트 내의 경로목록 각각의 하위 위치에서 해당 파일을 검색, 존재할 경우 그 파일을 리턴해준다

### URL TAG

- URL resolve

```djangogirls
request(url) -> urlresolver -> (resolve) -> view function

# ex) /blog-posts/
	-> urls
		r'^blog-posts/$'
			-> view (post_list)
```

- URL reverse

```djangogirls
template -> {% url '<url name>' %} -> (reverse) -> url

# ex) {% url 'post-list' %}
	-> urls
		name='post-list'
		r'^/blog-posts/$'
			-> /blog-posts/
```
