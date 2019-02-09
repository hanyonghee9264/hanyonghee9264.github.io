---
layout: post
title:  "[Docker hub 및 deploy script 작성]"
subtitle:   "docker-hub deploy script"
categories: aws
tags: deploy
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

# Docker hub 연결 및 deploy script 작성

### docker hub 연결

`docker tag ilovefish:base hanyonghee9264/ilovefish:base`

> docker hub 에는 내 아이디로 저장소가 되어있기 때문에 내 이미지를 docker id 명으로 변경

### eb 에러 확인

```eb
cat /var/log/eb-activity.log
```

### eb 배포 시 주의사항 (deploy script 작성)

deploy를 시도 하면 다음과 같은 에러가 뜬다
`FileNotFoundError: [Errno 2] No such file or directory: '/srv/project/.secrets/base.json'`
> gitignore에 우리가 secret 폴더는 제외 했기 때문

deploy는 우리가 커밋을 남긴거만 하기 때문에 secrets를 일시적으로 --staged (스테이징 영역)에 추가해줘야 한다

```eb
>>> git add -f .secrets

# 스테이징 상태 확인 후

>> eb deploy --profile <eb키 값> --staged

# 배포가 정상적으로 이뤄진것을 확인

# 이제 secrets 폴더를 스테이징 영역에서 취소해야 한다

>>> git reset HEAD .secrets
```

----

위와 같이 했을 경우 secrets 가 노출될 수 있기 때문에 `deploy script`를 작성하자

```deploy.sh
#!/usr/bin/env bash
git add -f .secrets/
eb deploy --profile <eb키 값> --staged
git reset HEAD .secrets/

# ./deploy.sh 를 실행하면 안됨 (실행권한 없음)
# 따라서 권한을 부여해줌

chmod 755 deploy.sh

```

> sh로 명령어를 실행했다는 말은
>
> 예를 들어 python abc.py 를 실행한다고 가정
>
> ./ 는 ./deploy.sh 자체를 실행한다는 뜻
