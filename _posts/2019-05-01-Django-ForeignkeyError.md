---
layout: post
title:  "[Related Field got invalid lookup: icontains error]"
subtitle:   "Django Foreignkey error"
categories: django
tags: error
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## Related Field got invalid lookup: icontains 에러

개인 프로젝트 `admin` 커스텀 중 위와 같은 에러가 발생

**Related Field got invalid lookup: icontains**

해당 모델에 대한 `ForeignKey`가 검색 필드에 포함되어 발생된 에러

> 내 코드

```django
# models.py
class Coffee(models.Model):
    COFFEESHOP_LIST = (
        ('STARBUCKS', 'Starbucks'),
    )
    category = models.ForeignKey(
        CoffeeCategory,
        on_delete=models.SET_NULL,
        verbose_name='카테고리',
        null=True,
    )
    coffeeshop_list = models.CharField(max_length=20, choices=COFFEESHOP_LIST)
    name = models.CharField('커피', max_length=100)
    .....

class CoffeeImage(models.Model):
    location = models.ImageField('커피이미지', upload_to=coffee_path, blank=True)
    coffee = models.ForeignKey(
        Coffee,
        on_delete=models.CASCADE,
        verbose_name='커피'
    )
    ......
```
```django
# admin.py
@admin.register(CoffeeImage)
class CoffeeImageAdmin(admin.ModelAdmin):
    list_per_page = 25
    list_display = (
        'coffee', 'location'
    )
    search_fields = ('coffee',)
```

- 단순하게 해당 `Foreignkey`필드와 관련된 명 혹은 참조하는 모델의 필드를 가져오려 했지만 에러 발생
	- 필드 값이 존재하지 않는다... 이런저런...

**해결**

- `CoffeeImage`의 데이터의 검색을 위해 `Coffee` 모델의 `name`필드의 데이터를 보여주려고`coffee__name` 와 같은 방식으로 검색에 대한 에러 문제 해결함
