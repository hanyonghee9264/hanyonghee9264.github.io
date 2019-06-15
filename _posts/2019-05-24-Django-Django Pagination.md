---
layout: post
title:  "[Django Paginator]"
subtitle:   "Django paginator pagination "
categories: django
tags: django paginator
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## Django Pagination
- `Django Pagination Example` 정리
- [링크 클릭](https://docs.djangoproject.com/en/2.2/topics/pagination/){: target="_blank" }

```Pagination
from blog.models import Post
from django.core.paginator import Paginator

posts = Post.objects.all()

posts.count()
>>> 11

posts = Post.objects.order_by('-pk')
paginator = Paginator(posts, 5) # 페이지 당 5개의 list를 보여줌

paginator.count
>>> 11 # 총 11개
paginator.num_pages
>>> 3 # 3개의 페이지로 구성됨
paginator.page_range
>>> range(1, 4) # 1~3 페이지 맨끝은 미만 이기 때문에

page1 = paginator.page(1)

page1
>>> <Page 1 of 3>

page1.objects_list
>>> <QuerySet [<Post: redirect shortcut을 사용>, <Post: URL 바꾸고 새 글 작성>, <Post: URL 바꾸고 새 글 작성>, <Post: 과연 목록으로 이동하는가>, <Post: 웹프스>]>

for item in page1:
	print(item)
>>> redirect shortcut을 사용
URL 바꾸고 새 글 작성
URL 바꾸고 새 글 작성
과연 목록으로 이동하는가
ㅎㅎㅎㅎㅎ

# 여기서 위에 있는 objects_list 인 QuerySet을 순회하지 않고 page1이라는 객체를 순회했는데 가능했다!
# 이것은 Python magic method 안에 __iter__ 가 정의되어 있어 가능함

page1.has_previous()
>>> False # page1에 이전은 없음

page1.has_next()
>>> True # page1 다음은 존재하기 때문..

page1.next_page_number()
>>> 2 # 총 3page

page1.previous_page_number()
>>> EmptyPage: That page number is less than 1 # page1의 이전 페이지는 없기 때문

page1.start_index()
>>> 1  # 페이지 첫 번째 항목에 대해 시작하는 index

page1.end_index()
>>> 5 # 총 11개의 게시물 3페이지 페이지당 5개의 게시물 이기때문에 끝 index 는 5
```
