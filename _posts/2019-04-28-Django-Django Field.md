---
layout: post
title:  "[Django Field - Decimalfield 정리]"
subtitle:   "django field Decimalfield"
categories: django
tags:
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## Django Field

### DecimalField

10 진수 표현으로, `python`에서 `Decimal` 인스턴스로 나타냄

`django.db.models.fields.DecimalField`에 해당

> DecimalField(max_digits, decimal_places, coerce_to_string=None, max_value=None, min_value=None)

- `max_digits` : 숫자에 허용되는 최대 자릿수를 말하며, `None`이거나 `decimal_places`보다 크거나 같은 정수여야 함
- `decimal_places` : 숫자와 함께 저장할 소수 자릿수를 말함
- `coerce_to_string`
	- 표현식에 문자열 값을 반환해야하는 경우 `True`로 설정
	- `Decimal` 객체를 반환해야하는 경우 `False`로 설정
	- 기본값은 `COERCE_DECIMAL_TO_STRING` 설정 키와 같은 값으로, 오버라이드 하지 않으면 `True`
	- `serializer`가 `Decimal` 객체를 반환하면 최종 출력 형식은 **렌더러에 의해 결정**
	- `localize`를 설정하면 값이 `True`로 설정
- `max_value` : 제공 된 숫자가 위 값보다 크지 않은지 확인
- `min_value` : 제공 된 숫자가 위 값보다 작지 않음을 검증
