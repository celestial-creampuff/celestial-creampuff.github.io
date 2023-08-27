---
title: "[AWS] 기초부터 시작하는 AWS 구축하기 - Bastion Host EC2를 통한 Private 서브넷에 있는 EC2 접근하기"
date: 2023-08-27 01:00:00 -0400
categories: AWS VPC
tags:
    - AWS
    - EC2
    - Computing
    - Server
    - Private Subnet
toc: true
toc_sticky: true
---
## 개요
우린 전에 EC2를 생성하는 방법을 배웠다.<br>
하지만 우리가 접근해야 하는 EC2가 프라이빗 서브넷에 배치되어 있으면 어떻게 접근해야 하는지 모를 것이다. 오늘은 그 방법에 대해 알아볼 것이다.

## 학습목표
이번 게시물의 학습목표는 아래와 같다.
- Bastion Host의 의의를 배운다.
- Bastion Host를 통해 Private Subnet에 있는 리소스에 접근하는 법을 익힌다.


## 1. Bastion Host란?
**Bastion Host(배스쳔 호스트)** 란 하나의 관문이라고 생각하면 된다. 즉 외부에 있는 인터넷과 내부에 배치된 서비스를 이어주는 다리 역할을 하는 Host라고 생각하면 된다.<br>
하지만 이 Bastion Host는 특별한 역할을 한다. 그 역할이 무엇일까?

### 1-1. Bastion Host의 기능
배스쳔 호스트는 보안을 위해 고안된 Host이다. 외부에서 혹시 악의적인 뜻을 품고 내부의 네트워크안에 있는 서비스를 공격하려고 온다면 이것을 막는 것이 Bastion Host이다.

그래서 외부에서는 결국 Bastion Host라는 관문을 통해서 허가된 사용자(AWS에서는 IP로 보면 된다.)만이 프라이빗 서브넷에 접근할 수 있는 것이다.

이제 이 Bastion Host를 통해서 Private에 배치된 EC2에 접근하는 방법을 배우자!

## 2. Bastion-Host Hands-on
일단 이 과정을 하기 전에 우리는 먼저 VPC를 구축하고 기본적인 구성을 완료하였다. 또한 Bastion Host 역할을 하는 EC2를 퍼블릭 서브넷에 생성까지 했다.

만약 해당 과정을 안거쳤다면 **기초부터 시작하는 AWS 구축하기**를 참고해서 천천히 하나씩 따라와보자🙂


