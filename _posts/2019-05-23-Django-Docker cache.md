---
layout: post
title:  "[Docker build 시 cache 문제]"
subtitle:   "Django docker cache "
categories: django
tags: docker cache
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## Docker build cache 문제

**문제점**

- `Django` 프로젝트 중 내용 변경사항이 있었음
- 변경사항을 추가 한 뒤 `Docker build` 를 했음
	- 변경사항에 대한 내용이 이미지로 안만들어짐
	- (같은 `image` 값을 확인)

**해결**

- `Docker Docs`를 참고함
- `docker build` 명령은 `Dockerfile`에 실제로 존재하는 장소에 관계 없이 현재 디렉토리에 있는 파일과 모든 재귀 컨텐트가 `Docker` 데몬으로 전송
- `cache` 가 남아 있을 수 도 있기 때문에
- `--no-cache`라는 명령어를 통해 마지막 `build`한 이미지 캐시에 의존하지 않고 새롭게 구축 가능

> 실제로 docker build --no-cache -t ecs-deploy:base -f Dockerfile.base . 로 해결
