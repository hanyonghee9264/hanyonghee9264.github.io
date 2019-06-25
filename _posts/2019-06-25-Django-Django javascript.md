---
layout: post
title:  "[Django CoffeeCalorie Project - Javascript]"
subtitle:   "django project javascript"
categories: django
tags: django javascript
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## Django Javascript [location]

#### 문제점
- `Django template`에서 `select`를 사용하던 중 해당 페이지 변동없이 `request.GET`을 사용하여 처리를 하려했음

	- `Django session`을 이용하면 될거 같은데.... 추후에 도전하기로..

- `Javascript`로 해결
	- 내 분야가 아니라 추후 공부 후 내용 정리 할 것

---
### Javascript location
- `javascript` 의 `object`에 `location`이라는 것이 있음
 - URL 정보를 가져오는 객체
- URL 전체 또는 일부의 정보를 가져올 수 있음

**기본형태**

> location.<속성 or 메서드>

> location.<속성> = 지정값;

**참고**
[링크 클릭](https://wayhome25.github.io/django/2017/03/01/django-99-my-first-project-5/){: target="_blank" }

----

```
<script>
$(document).ready(function(){
  var calorie = getUrlParameter('calorie');
  if(calorie == 'high'){
    $('.calorie-high').prop('selected', 'selected')
  }else if(calorie == 'low'){
    $('.calorie-low').prop('selected', 'selected')
  }
});
</script>

# 위에 참고사이트를 보고 한 후 동작은 원하는대로 됨
# 위에 있는 javascript 의 로직은 이해가 됨
# 그치만 원하는 결과만 출력이 되고 셀렉트박스는 여전히 초기화
```

---


```javascript
var getUrlParameter = function getUrlParameter(sParam) {
    var sPageURL = decodeURIComponent(window.location.search.substring(1)),
        sURLVariables = sPageURL.split('&'),
        sParameterName,
        i;

    for (i = 0; i < sURLVariables.length; i++) {
        sParameterName = sURLVariables[i].split('=');

        if (sParameterName[0] === sParam) {
            return sParameterName[1] === undefined ? true : sParameterName[1];
        }
    }
};
```
- 이 내용까지 추가하니 문제 해결
- 주석 내용을 보면 `get url query string`
- 이 부분은 `Django`에서 `request.get` 으로 해결가능하다 생각했음
	- 실제로 `url` 에 내가 설정한 `param`이 옴

- 위에 있는 문은 추후 공부 후 내용정리
