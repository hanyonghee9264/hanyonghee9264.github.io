---
layout: post
title:  "[Python Chapter1 07]"
subtitle:   "07. map과 filter 대신 리스트 컴프리헨션을 사용"
categories: python
tags: effectivepython
comments: true
---
> 본 포스팅은 **`파이썬코딩의기술`** 이라는 책을 참고하여 **개인 공부**를 하면서 정리하고 있습니다.
>
> 문제 될 시 삭제하겠습니다.

# Chapter 1
### 7. map과 filter 대신 리스트 컴프리헨션을 사용하자
- 파이썬에서는 한 리스트에서 다른 리스트를 만들어내는 간결한 문법이 있음
- 이 문법을 사용한 표현식을 리스트 컴프리헨션(list comprehension)

```python
# 리스트에 있는 각 숫자의 제곱을 계산
# 계산식과 루프로 돌릴 입력 시퀀스를 작성
a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
squares = [x**2 for x in a] # 리스트컴프리헨션은 오른쪽에서 왼쪽으로 보면 이해하기 쉬움
print(squares)

>>>
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

```

간단한 연산에는 리스트 컴프리헨션이 내장 함수 `map`보다 명확함

`map`을 쓰려면 계산에 필요한 `lamda` 함수를 생성해야 해서 깔끔해 보이지 않음

---
**여기서 잠깐!**

내장함수 `map`

- map(f, iterable)은 **함수(f)와 반복 가능한(iterable) 자료형**을 입력으로 받음
- map은 입력받은 자료형의 각 요소가 **함수 f에 의해 수행된 결과를 묶어서 리턴**하는 함수

	```python
	def two_times(x):
   		return x*2

	>>> list(map(two_times, [1, 2, 3, 4]))
	[2, 4, 6, 8]
	```

람다(lamda)함수

- 한 줄 짜리 표현식으로 이루어지며, 반복문이나 조건문 등의 제어문은 포함될 수 없음
- 함수이지만 정의/호출이 나누어져 있지 않으며 표현식 자체가 바로 호출된다.
	- lambda <매개변수> : <표현식>

	```python
	# 람다함수의 사용
	>>> (lambda x : x*x)(5)
	25

	# 람다함수를 사용해 함수 정의
	>>> f = lambda x : x*x
	>>> f(5)
	25
	```
----

```python
squares = map(lambda x: x ** 2, a)

```

`map`과 달리 리스트 컴프리헨션을 사용하면 입력 리스트에 있는 아이템을 간편하게 걸러낼 수 있음

```python
# 예를 들어 2로 나누어 떨어지는 숫자의 제곱만 계산
# 조건식을 추가해서 계산
even_squares = [x**2 for x in a if x % 2 == 0]
print(even_squares)

>>>
[4, 16, 36, 64, 100]
```

내장 함수 `filter`를 `map`과 연계해서 사용해도 같은 결과를 얻을 수 있지만 가독성이 안 좋음

```python
alt = map(lambda x: x**2, filter(lambda x: x % 2 == 0, a))
assert even_squares == list(alt)
```

딕셔너리와 셋(set)에서도 리스트 컴프리헨션에 해당하는 문법이 존재

컴프리헨션 문법을 쓰면 알고리즘을 작성할 때 파생되는 자료 구조를 쉽게 생성

```python
chile_ranks = {'ghost': 1, 'habanero': 2, 'cayenne': 3}
rank_dict = {rank: name for name, rank in chile_ranks.items()}
chile_len_set = {len(name) for name in rank_dict.values()}
print(rank_dict)
print(chile_len_set)

>>>
{1: 'ghost', 2: 'habanero', 3: 'cayenne'}
{8, 5, 7}
```

**정리**

- 리스트 컴프리헨션은 추가적인 `lambda`표현식이 필요 없어서 내장 함수인 `map`이나 `filter`를 사용하는 것보다 명확함
- 리스트 컴프리헨션을 사용하면 입력 리스트에서 아이템을 간단히 건너뛸 수 있음
- 딕셔너리와 셋(set)도 컴프리헨션 표현식을 지원함
