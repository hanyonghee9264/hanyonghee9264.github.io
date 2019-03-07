---
layout: post
title:  "[Django Import os 모듈]"
subtitle:   "os모듈"
categories: django
tags: 
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## os path 모듈

```os
import os

# 파일의 이름을 넣었을 때 그 파일의 절대경로를 불러옴
# absolute path
current_path = os.path.abspath(__file__)

# 한 단계 위로 올라감
# directory name
parent_dir = os.path.dirname(current_path)

# 경로를 병합하여 새 경로 생성 [디렉터리와 파일명을 이어줌]
blog_dir = os.path.join(parent_dir, 'blog)
blog_urls = os.path.join(blog_dir, 'urls.py)
```

**정리**

- `os.path.abspath(__file__)`

	> 코드가 실행중인 파일의 경로를 나타냄

- `os.path.dirname(<경로>)`

	> 특정 경로의 상위폴더를 나타냄

- `os.path.dirname(<경로>, <폴더/파일명>)`

	> 특정 경로에서 하위폴더 또는 하위 파일을 나타냄
