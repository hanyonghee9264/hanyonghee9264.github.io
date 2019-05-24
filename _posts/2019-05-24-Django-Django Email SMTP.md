---
layout: post
title:  "[Django SMTP Email]"
subtitle:   "Django SMTP Email "
categories: django
tags: django SMTP email
comments: true
---
> 본 포스팅은 개인공부 후 자료를 남기기 위한 목적임으로 내용 상에 오류가 있을 수 있습니다.

## Django SMTP

- SMTP [Simple Mail Transfer Protocol)의 약자
- 전자 메일 전송을 위한 표준 프로토콜
- 이메일을 송수신하는 서버를 `SMTP`서버라고 함

> Ex) id가 abc@gmail.com 인 전자메일을 xyz@gggg.com 으로 보낸다고 가정
>
> 보낸 사람은 SMTP 메일 서버 gmail, 받는 사람은 SMTP 메일 서버 gggg
>
> 전자 메일을 입력하고 xyz@gggg.com과 같은 받는 사람의 주소를 보내기를 클릭하면 전자메일이 gmail SMTP 서버에 도달하여 그쪽에 저장
>
> gmail SMTP 서버는 받는 사람의 주소를 ID는 xyz DNS는 gggg.com의 두 부분으로 액세스
>
> gmail SMTP 서버는 수신자에 대한 정보를 얻기 위해 DNS 서버에 접속
>
> DNS 서버는 우선 순위 정보를 사용하여 받는 사람의 도메인 이름에 매핑 된 IP 주소를 반환
>
> (이것들은 받는 사람의 SMTP 서버의 IP주소를 말함 위의 예시로는 gggg
>
> IP 주소를 받은 후 보낸 사람 SMTP 서버는 받는 사람 SMTP 서버를 찾을 때까지 IP 주소에 우선 순위를 부여
>
> 전자 메일이 저장되어 받는 사람의 SMTP 서버로 전자 메일이 전송
>
> 받는 사람 SMTP 서버는 받는사람의 ID (위의 예로 xyz) 에 액세스하여 사용자의 계정에 저장