### 2-1. Private Subnet에 EC2 생성
먼저 EC2 한대를 생성해주자!<br>
생성한 방법은 이전에 포스팅한 **[기초부터 시작하는 AWS 구축하기 - EC2 생성](https://celestial-creampuff.github.io/aws/vpc/EC2/)**을 참고하자.<br><br>
하지만 정보는 달라야 한다. 아래의 정보를 참고해주자!
<br>내가 만들었던 내용은 아래와 같다.
아래를 자신이 만들었던 VPC의 정보대로 맞추면 되고 내꺼는 그냥 참고만해주자.. ㅎ

|제목|내용|
|---|---|
|Name|WEB-Server-01|
|AMI|Amazon Linux 2023|
|인스턴스 유형|t2.micro|
|키 페어|WEB-Server.key|
|VPC|rmi-vpc|
|Subnet|rmi-web-subnet-01|
|퍼블릭 IP 자동 할당|비활성화|
|방화벽(보안그룹)|WEB-SG|

<br>
그리고 방화벽(보안그룹)의 정보는 아래와 같다.

![보안 그룹 생성](/assets/2023-08/Bastion-Host/스크린샷%202023-08-27%20오후%203.11.11.png)<br><br>

그리고 이렇게 WEB-Server-01의 인스턴스를 생성하였다.
![새로운 EC2 생성](/assets/2023-08/Bastion-Host/스크린샷%202023-08-27%20오후%202.36.03.png)


### 2-2. Bastion Host에 키 복사
우리는 Bastion Host 만들었을 때 동시에 만든 키 페어를 사용하지 않는다. 조금 전에 만들었던 정보에 보면 web 키 페어를 따로 만들었다는 것을 눈치챘을 것이다.

하지만 이 키 페어를 사용하려면 아래와 같은 방법을 따라야 한다. 방법은 사람마다 상이하지만 나는 Bastion-Host로 사용하는 EC2에 직접 키페어 파일 폴더를 만들었다. 그리고 거기에 키 내용을 복사하고 그것을 이용해서 Private EC2에 접근했다.

1. 우선 키는 아래와 같이 생성해줬다.<br>
그리고 다운로드 받은 파일 경로에서 직접 키페어를 열어주자.
![키페어 파일](/assets/2023-08/Bastion-Host/스크린샷%202023-08-27%20오후%202.30.54.png)<br><br>

2. 이때 Mac 유저들중에서 주의해야 할 점은 우클릭 후 다음으로 열기 -> 기타에서 열어줘야 내용을 볼 수 있다.
![키페어 파일2](/assets/2023-08/Bastion-Host/스크린샷%202023-08-27%20오후%202.40.21.png)<br><br>

3. 그리고 텍스트 편집기를 찾아 그것으로 열어주자!
![키페어 파일2](/assets/2023-08/Bastion-Host/스크린샷%202023-08-27%20오후%202.40.42.png)<br><br>

4. 그렇다면 아래와 같이 내용을 볼 수 있다. 그 해당내용 전체를 복사해주자!
![키페어 파일2](/assets/2023-08/Bastion-Host/스크린샷%202023-08-27%20오후%203.14.11.png)<br><br>

5. 이제 서버에 다시 접속해주자!
서버 접속은 전에 포스팅 했던 게시물을 참고하면 된다.<br>
(여기서 서버는 방금 Private Sunbet에 생성한 EC2가 아니라 Bastion Host로 만든 서버이니 걱정 안해도 된다!)
<br>

```py
$ cd Downloads

$ ssh -i bastion_key.pem ec2-user@x.xx.xx.xxx

----여기까지 서버 접속이 완료되었다면 아래를 따라와주자.----

## key-folder 라는 폴더를 생성
$ mkdir key-folder

## 폴더 확인을 위한 명령어
$ ls

### 결과값
key-folder

## key-folder로 이동
$ cd key-folder

## web-key 라는 폴더를 생성
$ mkdir web-key

## 폴더 확인을 위한 명령어
$ ls

### 결과값
web-key

## web-key라는 폴더에 접근

$ cd web-key

### WEB-Server-key.pem 이라는 파일을 생성
$ touch WEB-Server-key.pem

### WEB-Server-key.pem 파일 수정
$ vim WEB-Server-key.pem
```
이때 텍스트 편집기(Windows에서는 메모장)를 통해서 내용을 봤고 복사까지 했다. 그 복사했던 내용을 아래와 같이 붙여주고 저장하고 나온다!
![키 페어 파일 복사](/assets/2023-08/Bastion-Host/스크린샷%202023-08-27%20오후%202.41.23.png)

이렇게 키 페어 파일까지 Bastion 목적으로 쓰이는 EC2에 옮겼다. 마지막으로 이제 Private EC2에 접근하자!


### 2-3. Bastion Host를 통한 EC2 접근하기
먼저 EC2 콘솔에서 접근해야 할 EC2의 Private IP를 아래와 같이 복사한다.
![private EC2 접근](/assets/2023-08/Bastion-Host/스크린샷%202023-08-27%20오후%202.47.48.png)

이제 아래의 명령어를 따라와주자!
```py
## 키 권한 축소 
$ chmod 400 WEB-Server-key.pem

## Private Subnet에 배치된 EC2 접속
$ ssh -i WEB-Server-key.pem ec2-user@xx.x.xx.xxx

### 중간에 yes 입력

## 접속 성공!
Warning: Permanently added 'xx.x.xx.xxx' (ED25519) to the list of known hosts.
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'

$

```
<br><br>
이렇게 Bastion Host(EC2)를 통해서 Private Subnet에 있는 EC2에 접근을 성공적으로 할 수 있었다!<br>

내가 접근했을 때 명령어인데 참고하면 좋을 것 같다!
```py
    ~  cd Downloads

    ~/Downloads  ssh -i bastion_key.pem ec2-user@x.xxx.xx.xxx

The authenticity of host 'x.xxx.xx.xxx (x.xxx.xx.xxx)' can't be established.
ED25519 key fingerprint is SHA256:CtXJcoteB17zOHEWLRuoRexdGl9vpoeCl5ZQKEZYg9o.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:109: x.xxx.xx.xxx
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'x.xxx.xx.xxx' (ED25519) to the list of known hosts.

A newer release of "Amazon Linux" is available.
  Version 2023.1.20230825:
Run "/usr/bin/dnf check-release-update" for full release and version update info
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
Last login: Mon Aug 21 12:44:13 2023 from x.xxx.xx.xxx
[ec2-user@ip-10-0-10-99 ~]$ mkdir key-folder
[ec2-user@ip-10-0-10-99 ~]$ ls
key-folder
[ec2-user@ip-10-0-10-99 ~]$ cd key-folder
[ec2-user@ip-10-0-10-99 key-folder]$ mkdir web-key
[ec2-user@ip-10-0-10-99 key-folder]$ ls
web-key
[ec2-user@ip-10-0-10-99 key-folder]$ cd web-key
[ec2-user@ip-10-0-10-99 web-key]$ touch WEB-Server-key.pem
[ec2-user@ip-10-0-10-99 web-key]$ vim WEB-Server-key.pem
[ec2-user@ip-10-0-10-99 web-key]$ chmod 400 WEB-Server-key.pem
[ec2-user@ip-10-0-10-99 web-key]$ ssh -i WEB-Server-key.pem ec2-user@xx.x.xx.xxx
The authenticity of host 'xx.x.xx.xxx (xx.x.xx.xxx)' can't be established.
ED25519 key fingerprint is SHA256:X6y8JiZ4VcIMjSg0S87bERZd3M+PsUsr0FW2tx7EB/4.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'xx.x.xx.xxx' (ED25519) to the list of known hosts.
   ,     #_
   ~\_  ####_        Amazon Linux 2023
  ~~  \_#####\
  ~~     \###|
  ~~       \#/ ___   https://aws.amazon.com/linux/amazon-linux-2023
   ~~       V~' '->
    ~~~         /
      ~~._.   _/
         _/ _/
       _/m/'
[ec2-user@ip-xx-x-xx-xxx ~]$
```

## 3. 마치며
사실 요새는 보안이 매우 중요해지면서 Bastion-host를 통한 접근은 점점 멀어지고 있다고 한다(AWS에서는..?)<br>

요새는 SSM을 사용해서 Private Subnet에 있는 EC2에 접근하기도 하고 Client VPN을 이용하거나 AWS에서 제공하는 CloudShell을 이용한다고 한다.(물론 난 아직 이것들을 못써봤다 ㅜㅜ)

시간되면 이것도 공부하고 나서 포스팅을 하는걸로 하자!!🥹


<br><br><br>
끝!
<br><br><br>
## 참고 및 출처

[Bastion Host의 이해와 AWS에서의 구성 (Proxy)](https://err-bzz.oopy.io/f5616e26-79ca-4167-b2eb-140de69b9b54)

[Bastion Host 란](https://dodomp0114.tistory.com/7)

[Bastion Host란 무엇인가?](https://skstp35.tistory.com/363)

[EC2 Bastion Host를 통해 Private Subnet EC2에 접속](https://dev.classmethod.jp/articles/access-private-subnet-ec2-via-ec2-bastion-host/)