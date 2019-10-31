---
layout: post
title:  "[AWS Route53]"
subtitle:   "AWS route53 elasticbeanstalk"
categories: AWS
tags: AWS route53
comments: true
---
> 본 포스팅은 생활코딩을 바탕으로 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.


## AWS Route 53 (DNS)

### IP
- 인터넷에 연결된 컴퓨터와 컴퓨터를 식별하는 주소

	> 전화로 비유하면 전화번호

### Domain
- `IP`는 사람이 기억하기 어렵기 때문에 문자로 매핑

	> 도메인은 주소록에 기록된 친구 이름

### DNS
- `Domain Name Server`의 약자
- 도메인과 도메인에 해당하는 `IP`에 대한 정보를 가지고 있다가 도메인 주소에 대한 요청이 들어왔을 때 이에 해당하는 `IP`를 알려주는 서버

	> 친구의 이름에 대한 전화번호를 알려주는 주소록

### Route 53
- `Route 53`은 `AWS`에서 제공되는 `DNS 웹서비스`
	- 고가용성과 안정성
	- 확장성
	- 다른 `Amazon Web Services`와 연동
	- 간편함
	- 신속함
	- 비용 효율성
	- 보안
	- 유연성

----
**정리**

- 나같은 경우는 `Route 53`을 활용
	- 개인프로젝트 대부분 `ElasticBeanstalk`로 배포
		- `ELB`를 사용하기 때문에 ec2가 여러개 존재 할 수 있기 때문에 `load balancer`쪽으로 도메인을 설정

		> 구매한 도메인의 네임서버 변경 -> route53 record set 추가
