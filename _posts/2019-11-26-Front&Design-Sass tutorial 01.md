---
layout: post
title:  "[Frontend Sass]"
subtitle:   "front css Sass"
categories: front&design
tags: front
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.


## Sass

- `Sass`는 `CSS` **pre-processor**
	- **pre-processor**란?
		- `CSS` 문서 작성 시 코드의 중복이 많아 유지 보수가 힘들어짐
		- 이런 문제점을 해결하기 위해 변수, 함수, 상속 뜽 해결

		> 결론 : CSS 반복을 줄여 시간을 절약

----
#### Example

- 결과 View
	![Front-Sass](/assets/img/front/Sass_example_resultview01.png)

- `html` 코드

	```html
	<!DOCTYPE html>
	<html>
	<link rel="stylesheet" href="mystyle.css">
	<body>

	<h1>Hello World</h1>

	<p>This is a paragraph.</p>

	</body>
	</html>

	```

- `CSS` 코드 & `Sass`

	```html
	# sass 코드
	/* Define standard variables and values for website */
	 $bgcolor: lightblue;
	 $textcolor: darkblue;
	 $fontsize: 18px;

	/* Use the variables */
	body {
	  background-color: $bgcolor;
	  color: $textcolor;
	  font-size: $fontsize;
	}

	# css에 해당 sass코드가 저장되는 모습
	body {
	  background-color: lightblue;
	  color: darkblue;
	  font-size: 18px;
	}
	```
	- 해당 `Sass`에서 결과 `View`에 보이는 것처럼 표준 변수 및 값을 정의
		- **$** 표시
		- 주로 자주 사용하는 바탕색, 폰트사이즈, 폰트색
