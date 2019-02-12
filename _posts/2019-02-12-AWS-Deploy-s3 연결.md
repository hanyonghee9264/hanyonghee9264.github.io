---
layout: post
title:  "[CLI를 통한 S3 연결]"
subtitle:   "s3 create bucket"
categories: aws
tags: deploy
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## S3

### S3. client 생성(사용자 정의)

```boto3
import boto3
client = boto3.client('s3')
```

---

### 버킷생성

- `Bucket` 명은 고유해야한다
- `LocationConstraint`: 리전, `ap-northeast-2`: 서울

```bucket
client.create_bucket(
	Bucket='hanyonghee',
	CreateBucketConfiguration={
        'LocationConstraint': 'ap-northeast-2'
	}
)
```

---

### 파일 업로드

s3.meta.client.upload_file('<파일위치>', '<버킷명>', '<버킷에 올릴 파일명>')

```boto
import boto3
s3 = boto3.resource('s3')
s3.meta.client.upload_file(
	'.media/user/pby1.jpg', 'hanyonghee', 'bbo.jpg'
)
```
