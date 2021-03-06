---
layout: post
title:  "[Python Chapter2 14]"
subtitle:   "14. None을 반환하기보다는 예외를 일으키자"
categories: python
tags: effectivepython
comments: true
---
> 본 포스팅은 **`파이썬코딩의기술`** 이라는 책을 참고하여 **개인 공부**를 하면서 정리하고 있습니다.
>
> 문제 될 시 삭제하겠습니다.

# Chapter 2
### 14. None을 반환하기보다는 예외를 일으키자

파이썬 프로그래머들은 유틸리티 함수를 작성할 때 반환 값 `None`에 특별하는 의미를 부여하는 경향이 있음

예를 들어 어떤 숫자를 다른 숫자로 나누는 헬퍼 함수를 생각해보자

```python
# 0으로 나누는 경우에는 결과가 정의되어 있지 않기 때문에
# None을 반환하는게 자연스럽다
def divide(a, b):
	try:
		return a / b
	except ZeroDivisionError:
		return None

# 위의 함수를 사용하는 코드는 반환 값을 다음과 같이 해석한다
result = divide(x, y)
if result is None:
	print('Invalid inputs')
```

그런데 분자가 0이 되면? 반환 값도 0이 되어버린다(분모가 0이 아닐 경우)

그러면 if 문과 같은 조건에서 결과를 평가할 때 문제가 될 수 있다

오류인지 알아내려고 None 대신 실수로 False에 해당하는 값을 검사할 수도 있다

```python
x, y = 0, 5
result = divide(x, y)
if not result:
	print('Invalid inputs')    # 잘못됨!
```

이 예는 `None`에 특별한 의미가 있을 때 파이썬 코드에서 흔히 하는 실수

또한 함수에서 `None`을 반환하면 오류가 일어나기 쉬운 이유다

이런 오류가 일어나는 상황을 줄이는 방법은 두 가지다

1. 반환 값을 두개로 나눠서 튜플에 담는다
	- 튜플의 첫 번째 부분은 작업의 성공 유무를 보여줌
	- 두 번째 부분은 계산 된 실제 결과

	```python
	def divide(a, b):
		try:
			return True, a / b
		except ZeroDivisionError:
			return False, None
	```

	이 함수를 호출하는 쪽에서는 튜플을 풀어야 한다

	따라서 나눗셈의 결과만 얻을 게 아니라 튜플에 들어 있는 상태 부분까지 고려해야 함

	```python
	success, result = divide(x, y)
	if not success:
		print('Invalid inputs')
	```
	문제는 호출자(사용하지 않을 변수 _)가 튜플의 첫 번째 부분에 놓일 경우

	얼핏 보면 코드가 잘못된 것 같지 않지만, 결과는 그냥 `None`을 반환하는 것 만큼 나쁘다

	```python
	_, result = divide(x, y)
	if not result:
		print('Invalid inputs')
	```

2. 절대로 None을 반환하지 않는 것
	- 대신에 호출하는 쪽에 예외를 일으켜서 예외처리

	```python
	# 여기서는 호출하는 쪽에 입력값이 잘못됐음을 알리려고
	# ZeroDivisionError 를 ValueError로 변경함
	def divide(a, b):
		try:
			return a / b
		except ZeroDivisionError as e:
			raise ValueError('Invalid inputs') from e
	```
	이제 호출하는 쪽에서는 잘못된 입력에 대한 예외를 처리해야 한다

	호출하는 쪽에서 더는 함수의 반환 값을 조건식으로 검사할 필요가 없다

	함수가 예외를 일으키지 않았다면 반환 값은 문제가 없으며, 예외 처리 코드도 깔끔해진다

	```python
	x, y = 5, 2
	try:
		result = divide(x, y)
	except ValueError:
		print('Invalid inputs')
	else:
		print('Result is %.1f' % result)

	>>>
	Result is 2.5
	```

**정리**

- 특별한 의미를 나타내려고 `None`을 반환하는 함수가 오류를 일으키기 쉬운 이유는
`None`이나 다른 값(예를 들면 0이나 빈 문자열)이 조건식에서 False로 평가되기 때문
- 특별한 상황을 알릴 때 `None`을 반환하는 대신에 예외를 일으키자
- 문서화가 되어 있다면 호출하는 코드에서 예외를 적절하게 처리할 것이라고 기대할 수 있다
