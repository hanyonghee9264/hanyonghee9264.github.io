---
layout: post
title:  "[Django Admin 커스텀]"
subtitle:   "admin custom"
categories: django
tags:
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## Django Admin Custom

기본적으로 장고가 제공함 (settings.py에 INSTALLED_APPS에서 확인가능)

```django
INSTALLED_APPS = [
    'django.contrib.admin',
]
```

### Model Admin 등록
admin.ModelAdmin 상속을 통해 커스텀 방법

```admin
from django.contrib import admin

from .models Coffee

class CoffeeAdmin(admin.ModelAdmin):
    list_per_page = 25
```

> 그치만 이 방법이 잘 적용이 안되서 또 다른 방법을 찾아봄
>
> 알고보니 admin.site.register(모델명, 해당 클래스명) 을 빼먹음

### Model Admin 등록2
데코레이터(decorator) 형태로도 등록이 가능

> 이 방식을 더 선호하며 개인적으로 데코레이터로 했을 때 해결함

```admin
from django.contrib import admin

from .models Coffee

class CoffeeAdmin(admin.ModelAdmin):
    list_per_page = 25
```

----

이 외 많은 옵션들이 있었음

### Admin 커스텀 옵션
#### List
- `list_per_page`: Admin 페이지에서 보여지는 수량 (default 100)
- `list_display`: Admin 목록에 보여질 필드 목록
- `list_editable`: 수정 할 필드 목록
- `list_filter`: 필터 옵션을 제공할 필드 목록
- `search_fields` : 제공 된 필드를 사용하여 검색
- `ordering` : 필드 목록 정렬
