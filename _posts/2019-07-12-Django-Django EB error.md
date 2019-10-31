---
layout: post
title:  "[Django CoffeeCalorie Project EB Error]"
subtitle:   "django project elasticbeanstalk"
categories: django
tags: django elasticbeanstalk
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## ElasticBeanstalk 배포 에러
- `EB` 로 배포 하던 중 계속 에러에 대해 해결을 못함
	- 에러 내용은 `  FileNotFoundError: [Errno 2] No such file or directory: '/srv/projects/.static/admin'`

- `eb ssh` 에서 자세하게 내용을 살펴보면..
![EB-Error](/assets/img/EC2/EB-error-01.png)
 > 내가 gitignore에 .static 폴더를 추가한 사실을 망각함... 아주 간단한 문제..

 **해결**

 - 아주 간단한 문제.. gitignore에 .static 을 임시적으로 staging 상태에 올려둔 후 배포 후 다시 내리면 되는 문제..
 - 너무 부주의했음. 앞으로 좀 더 에러문제를 볼 때 생각을 하자..
