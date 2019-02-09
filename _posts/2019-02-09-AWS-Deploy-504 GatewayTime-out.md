---
layout: post
title:  "[AWS 504 Gateway Time-out]"
subtitle:   "EB environment"
categories: aws
tags: deploy
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

# AWS 504 Gateway Time-out 에러

### RDS 보안그룹 EB Environment 그룹 추가

**runserver나 docker를 열었을 때**

```eb
RDS
	Inbound: my computer

								EC2
# 우리컴퓨터(my computer)

```

> 위는 현재 로컬이나 docker에서만 돌릴 경우에는 문제가 없다
>
> 그치만 deploy 과정에서는 inbound에 EC2를 추가 해줘야한다
>
> 왜냐하면 외부에서 접근을 허용을 안하기 때문

**deploy**

```eb
RDS
	Inbound: my computer, EC2

								EC2
# 우리컴퓨터(my computer)

```

> 다음과 같이 EC2를 Inbound에 추가해줘야 접근이 가능해짐

![EB environment](/assets/img/deploy/eb-environment-01.png)

위 첨부이미지를 확인해보면

SecurityGroup for ElasticBeanstalk environment. 에 그룹 ID를 추가 => RDS Security Group
