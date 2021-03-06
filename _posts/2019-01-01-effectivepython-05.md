---
layout: post
title:  "[Python Chapter1 05]"
subtitle:   "05. 시퀀스를 슬라이스하는 방법"
categories: python
tags: effectivepython
comments: true
---
> 본 포스팅은 **`파이썬코딩의기술`** 이라는 책을 참고하여 **개인 공부**를 하면서 정리하고 있습니다.
>
> 문제 될 시 삭제하겠습니다.

# Chapter 1
### 5. 시퀀스를 슬라이스하는 방법을 알자
- 파이썬은 시퀀스를 `슬라이스(slice:자르기)`해서 조각으로 만드는 문법 제공
- 최소한의 노력으로 시퀀스 아이템의 `부분집합(subset)`에 접근'
- `list`, `str`, `bytes`
- 기본형태는 `somelist[start:end]`, start인덱스는 포함, end는 제외
	- 즉, start는 이상 end는 미만

```python
a = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']

print('First four:', a[:4])
print('Last four:', a[-4:])
print('Middle two:', a[3:-3])

>>>
First four: ['a', 'b', 'c', 'd']
Last four: ['e', 'f', 'g', 'h']
Middle two: ['d', 'e']

# 리스트를 슬라이스할 때 인덱스 0을 생략
assert a[:5] == a[0:5]

# 리스트의 끝까지 슬라이스할 때도 마지막 인덱스 생략
assert a[5:] == a[5:len(a)]

# 리스트의 끝을 기준으로 오프셋을 계산할 때는 음수로 슬라이스 하는게 편함
a[:]		# ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
a[:5]		# ['a', 'b', 'c', 'd', 'e']
a[:-1]		# ['a', 'b', 'c', 'd', 'e', 'f', 'g']
a[4:]		# 		      ['e', 'f', 'g', 'h']
a[-3:]		#			    ['f', 'g', 'h']
a[2:5]		#	    ['c', 'd', 'e']
a[2:-1]		#	    ['c', 'd', 'e', 'f', 'g']
a[-3:-1]	#		    	    ['f', 'g']

# 슬라이싱은 start와 end 인덱스가 경계를 벗어나도 적절하게 처리해줌
first_twenty_items = a[:20]	# ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
last_twenty_items = a[-20:]	# ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']

# 하지만 인덱스를 직접 접근하면 예외 발생
a[20]

>>>
IndexError: list index out of range

# 슬라이싱의 결과는 새로운 리스트 반환
# 원본리스트에 들어 있는 객체에 대한 참조는 유지됨
# 슬라이싱 결과를 수정해도 원본 리스트에는 아무런 영향 없음

b = a[4:]
print('Before:		', b)
b[1] = 99
print('After:		', b)
print('No change:	', a)

>>>
Before:		['e', 'f', 'g', 'h']
After:		['e', 99, 'g', 'h']
No change:	['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']

# a, b = c[:2] 같은 튜플 할당과 달리 슬라이스 할당의 길이는 달라도 됨
# 슬라이스의 앞, 뒤 값은 유지 되며, 새로 들어온 값에 맞춰 늘어나거나 줄어듬
print('Before ', a)
a[2:7] = [99, 22, 14]
print('After ', a)

>>>
Before ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
After  ['a', 'b', 99, 22, 14, 'h']

# 시작과 끝 인덱스를 모두 생략하고 슬라이스하면 원본 리스트의 복사본을 가짐
b = a[:]
assert b == a and b is not a

# 슬라이스에 시작과 끝 인덱스를 지정하지 않고 할당하면
# 슬라이스의 전체 내용을 참조 대상의 복사본으로 대체함
b = a
print('Before', a)
a[:] = [101, 102, 103]
assert a is b		# 여전히 같은 리스트 객체
print('After', a)	# 이제 다른 내용을 담음

>>>
Before ['a', 'b', 99, 22, 14, 'h']
After  [101, 102, 103]

```
> 예를 들어 somelist[-n:]이라는 구문은 somelist[-3:] 처럼 n이 1보다 클 때는 제대로 작동함

> **그러나** n이 0이어서 somelist[-0:]이 되면 원본 리스트의 복사본 생성

---

**정리**

- `start` 인덱스에 0을 설정하거나 `end` 인덱스에 시퀀스의 길이를 설정하지 말자
	- a[0:5]  => a[:5]
	- a[5:len(a)] => a[5:]
- 슬라이싱은 범위를 벗어난 `start`나`end` 허용
- `list`슬라이스에 할당하면 원본 시퀀스에 지정한 범위를 참조 대상의 내용으로 대체
(**이 때, 길이가 달라도 동작함**)
