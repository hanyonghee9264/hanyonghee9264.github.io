---
layout: post
title:  "[Django API drf-yasg]"
subtitle:   "django api drf-yasg"
categories: Django
tags: Django drf-yasg api
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.


## drf-yasg

- `django restframework`를 이용하여 `API` 문서작성시 자동으로 생성

	> 이전의 `gitbook`은 일일이 작성해야하는 번거로움

- `django restframework`에 다양한 자동 문서화 도구가 있음
	- 그 중 최근 공모전을 통해 팀원 분을 통해 새로 알게 되어 정리하자

### 패키지 설치

```
pip install -u drf-yasg
pip install flex
```
- **drf-yasg** : `API` 자동 문서화 패키지
- **flex** :  `JSON schema`를 체크하는 검사 패키지
	- `JSON`은 좀 더 쉽게 데이터를 교환하고 저장하기 위하여 만들어진 데이터 교환 표준
	- 이때 `JSON` 데이터를 전송받는 측에서는 전송받은 데이터가 적법한 형식의 데이터인지를 확인할 방법이 필요
	- 따라서 적법한 `JSON` 데이터 형식을 기술한 문서를 `JSON` 스키마(schema)라고 함

	> [참고 사이트](http://tcpschool.com/json/json_schema_schema){: target="_blank" }

### settings.py

```
INSTALLED_APPS = [
   'drf_yasg',
]
```

### urls.py
```
from django.conf.urls import url
from django.contrib import admin
from django.urls import path, include
from rest_framework import permissions
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

schema_view = get_schema_view(
    openapi.Info(
        title="test API",
        default_version='v1',
        description="test",
        contact=openapi.Contact(email="<your email>"),
        license=openapi.License(name="MIT License"),
    ),
    validators=['flex', 'ssv'],
    public=True,
    permission_classes=(permissions.AllowAny,),
)
urlpatterns = [
	# drf API docs
    url(r'^swagger(?P<format>\.json|\.yaml)$', schema_view.without_ui(cache_timeout=0), name='schema-json'),
    url(r'^swagger/$', schema_view.with_ui('swagger', cache_timeout=0), name='schema-swagger-ui'),
    url(r'^redoc/$', schema_view.with_ui('redoc', cache_timeout=0), name='schema-redoc'),

    path('admin/', admin.site.urls),

]
```
- 4개의 엔드포인트
 - `JSON`의 `API` **/swagger.json**
 - `YAML`의 `API` **/swagger.yaml**
 - `swagger-ui` **/swagger/**
 - `ReDoc` **/redoc/**

### API 문서 커스텀
- `API`문서 자동화에 사용될 내용은 주석을 기반으로 작성
- `DRF`의 `ViewSet` **Class 바로 아래 작성** 또는 **각 method 바로 아래**
- 각 함수 바로 앞에 주석을 사용하면 그 내용을 바탕으로 `markdown`형식으로 입력

```
report_query_parameter = openapi.Parameter('request', openapi.IN_QUERY,
                                           description="특정 Request의 Report를 list\nex) report/?request=1",
                                           type=openapi.TYPE_INTEGER)

class ReportViewSet(viewsets.ModelViewSet):
    """
    Report 관련 REST API 제공

    ---
    ... 내용 ...
    """

    def get_queryset(self):
		"""
		Report의 쿼리셋을 불러오는 API

		---
		... 내용 ...
		"""

    @swagger_auto_schema(manual_parameters=[report_query_parameter])
    def list(self, request, *args, **kwargs):
        return super().list(request, *args, **kwargs)


```
 - 위와 같이 **class 바로 아래** 또는 **method 바로 아래** 작성
 - **주의**
 	- 주석의 제목을 적음 : Report 관련 REST API 제공
 	- 한 줄 띄우고
 	- 칸을 나누는 `---` 입력 후 그 아래 내용

	> 참고 views.py에 정리 시 해당 Serializer가 generic으로 정의하지 않으면 일일이 작성해야하는걸로 보임
