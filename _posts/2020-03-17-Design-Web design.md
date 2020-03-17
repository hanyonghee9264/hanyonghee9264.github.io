---
layout: post
title:  "[반응형 웹 디자인 그리드]"
subtitle:   "grid webdesign"
categories: front&design
tags: design
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## 반응형 웹디자인 그리드
- 사용자의 폭이 넓을 수록 **작은 해상도**에 맞추는 것이 좋다
- 우리나라에서 가장 많이 사용하는 해상도 비중은 **1920x1080**

---

### Column & Gutter
`Column` : 콘텐츠를 포함하는 부분

`Gutter` : `Column`과 `Column`사이의 간격

- `Column`의 넓이는 고정된 값
- 1개 이상의 `Column`이 조합하여 콘텐츠의 크기를 결정
- `Gutter`는 `Column`부분에 `CSS`의 `Margin`값이 10px씩 적용된다는 개념과 같음

**주의**

- 2개 이상의 `Column`은 맞닿는 내부의 `Gutter`를 포함한 넓이를 가짐
	- 단, 양쪽에 `Gutter` 값을 가지는 `Column`의 성질은 병합된 `Column` 수가 늘어도 그대로 유지

### Container
콘텐츠 영역의 가장 큰 폭의 기준

- 안에 담기는 내용물의 최대 폭을 말한다
- `Column`의 수를 지정
- `Container`의 내부에는 콘텐츠를 담아내는 한 개 이상의 `Column`이 존재
- 가장 보편적인 틀은 **1200px**

	> 전체 폭: 1920px / 컨테이너: 1200px

**TIP**

- 보통 12단을 보편적으로 많이 사용
	- 가이드라인 잡기도 편하고, 퍼블리싱할 때도 편함

	> 2, 3, 4, 6단으로 했을 때 정수값이 나옴

- **단을 너무 많이 쪼개면 반응형에 취약함**
- `Gutter`는 30px을 보편적으로 많이 사용

	> ex) Container: 1200px | Columns: 12 | Offset: Center | Gutter: 30px | Column: 72px

**!! 정해진 규칙은 없지만 본인이 원하는 규칙을 만들어서 작업하는 습관을 들이자**
