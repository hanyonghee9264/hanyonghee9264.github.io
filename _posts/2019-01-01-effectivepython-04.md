---
layout: post
title:  "[Python Chapter1 04]"
subtitle:   "04. 복잡한 표현식 대신 헬퍼 함수 작성"
categories: python
tags: effectivepython
comments: true
---
> 본 포스팅은 **`파이썬코딩의기술`** 이라는 책을 참고하여 **개인 공부**를 하면서 정리하고 있습니다.
>
> 문제 될 시 삭제하겠습니다.

# Chapter 1
### 4. 복잡한 표현식 대신 헬퍼 함수를 작성하자.
- 파이썬은 간결한 문법을 이용하면 한 줄로 쉽게 작성이 가능
- 헬퍼함수는 개발자의 편의를 위해 씀

> 예) URL에서 쿼리 문자열을 디코드

```python
# 각 쿼리 문자열 파라미터는 정수 값을 표현
from urllib.parse import parse_qs

my_values = parse_qs('red=5&blue=0&green=', keep_blank_values=True)
print(repr(my_values))

>>>
{'red': ['5'], 'blue': ['0'], 'green': ['']}

# 딕셔너리에 get 메서드를 사용하여 다른 값 반환
print('Red:		', my_values.get('red'))
print('Green:	', my_values.get('green'))
print('Opacity: ', my_values.get('opacity'))

>>>
Red:		['5']
Green:		['']
Opacity:	None

# 파라미터가 없거나 비어 있으면 기본값으로 0을 할당
# 1. bool 표현식으로 처리
# 쿼리 문자열: 'red=5&blue=0&green='
red = my_values.get('red', [''])[0] or 0
green = my_values.get('green', [''])[0] or 0
opacity = my_values.get('opacity', [''])[0] or 0
print('Red:		%r' % red)
print('Green:	%r' % green)
print('Opacity: %r' % opacity)

>>>
Red:		'5'
Green:		0
Opacity:	0

# 위에 코드는 표현식이 읽기도 어려우며 Red는 str형태
# 따라서 내장 함수 int로 처리해서 문자열을 정수 값으로 파싱
red = int(my_values.get('red', [''])[0] or 0)

# 위에 코드는 가독성이 좋지 않음
# 따라서 코드가 짧아 좋을 수는 있지만 한 줄에 모든 코드를 넣는 건 큰 의미가 없음 (가독성 문제)

# if/else 조건식을 이용하여 명확하게 표현
red = my_values.get('red', [''])
red = int(red[0]) if red[0] else 0

# 하지만 여러 줄에 걸친 if/else문을 대체할 정도로 명확하지는 않음
# 만약 모든 로직을 펼쳐서 보면 더 복잡해 보일 수 있음
green = my_values.get('green', [''])
if green[0]:
	green = int(green[0])
else:
	green = 0

# 이 로직을 반복해서 사용해야 한다면 헬퍼 함수를 만드는게 좋음
def get_first_int(values, key, default=0):
	found = values.get(key, [''])
	if found[0]:
		found = int(found[0])
	else:
		found = default
	return found

green = get_first_int(my_values, 'green')

```
> Http URL 파라미터를 다루려면 `parse_qs()` 메서드를 사용

>`parse_qs()` 는 지정된 쿼리스트링을 해석하여 `dict()` 로 반환

---

**정리**

- 표현식이 복잡해지기 시작하면?
	- 해당 표현식을 작은 조각으로 분할하고 로직을 `헬퍼 함수`로 옮기자
- 무조건 짧은 코드보다 가독성을 선택
- if/else 표현식을 이용하면 or, and 와 같은 `bool` 연산자를 사용할 때보다 읽기 편한 코드를 작성할 수 있다.
