---
layout: post
title:  "[Python Chapter1 11]"
subtitle:   "11. 이터레이터를 병렬로 처리하려면 zip을 사용"
categories: python
tags: effectivepython
comments: true
---
> 본 포스팅은 **`파이썬코딩의기술`** 이라는 책을 참고하여 **개인 공부**를 하면서 정리하고 있습니다.
>
> 문제 될 시 삭제하겠습니다.

# Chapter 1
### 11. 이터레이터를 병렬로 처리하려면 zip을 사용하자

파이썬에서 관련 객체로 구성된 리스트를 많이 사용한다는 사실은 쉽게 알 수 있다

리스트 컴프리헨션을 사용하면 소스 리스트(source list)에 표현식을 적용하여

파생 리스트(derived list)를 쉽게 얻을 수 있다

```python
names = ['Cecilia', 'Lise', 'Marie']
letters = [len(n) for n in names]
```
파생 리스트의 아이템과 소스 리스트의 아이템은 서로 인덱스로 연관되어 있음

따라서 두 리스트를 병렬로 순회하려면 소스 리스트인 `names`의 길이만큼 순회하면 된다

```python
longest_name = None
max_letters = 0

for i in range(len(names)):
	count = letters[i]
	if count > max_letters:
		longest_name = names[i]
		max_letters = count

print(longest_name)

>>>
Cecilia
```
전체 루프 문이 별로 보기 안 좋음

`names`와 `letters`를 인덱스로 접근하면 코드를 읽기 어려워짐

루프의 인덱스 i로 배열에 접근하는 동작이 두 번 일어난다

`enumerate`를 사용하면 문제점을 약간 개선할 수 있지만 여전히 완벽하지는 않음

```python
for i, name in enumerate(names):
    count = letters[i]
    if count > max_letters:
        longest_name = name
        max_letters = count
```
파이썬은 위 코드를 좀 더 명료하게 하는 내장 함수 `zip`을 제공

파이썬 3에서 `zip`은 지연 제너레이터(lazy generator)로 이터레이터 두 개 이상을 감싼다

`zip` 제너레이터는 각 이터레이터로부터 다음 값을 담은 튜플을 얻어옴

`zip` 제너레이터를 사용한 코드는 다중 리스트에서 인덱스로 접근하는 코드보다 훨씬 명료

```python
for name, count in zip(names, letters):
    if count > max_letters:
        longest_name = name
        max_letters = count
```

내장 함수 `zip`을 사용할 때는 두 가지 문제가 있음

1. 파이썬 2에서 제공하는 `zip`이 제너레이터가 아니라는 점
	- 제공한 이터레이터를 완전히 순회해서 생성한 모든 튜플을 반환하기 때문에 메모리를 많이 사용해서 프로그램이 망가질 수 있음
	- 파이썬2 에서 매우 큰 이터레이터를 `zip`으로 묶어서 사용하려고 한다면 내장 모듈 `itertools`의 `izip`을 사용해야 한다
2. 입력 이터레이터들의 길이가 다르면 `zip`이 이상하게 동작함
	- 예를 들어 `names` 리스트에 다른 이름을 추가했지만 `letters`의 카운터 업데이트를 잊었다고 가정
	- 두 입력 리스트에 `zip`을 실행하면 예상치 못한 결과 발생

	```python
	names.append('Rosalind')
	for name, count in zip(names, letters):
	    print(name)

	>>>
	Cecilia
	Lise
	Marie

	# 새 아이템 'Rosalind'가 결과에 없다
	# zip은 감싼 이터레이터가 끝날 때까지 튜플을 계속 넘겨준다
	# 이 방식은 이터레이터들의 길이가 같을 때는 제대로 동작함
	```
	그래서 만약 `zip`을 적용할 이터러블들의 길이가 같음을 확신할 수 없다면

	내장 모듈 `itertools`의 `zip_longest`를 사용하는 방안을 고려하자


**정리**

- 내장 함수 `zip`은 여러 이터레이터를 병렬로 순회할 때 사용할 수 있음
- 파이썬 3의 `zip`은 튜플을 생성하는 지연 제너레이터, 파이썬 2의 `zip`은 전체 결과를 튜플 리스트로 반환
- 길이가 다른 이터레이터를 사용하면 `zip`은 그 결과를 조용히 잘라냄
- 내장 모듈 `itertools`의 `zip_longest`함수를 쓰면 여러 이터레이터를 길이에 상관없이 병렬로 순회할 수 있음
