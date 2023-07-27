---
title: "[AWS] Client-VPN Hands On"
date: 2023-07-27 00:00:00 -0400
categories: AWS VPC Client-VPN
tags:
    - AWS
    - VPC
    - Client-VPN
toc: true
toc_sticky: true
---

## 개요

### Client VPN 이란?

AWS Client VPN은 AWS 리소스 및 온프레미스 네트워크의 리소스에 안전하게 액세스할 수 있는 클라이언트 기반 관리형 VPN 서비스입니다. 클라이언트 VPN을 사용하면 OpenVPN 기반의 VPN 클라이언트를 사용하여 어디서나 리소스에 액세스가 가능합니다.

## Hands-On

해당 핸즈온은 아래와 같은 특징을 가지고 있습니다!

### 구성도

(추가하기)

### 실습환경

- Chrome 브라우저 사용
- 로컬 = Windows 환경
- ap-northeast-2(서울 리전)

### VPC 환경 설정



1. VPC 콘솔로 이동합니다.
2. VPC 생성을 클릭 후 아래와 같이 VPC 하나를 만들어줍니다.
![VPC 생성](/assets/2023-07/Client_VPN/2023-07-27-1.png)
</br>
[VPC 생성](/assets/2023-07/Client_VPN/2023-07-27-1.png)

3. VPC를 만들었다면 서브넷 4개를 만들어줍니다.
아래와 같은과정을 거쳐 서브넷을 만들어주세요.
새 서브넷 추가를 클릭하여 다른 서브넷을 만들 수 있으며, 서울 지역은 A와C 부분의 가용영역을 사용합니다.
</br></br>
해당 과정을 잘 거쳤다면 두 번째 사진과 같이 서브넷 4개가 만들어진 것을 볼 수 있습니다.
![서브넷 구성](/assets/2023-07/Client_VPN/2023-07-27-2.png)
</br>
[서브넷 구성](/assets/2023-07/Client_VPN/2023-07-27-2.png)
</br>
![서브넷 구성2](/assets/2023-07/Client_VPN/2023-07-27-3.png)
</br>
[서브넷 구성2](/assets/2023-07/Client_VPN/2023-07-27-3.png)
</br>

### 인스턴스 생성
1. EC2 인스턴스 한 대를 만들기 위하여, EC2 콘솔로 접속합니다.
해당 사진은 EC2 인스턴스를 만들기 위해 해당 콘솔로 들어가는 과정입니다!
![인스턴스 구성1](/assets/2023-07/Client_VPN/2023-07-27-4.png)</br>
[인스턴스 구성1](/assets/2023-07/Client_VPN/2023-07-27-4.png)</br>
![인스턴스 구성2](/assets/2023-07/Client_VPN/2023-07-27-5.png)</br>
[인스턴스 구성2](/assets/2023-07/Client_VPN/2023-07-27-5.png)</br></br></br>
2. 아래와 같이 인스턴스를 1대를 생성해줍니다.
여기서 웬만한 값은 기본값으로 두며, 테스트 용도이기 때문에 키페어는 없는 상태로 하였습니다.</br></br>
하지만 네트워크 부분과 보안그룹 부분만 유의해서 설정해주세요.
![인스턴스 구성3](/assets/2023-07/Client_VPN/2023-07-27-6.png)
</br>
[인스턴스 구성3](/assets/2023-07/Client_VPN/2023-07-27-6.png)
</br>
![인스턴스 구성4](/assets/2023-07/Client_VPN/2023-07-27-7.png)
</br>
[인스턴스 구성4](/assets/2023-07/Client_VPN/2023-07-27-7.png)
</br>
![인스턴스 구성5](/assets/2023-07/Client_VPN/2023-07-27-8.png)
</br>
[인스턴스 구성5](/assets/2023-07/Client_VPN/2023-07-27-8.png)
</br>
![인스턴스 구성6](/assets/2023-07/Client_VPN/2023-07-27-9.png)
</br>
[인스턴스 구성6](/assets/2023-07/Client_VPN/2023-07-27-9.png)
</br>
![인스턴스 구성7](/assets/2023-07/Client_VPN/2023-07-27-10.png)
</br>
[인스턴스 구성7](/assets/2023-07/Client_VPN/2023-07-27-10.png)
</br>
![인스턴스 구성8](/assets/2023-07/Client_VPN/2023-07-27-11.png)
</br>
[인스턴스 구성8](/assets/2023-07/Client_VPN/2023-07-27-11.png)
</br></br>
이렇게 인스턴스 한 대가 정상적으로 생성된 것을 볼 수 있습니다.


### CloudWatch Log 그룹 설정

 해당 단계는 Client VPN 연결 시 로그를 확인하기 위하여 설정하는 단계입니다.
1. Log 그룹을 만들기 위하여 콘솔 상단에 돋보기 버튼 옆에 CloudWatch를 검색하고 서비스에서 CloudWatch를 클릭합니다.
![Log 그룹 설정 1](/assets/2023-07/Client_VPN/2023-07-27-12.png)</br>
[Log 그룹 설정 1](/assets/2023-07/Client_VPN/2023-07-27-12.png)</br></br>
2. 좌측 사이드 바에 로그 부문에서 로그 그룹을 클릭하고 로그 그룹 생성을 클릭합니다.
![Log 그룹 설정 2](/assets/2023-07/Client_VPN/2023-07-27-13.png)</br>
[Log 그룹 설정 2](/assets/2023-07/Client_VPN/2023-07-27-13.png)</br></br>
3. 로그 그룹 이름은 /aws/clientvpn 으로 설정하고 생성을 클릭합니다.
![Log 그룹 설정 3](/assets/2023-07/Client_VPN/2023-07-27-14.png)</br>
[Log 그룹 설정 3](/assets/2023-07/Client_VPN/2023-07-27-14.png)</br></br>
4. 아래와 같이 log 그룹이 만들어진 것을 볼 수 있습니다. 그리고 해당 로그 그룹을 클릭해서 들어가주세요.
![Log 그룹 설정 4](/assets/2023-07/Client_VPN/2023-07-27-15.png)</br>
[Log 그룹 설정 4](/assets/2023-07/Client_VPN/2023-07-27-15.png)</br></br>
5. 해당 로그 그룹에 들어가셨다면 아래 사진과 같이 로그 스트림 생성을 클릭합니다.
![Log 그룹 설정 5](/assets/2023-07/Client_VPN/2023-07-27-16.png)</br>
[Log 그룹 설정 5](/assets/2023-07/Client_VPN/2023-07-27-16.png)</br></br>
6. 로그 스트림을 아래와 같이 생성합니다.
그렇다면 두 번째 사진과 같이 로그 스트림이 생성된 것을 볼 수 있습니다.</br>
![Log 그룹 설정 5](/assets/2023-07/Client_VPN/2023-07-27-17.png)</br>
[Log 그룹 설정 5](/assets/2023-07/Client_VPN/2023-07-27-17.png)</br></br>
![Log 그룹 설정 6](/assets/2023-07/Client_VPN/2023-07-27-18.png)</br>
[Log 그룹 설정 6](/assets/2023-07/Client_VPN/2023-07-27-18.png)</br></br>


### 인증서 만들기

아래 해당 링크를 클릭하여 핸즈온 -> 공통 설정 -> 인증서 만들기 부분을 참고해서 Windows라면 OpenVPN 커뮤니티에서 OpenVPN을 다운로드 하고 EasyRSA도 다운로드 받으셔야 합니다!

#### Windows
자신의 컴퓨터에서 cmd(terminal)을 관리자 권한 실행으로 실행합니다.(Windows 키를 눌러 검색 가능)
</br>
이후에 아래의 **명령어를 차례대로 입력**해주세요!

```py
## C:로 이동 (터미널 켰을 때 기본 경로가 유저일 시 아래의 명령어 입력하면서 C:로 이동
C:\Users\kimhj>cd ..

C:\Users>cd ..

## Easy RSA가 있는 폴더로 이동(다운로드 받은 위치 파악 및 버전에 따라 명령어가 다를 수 있음)
C:\>cd "Program Files\OpenVPN\EasyRSA-3.1.0" (해당 명령어가 이렇게 입력된 이유는 \ 누르고 prog 까지만 하고 키보드에 tab 버튼을 누르면 다음과 같이 되는 현상 발견)

## Easy RSA 3.1.0  셸 시작
C:\Program Files\OpenVPN\EasyRSA-3.1.0>EasyRSA-Start

## 새 PKI 환경을 초기화
./easyrsa init-pki

## 새 인증 기관(CA)을 빌드
./easyrsa build-ca nopass

## 도중에 Common Name 물을 시, 기본값 그대로(Easy-RSA CA.)
Common Name (eg: your user, host, or server name) [Easy-RSA CA]:

## 서버 인증서와 키 생성
./easyrsa build-server-full server nopass

## 클라이언트 인증서와 키 생성
./easyrsa build-client-full client1.domain.tld nopass

## EasyRSA 3 셸을 종료
exit

## 서버 인증서와 키와 클라이언트 인증서와 키를 사용자 지정 폴더에 복사
C:\Program Files\OpenVPN\EasyRSA-3.1.0> mkdir C:\custom_folder
C:\Program Files\OpenVPN\EasyRSA-3.1.0> copy pki\ca.crt C:\custom_folder
C:\Program Files\OpenVPN\EasyRSA-3.1.0> copy pki\issued\server.crt C:\custom_folder
C:\Program Files\OpenVPN\EasyRSA-3.1.0> copy pki\private\server.key C:\custom_folder
C:\Program Files\OpenVPN\EasyRSA-3.1.0> copy pki\issued\client1.domain.tld.crt C:\custom_folder
C:\Program Files\OpenVPN\EasyRSA-3.1.0> copy pki\private\client1.domain.tld.key C:\custom_folder
```
#### 명령어 입력 시 캡쳐 화면
![명령어 입력 시 캡쳐 화면 1](/assets/2023-07/Client_VPN/2023-07-27-19.png)
</br>

[명령어 입력 시 캡쳐 화면 1](/assets/2023-07/Client_VPN/2023-07-27-19.png)
</br>

![명령어 입력 시 캡쳐 화면 2](/assets/2023-07/Client_VPN/2023-07-27-20.png)
</br>

[명령어 입력 시 캡쳐 화면 2](/assets/2023-07/Client_VPN/2023-07-27-20.png)
</br>

![명령어 입력 시 캡쳐 화면 3](/assets/2023-07/Client_VPN/2023-07-27-21.png)
</br>

[명령어 입력 시 캡쳐 화면 3](/assets/2023-07/Client_VPN/2023-07-27-21.png)
</br>

![명령어 입력 시 캡쳐 화면 4](/assets/2023-07/Client_VPN/2023-07-27-22.png)
</br>

[명령어 입력 시 캡쳐 화면 4](/assets/2023-07/Client_VPN/2023-07-27-22.png)
</br>

![명령어 입력 시 캡쳐 화면 5](/assets/2023-07/Client_VPN/2023-07-27-23.png)
</br>

[명령어 입력 시 캡쳐 화면 5](/assets/2023-07/Client_VPN/2023-07-27-23.png)
</br>

![명령어 입력 시 캡쳐 화면 6](/assets/2023-07/Client_VPN/2023-07-27-24.png)
</br>

[명령어 입력 시 캡쳐 화면 6](/assets/2023-07/Client_VPN/2023-07-27-24.png)
</br>


#### 명령어 입력 화면 (결과와 같이 출력!)

```py
Microsoft Windows [Version 10.0.22000.1696]
(c) Microsoft Corporation. All rights reserved.

C:\Users\kimhj>cd ..

C:\Users>cd ..

C:\>cd "Program Files\OpenVPN\EasyRSA-3.1.0"

C:\Program Files\OpenVPN\EasyRSA-3.1.0>EasyRSA-Start

Welcome to the EasyRSA 3 Shell for Windows.
Easy-RSA 3 is available under a GNU GPLv2 license.

Invoke './easyrsa' to call the program. Without commands, help is displayed.

EasyRSA Shell
# ./easyrsa init-pki

WARNING!!!

You are about to remove the EASYRSA_PKI at:
* C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki

and initialize a fresh PKI here.

Type the word 'yes' to continue, or any other input to abort.
  Confirm removal: yes

* Notice:

  init-pki complete; you may now create a CA or requests.

  Your newly created PKI dir is:
  * C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki

* Notice:
  IMPORTANT: Easy-RSA 'vars' file has now been moved to your PKI above.


EasyRSA Shell
# ./easyrsa build-ca nopass
* Notice:
Using Easy-RSA configuration from: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/vars

* Notice:
Using SSL: openssl OpenSSL 3.0.3 3 May 2022 (Library: OpenSSL 3.0.3 3 May 2022)

Using configuration from C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/safessl-easyrsa.cnf.init-tmp
...+..+...+...............+....+......+.....+.........+.......+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.+..........+.....+.+.....+...+.......+...+...+............+........+.......+..+....+.....+....+...+.....+...+.....................+......+.........+.+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*..+.....+....+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
...+..+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*...+...+..+..........+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*............+..+.+...+.........+..+...+.+..+.......+...+...+.....+.+...............+......+.....+....+...+........+.............+.................+.......+........+....+...+...+...+..............+......+.+...........+...+....+........+.......+..+.............+..+...+............+..................+......+....+..+...+......+.+.........+.........+...+..+................+..+...+.......+...+..+.........+....+.........+...+.....+...+....+......+......+.....+...+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (eg: your user, host, or server name) [Easy-RSA CA]:

* Notice:

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at:
C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/ca.crt


EasyRSA Shell
# ./easyrsa build-server-full server nopass
* Notice:
Using Easy-RSA configuration from: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/vars

* Notice:
Using SSL: openssl OpenSSL 3.0.3 3 May 2022 (Library: OpenSSL 3.0.3 3 May 2022)

...+..+.........+...+.+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.+...........+....+.....+.+..+...+....+..+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*................+........+.......+...+...+............+...+.................+...+.......+......+..+....+.........+.....+......+...+.....................+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
....+.+......+..............+.......+...+.....+....+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*..+.........+.....+...+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*........+....+........+...+.+.....+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-----
* Notice:

Keypair and certificate request completed. Your files are:
req: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/reqs/server.req
key: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/private/server.key

Using configuration from C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/safessl-easyrsa.cnf.init-tmp
C8600000:error:0700006C:configuration file routines:NCONF_get_string:no value:crypto/conf/conf_lib.c:315:group=<NULL> name=unique_subject
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'server'
Certificate is to be certified until Jul  8 02:56:45 2025 GMT (825 days)

Write out database with 1 new entries
Data Base Updated

* Notice:
Certificate created at: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/issued/server.crt


EasyRSA Shell
# ./easyrsa build-client-full client.domain.tld nopass
* Notice:
Using Easy-RSA configuration from: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/vars

* Notice:
Using SSL: openssl OpenSSL 3.0.3 3 May 2022 (Library: OpenSSL 3.0.3 3 May 2022)

....+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*...+...+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*..+.....+.......+..+...+.+...........+....+.....+..........+......+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
..+...+..+.............+..+....+......+.................+.............+..+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*....+.+.....+...+....+...+..+.+......+........+.......+........+...+.........+.+...+.....+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*...+................+......+....................+.+...+...........+...+.+..............+.+..+...+.+...+.....+.......+..............+.........+......+............+.........+....+..+.......+..+.+...+..+.........+.........+.+......+.....+...........................+.......+........+...+....+.....+.+............+..+...+.......+........+.+..............+......+..........+......+.........+.....+...............+.+.....+............................+..+...+.+.....+............+.......+..+....+.....+................+...........+.............+.....+....+..........................+.......+........+....+...+..+......+.+.....+.+...+............+..+...+....+.........+..+.......+..+....+..+....+...+...............+........+....+.....+.+..+.+.....+......+................+..+.....................+....+..+.+..+...+....+.....+......+....+......+.....+....+.........+.....+.........+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
-----
* Notice:

Keypair and certificate request completed. Your files are:
req: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/reqs/client.domain.tld.req
key: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/private/client.domain.tld.key

Using configuration from C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/safessl-easyrsa.cnf.init-tmp
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'client.domain.tld'
Certificate is to be certified until Jul  8 02:57:21 2025 GMT (825 days)

Write out database with 1 new entries
Data Base Updated

* Notice:
Certificate created at: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/issued/client.domain.tld.crt


EasyRSA Shell
# exit

C:\Program Files\OpenVPN\EasyRSA-3.1.0>mkdir C:\custom_folder

C:\Program Files\OpenVPN\EasyRSA-3.1.0>copy pki\ca.crt C:\custom_folder
        1 file(s) copied.

C:\Program Files\OpenVPN\EasyRSA-3.1.0>copy pki\issued\server.crt C:\custom_folder
        1 file(s) copied.

C:\Program Files\OpenVPN\EasyRSA-3.1.0>copy pki\private\server.key C:\custom_folder
        1 file(s) copied.

C:\Program Files\OpenVPN\EasyRSA-3.1.0>copy pki\issued\client.domain.tld.crt C:\custom_folder
        1 file(s) copied.

C:\Program Files\OpenVPN\EasyRSA-3.1.0>copy pki\private\client.domain.tld.key C:\custom_folder
        1 file(s) copied.

C:\Program Files\OpenVPN\EasyRSA-3.1.0>
```
</br>

1. 다시 AWS 콘솔로 돌아와서, ACM 콘솔에 접속합니다. 그리고 아래와 같이 인증서 가져오기를 클릭합니다.
</br>
그러면 인증서 본문, 프라이빗 키, 체인을 입력하는 란이 있는 것을 알 수 있습니다.
![인증서 가져오기1](/assets/2023-07/Client_VPN/2023-07-27-25.png)
</br>
[인증서 가져오기1](/assets/2023-07/Client_VPN/2023-07-27-25.png)
</br></br>

2. 해당 부분에 포함되는 내용은 내 PC -> 로컬 디스크  (C:) 폴더 안에 있습니다.
왜냐하면  아까 인증서를 만들고 복사할 때 c 폴더 안에 Custom_folder라는 폴더를 만들었고, 그 안에 키들을 복사하였기 때문입니다.
</br></br>그리고 두 번째 사진의 해당 파일들의 속성은 아래 표와 같습니다.  </br>

|파일|&#124;내용&#124;|
|---|---|
|서버.crt|서버 인증서|
|서버.key|서버 인증서 키|
|클라이언트1.도메인.tld.crt|클라이언트 인증서|
|클라이언트1.도메인.tld.key|클라이언트 인증서 키|
|ca.crt|CA 인증서|

![인증서 가져오기2](/assets/2023-07/Client_VPN/2023-07-27-26.png)
</br>
[인증서 가져오기2](/assets/2023-07/Client_VPN/2023-07-27-26.png)
</br>
![인증서 가져오기3](/assets/2023-07/Client_VPN/2023-07-27-27.png)
</br>
[인증서 가져오기3](/assets/2023-07/Client_VPN/2023-07-27-27.png)
</br></br>

3. 다시 AWS 콘솔로 돌아와서 ACM 인증서를 가져올 단계입니다.
콘솔과 파일이 매핑되는 부분은 아래와 같아요.</br>
(열고자 하는 해당 파일은 아래 사진처럼 먼저 한번 클릭 -> 선택되었으면 우측 클릭 -> 연결 프로그램 -> 메모장으로 열 수 있어요.)</br>
(또한 유형에서 보안 인증서라는 부분은 crt라고 보시면 됩니다.)
![인증서 가져오기4](/assets/2023-07/Client_VPN/2023-07-27-28.png)</br>
[인증서 가져오기4](/assets/2023-07/Client_VPN/2023-07-27-28.png)</br>  

|콘솔|파일|설정값|
|---|---|---|
|인증서 본문|server.crt|-----BEGIN CERTIFICATE----- 에서 -----END CERTIFICATE----- 까지 복사하여 붙여넣기.|
|인증서 프라이빗 키|server.key|-----BEGIN CERTIFICATE----- 에서 -----END CERTIFICATE----- 까지 복사하여 붙여넣기.|
|인증서 체인|ca.crt|-----BEGIN CERTIFICATE----- 에서 -----END CERTIFICATE----- 까지 복사하여 붙여넣기.|
</br>

4. 서버 인증서를 생성하기 위하여 아래와 같은 단계를 거칩니다.
해당 파일들을 붙여넣기 하고,  다음을  클릭합니다.
</br>
이후 검토 및 가져오기 단계에서 가져오기를 클릭합니다.
그러면 세 번째 사진과 같이 인증서를 성공적으로 가져온 것을 볼 수 있습니다.
![인증서 가져오기5](/assets/2023-07/Client_VPN/2023-07-27-29.png)
</br>
[인증서 가져오기5](/assets/2023-07/Client_VPN/2023-07-27-29.png)
</br>
![인증서 가져오기6](/assets/2023-07/Client_VPN/2023-07-27-30.png)
</br>
[인증서 가져오기6](/assets/2023-07/Client_VPN/2023-07-27-30.png)
</br>
![인증서 가져오기7](/assets/2023-07/Client_VPN/2023-07-27-31.png)
</br>
[인증서 가져오기7](/assets/2023-07/Client_VPN/2023-07-27-31.png)
</br>

5. 만약 Client 인증서도 발행해야 한다면, 전 단계에서 Server를 만들었던 것과 같은 방식으로 만들어야 합니다. 단, server 인증서를 만들었을 때와는 달리, 아래의 목록을 참고해서 만들어야 합니다.</br>

|콘솔|파일|설정값|
|---|---|---|
|인증서 본문|client.domain.tld.crt|-----BEGIN CERTIFICATE----- 에서 -----END CERTIFICATE----- 까지 복사하여 붙여넣기.|
|인증서 프라이빗 키|client.domain.tld.key|-----BEGIN CERTIFICATE----- 에서 -----END CERTIFICATE----- 까지 복사하여 붙여넣기.|
|인증서 체인|ca.crt|-----BEGIN CERTIFICATE----- 에서 -----END CERTIFICATE----- 까지 복사하여 붙여넣기.|
</br>

![인증서 가져오기8](/assets/2023-07/Client_VPN/2023-07-27-32.png)
</br>
[인증서 가져오기8](/assets/2023-07/Client_VPN/2023-07-27-32.png)
</br>
![인증서 가져오기9](/assets/2023-07/Client_VPN/2023-07-27-33.png)
</br>
[인증서 가져오기9](/assets/2023-07/Client_VPN/2023-07-27-33.png)
</br>
</br>

6. 전 과정을 거쳤을 시, 아래와 같이 server 인증서와, client 인증서 모두 생성된 것을 인증서 나열 부문에서 볼 수 있습니다. (Client 인증서는 인증서를 생성 시 Name 지정에 따라 파일 이름이 달라질 수 있습니다. 이 부분 주의해주세요!)
</br>

![인증서 가져오기10](/assets/2023-07/Client_VPN/2023-07-27-34.png)
</br>
[인증서 가져오기10](/assets/2023-07/Client_VPN/2023-07-27-34.png)
</br>
</br>

### Client VPN 엔드포인트 생성 
1. VPC 콘솔에서 좌측 사이드바에 가상 사설 네트워크(VPN)부문에서 Client VPN 엔드포인트 부분을 클릭하고 우측 상단에 클라이언트 VPN 엔드포인트 생성 버튼을 클릭합니다.
</br>
![엔드포인트 생성1](/assets/2023-07/Client_VPN/2023-07-27-35.png)
</br>
[엔드포인트 생성1](/assets/2023-07/Client_VPN/2023-07-27-35.png)
</br>

2. 클라이언트 VPN 엔드포인트를 생성할 차례입니다.</br>
이름 : rmi-client-vpn </br>
설명 : rmi-client-vpn </br>
클라이언트 IPv4 CIDR : 100.52.16.0/22 </br></br>
인증 정보 중 서버 인증서는 ACM 단계에서 만들어줬던 server를 선택하고, 인증 옵션은 상호 인증 사용을 클릭합니다. 또한 클라이언트 인증서 ARN은 마찬가지로 ACM 단계에서 만들어줬던 client.domain.tld를 클릭합니다. </br></br>
연결 로깅의 경우 초반에 CloudWatch Log 그룹과 스트림을 만들었을 때 해당되는 것을 선택해주세요. </br> </br>
기타 피라미터 - 선택 사항에서는 분할 터널 활성화를 반드시 ON 해주시고 VPC ID는 Client VPN이 위치해야 할 VPC를 선택해줍니다. </br>
그리고 나머지는 기본값으로 두고, 하단에 클라이언트 VPN 엔드포인트 생성을 클릭합니다.
![엔드포인트 생성2](/assets/2023-07/Client_VPN/2023-07-27-36.png) 
[엔드포인트 생성2](/assets/2023-07/Client_VPN/2023-07-27-36.png)
![엔드포인트 생성3](/assets/2023-07/Client_VPN/2023-07-27-37.png)
[엔드포인트 생성3](/assets/2023-07/Client_VPN/2023-07-27-37.png)
![엔드포인트 생성4](/assets/2023-07/Client_VPN/2023-07-27-38.png)
[엔드포인트 생성4](/assets/2023-07/Client_VPN/2023-07-27-38.png)

3. 다음은 대상 네트워크 연결을 클릭해서 연결해야 할 서브넷을 선택해야 할 차례입니다. </br></br>
만들었던 클라이언트 VPN 엔드포인트를 체크하고 대상 네트워크 연결을 클릭해서 서브넷을 아래와 같이 연결해줍니다. </br></br>
VPC의 경우 Client VPN이 위치해야 할 VPC를 선택하고, 서브넷은 저의 경우, 만들었던 4개의 서브넷 중 아래와 같이 순서대로 (rmi_subnet_03, rmi_subnet_04)에 연결하였습니다. </br></br>
해당 과정을 거쳤으면, 5번째 사진과 같이 상태가 연결 중이라는 것을 볼 수 있습니다.
![엔드포인트 생성5](/assets/2023-07/Client_VPN/2023-07-27-39.png)
[엔드포인트 생성5](/assets/2023-07/Client_VPN/2023-07-27-39.png)
![엔드포인트 생성6](/assets/2023-07/Client_VPN/2023-07-27-40.png)
[엔드포인트 생성6](/assets/2023-07/Client_VPN/2023-07-27-40.png)
![엔드포인트 생성7](/assets/2023-07/Client_VPN/2023-07-27-41.png)
[엔드포인트 생성7](/assets/2023-07/Client_VPN/2023-07-27-41.png)
![엔드포인트 생성8](/assets/2023-07/Client_VPN/2023-07-27-42.png)
[엔드포인트 생성8](/assets/2023-07/Client_VPN/2023-07-27-42.png)
![엔드포인트 생성9](/assets/2023-07/Client_VPN/2023-07-27-43.png)
[엔드포인트 생성9](/assets/2023-07/Client_VPN/2023-07-27-43.png) </br>
4. 다음은 권한 부여 규칙을 추가해야 합니다. </br></br>
해당 단계는 네트워크에 대한 액세스를 클라이언트에 부여하는 인증 규칙을 추가하는 단계입니다.
현재 과정은 모든 사용자가 VPC 내의 네트워크 (10.0.0.0/16)에 대한 액세스를 허용하도록 합니다. </br></br>
먼저 해당 클라이언트 VPN 엔드포인트에서 권한 부여 규칙을 클릭합니다. </br></br>
그리고 액세스를 활성화할 대상 네트워크는 본인이 Client VPN 전용으로 만든 VPC의 IP 주소를 뜻합니다. 
그래서 저는 VPC를 10.0.0.0/16으로 만들었으므로, 10.0.0.0/16으로 하였습니다. </br></br>
또한 모든 사용자가 들어올 수 있도록 하기 위해 
모든 사용자에게 액세스 허용을 체크해주고 권한 부여 규칙 추가 버튼을 클릭합니다. </br></br>
이 과정을 모두 거쳤으면 3번째 사진과 같이 인증 부여 규칙에서 상태가 Authorizing으로 표시된 것을 볼 수 있습니다. 
![엔드포인트 생성10](/assets/2023-07/Client_VPN/2023-07-27-44.png)
[엔드포인트 생성10](/assets/2023-07/Client_VPN/2023-07-27-44.png)
![엔드포인트 생성11](/assets/2023-07/Client_VPN/2023-07-27-45.png)
[엔드포인트 생성11](/assets/2023-07/Client_VPN/2023-07-27-45.png)
![엔드포인트 생성12](/assets/2023-07/Client_VPN/2023-07-27-46.png)
[엔드포인트 생성12](/assets/2023-07/Client_VPN/2023-07-27-46.png)

5. 모든 과정을 거쳤으면 잠시 기다려야 합니다.
기다리면서 자신이 설정했던 사항이 일치하는지 세부 정보에서 상태와 정보들을 확인합니다. </br></br>
그리고 잠시 15~20분 정도 기다리면 두 번째 사진과 같이 상태가 Pending-associate => Available이 된 것을 볼 수 있습니다. </br></br>
이렇게 클라이언트 VPN 엔드포인트를 사용할 수 있게 됩니다.
![엔드포인트 생성13](/assets/2023-07/Client_VPN/2023-07-27-47.png)
[엔드포인트 생성13](/assets/2023-07/Client_VPN/2023-07-27-47.png)
![엔드포인트 생성14](/assets/2023-07/Client_VPN/2023-07-27-48.png)
[엔드포인트 생성14](/assets/2023-07/Client_VPN/2023-07-27-48.png)

6. 다음으로 클라이언트 구성을 다운로드 해야합니다.
이 과정은 VPN 연결 시 필요한 클라이언트 구성 파일입니다. </br></br>
우선 해당 클라이언트 VPN 엔드포인트를 체크하고, 상단에 클라이언트 구성 다운로드를 클릭합니다. </br></br>
그리고 두 번째 사진과 같이 클라이언트 구성 다운로드 팝업창이 뜨면, 클라이언트 구성 다운로드를 클릭하면 Chrome 브라우저 기준으로 아래와 같이 구성 파일이 다운로드 된 것을 볼 수 있습니다.
![엔드포인트 생성15](/assets/2023-07/Client_VPN/2023-07-27-49.png) </br>
[엔드포인트 생성15](/assets/2023-07/Client_VPN/2023-07-27-49.png) </br>
![엔드포인트 생성16](/assets/2023-07/Client_VPN/2023-07-27-50.png) </br>
[엔드포인트 생성16](/assets/2023-07/Client_VPN/2023-07-27-50.png) </br>
![엔드포인트 생성17](/assets/2023-07/Client_VPN/2023-07-27-51.png) </br>
[엔드포인트 생성17](/assets/2023-07/Client_VPN/2023-07-27-51.png) </br></br>
이렇게 해당 페이지에서는 AWS Client VPN 엔드포인트를 구성했습니다. 
이 과정을 통해서 인증에서는 인증서를 통해, 상호 인증을 사용해서 VPC(10.0.0.0/16)에 대한 액세스만 클라이언트 VPN 연결을 경유하도록 설정했습니다.


### VPN 연결
해당 과정에서는 AWS Clinet VPN 엔드포인트에 연결하기 위한 도구를 설치하고 실제로 VPN 연결을 수행할 겁니다.
제가 사용한 방법은 아래의 AWS WorkShop에서 제공하는 AWS 클라이언트 VPN 다운로드 툴을 사용했고, 두 번째는 OpenVPN 툴을 사용하여 접속을 하였습니다.

먼저 AWS WorkShop에서 제공하는 AWS 클라이언트 VPN 다운로드 툴로 접속을 시도합니다.
1. 아래의 링크에  들어가서  AWS 클라이언트 VPN 다운로드 하이퍼링크를 클릭하여 다운로드를 받습니다.  
[AWS Client VPN Download 검색](https://catalog.us-east-1.prod.workshops.aws/workshops/be2b90c2-06a1-4ae6-84b3-c705049d2b6f/ja-JP/03-hands-on/03-02-mutal/03-vpn-connection)

2. 그리고 이전 항목에서 다운로드했던 downloaded-client-config.ovpn 파일을 편집해야 합니다.  </br></br>
먼저 해당 파일을 메모장으로 변환 후 들어간 다음, <cert></cert>와< key></key> 설정을 끝에 추가합니다.  </br></br>
그리고 해당 부분에서는 client.domain.tld에서 .crt 부분과 .key 부분에서 각각 ------BEGIN CERTIFICATE----- 에서 ------END CERTIFICATE----- 부분을 복사해서 붙여넣어야 합니다.      
(이 과정은 전에 인증서 만들기 과정에서 설정한 사항입니다. 이름이 다르다면 client.domain.tld가 아닐 수 있습니다. 또한 해당 .crt 파일과 .key 파일을 열 때에도 연결 프로그램을 메모장으로 변환하여 열어주면 됩니다!) </br></br>
해당 과정을 거치면 아래와 같게 됩니다! </br></br>
그리고 저장을 클릭해주세요.
![VPN 연결1](/assets/2023-07/Client_VPN/2023-07-27-52.png)  
[VPN 연결1](/assets/2023-07/Client_VPN/2023-07-27-52.png)   
위의 사진을 보면 저는 client.domain.tld의 .crt 파일과 .key 파일, downloaded-client-config.ovpn 파일을 모두 메모장으로 연결 프로그램을 변경 후 열었습니다.   
그리고 downloaded-client-config.ovpn 파일에서 client.domain.tld의 .crt 파일과 .key 파일에서 ------BEGIN CERTIFICATE----- 에서 ------END CERTIFICATE----- 부분의 내용을 복사해서 붙여넣었습니다. 
그리고 `<cert>,</cert>`와`<key>,</key>`의 분류는 위 사진과 같이 된겁니다.  
다시 말하면   
`<cert>`
client.domain.tld .crt 파일의 ------BEGIN CERTIFICATE----- 에서 ------END CERTIFICATE----- 부분의 내용
`</cert>`  
`< key>`  
client.domain.tld .key 파일의 ------BEGIN CERTIFICATE----- 에서 ------END CERTIFICATE----- 부분의 내용  
`</key>`    
이렇게 정리됩니다.  
추가적으로 저는 downloaded-client-config.ovpn 파일의 이름을 rmi_cto.ovpn 으로 변경하였습니다!
![VPN 연결2](/assets/2023-07/Client_VPN/2023-07-27-53.png)
[VPN 연결2](/assets/2023-07/Client_VPN/2023-07-27-53.png) 

3. 다음으로 다운로드 받았던 AWS 클라이언트 VPN 툴을 클릭해서 들어갑니다. 그리고 아래와 같은 과정을 따라줍니다.

![VPN 연결3](/assets/2023-07/Client_VPN/2023-07-27-54.png)
[VPN 연결3](/assets/2023-07/Client_VPN/2023-07-27-54.png)

![VPN 연결4](/assets/2023-07/Client_VPN/2023-07-27-55.png)  
[VPN 연결4](/assets/2023-07/Client_VPN/2023-07-27-55.png)

![VPN 연결5](/assets/2023-07/Client_VPN/2023-07-27-56.png)  
[VPN 연결5](/assets/2023-07/Client_VPN/2023-07-27-56.png)

![VPN 연결6](/assets/2023-07/Client_VPN/2023-07-27-57.png)  
[VPN 연결6](/assets/2023-07/Client_VPN/2023-07-27-57.png)  

![VPN 연결7](/assets/2023-07/Client_VPN/2023-07-27-58.png)  
[VPN 연결7](/assets/2023-07/Client_VPN/2023-07-27-58.png)

![VPN 연결8](/assets/2023-07/Client_VPN/2023-07-27-59.png)  
[VPN 연결8](/assets/2023-07/Client_VPN/2023-07-27-59.png)
 
![VPN 연결9](/assets/2023-07/Client_VPN/2023-07-27-60.png)  
[VPN 연결9](/assets/2023-07/Client_VPN/2023-07-27-60.png)

![VPN 연결10](/assets/2023-07/Client_VPN/2023-07-27-61.png)  
[VPN 연결10](/assets/2023-07/Client_VPN/2023-07-27-61.png)  

4. 이렇게 마지막 사진과 같이 잘 연결된 것을 볼 수 있습니다.
그리고 콘솔에서도 해당 사항을 확인할 수 있습니다!
해당 콘솔로 접속해서 확인이 가능합니다! (VPC -> Client VPN Endpoint 에서 해당되는 클라이언트 VPN 엔드포인트 선택 후 연결 클릭)  
연결 부분에서는 해당 연결의 상세한 정보를 볼 수 있어요

![VPN 연결11](/assets/2023-07/Client_VPN/2023-07-27-62.png)  
[VPN 연결11](/assets/2023-07/Client_VPN/2023-07-27-62.png)

![VPN 연결12](/assets/2023-07/Client_VPN/2023-07-27-63.png)  
[VPN 연결12](/assets/2023-07/Client_VPN/2023-07-27-63.png)  

5. 또한 기존에 만들었던 EC2 인스턴스와 통신이 되는 지 확인하기 위하여 ping 명령어를 입력합니다.
(만약 Windows cmd에서 ping 명령어 인식이 안되는 경우, 환경변수가 설정되지 않아 그럴 수 있습니다. 이럴 때의 경로는C:\Windows\System32 <-이겁니다!)  
먼저 EC2 인스턴스의 프라이빗 IPv4 주소 확인을 합니다.

![VPN 연결13](/assets/2023-07/Client_VPN/2023-07-27-64.png)  
[VPN 연결13](/assets/2023-07/Client_VPN/2023-07-27-64.png)

```py
## Ping 보내기
ping <해당 Private IP 주소>

--> ping 10.0.10.114

======출력 화면=====

C:\>cd Windows\System32

C:\Windows\System32>ping 0.0.0.0

Ping 0.0.0.0 32바이트 데이터 사용:
PING: 전송하지 못했습니다. 일반 오류입니다.
PING: 전송하지 못했습니다. 일반 오류입니다.
PING: 전송하지 못했습니다. 일반 오류입니다.
PING: 전송하지 못했습니다. 일반 오류입니다.

0.0.0.0에 대한 Ping 통계:
    패킷: 보냄 = 4, 받음 = 0, 손실 = 4 (100% 손실),

C:\Windows\System32>

C:\Windows\System32>ping 10.0.10.114

Ping 10.0.10.114 32바이트 데이터 사용:
10.0.10.114의 응답: 바이트=32 시간=9ms TTL=126
10.0.10.114의 응답: 바이트=32 시간=6ms TTL=126
10.0.10.114의 응답: 바이트=32 시간=7ms TTL=126
10.0.10.114의 응답: 바이트=32 시간=6ms TTL=126

10.0.10.114에 대한 Ping 통계:
    패킷: 보냄 = 4, 받음 = 4, 손실 = 0 (0% 손실),
왕복 시간(밀리초):
    최소 = 6ms, 최대 = 9ms, 평균 = 7ms

C:\Windows\System32>
```

![VPN 연결14](/assets/2023-07/Client_VPN/2023-07-27-65.png)  
[VPN 연결14](/assets/2023-07/Client_VPN/2023-07-27-65.png)  

이렇게 통신도 잘 되는 것을 볼 수 있습니다!  
해당 연결은 아래와 같이 종료할 수 있습니다.

![VPN 연결15](/assets/2023-07/Client_VPN/2023-07-27-66.png)  
[VPN 연결15](/assets/2023-07/Client_VPN/2023-07-27-66.png)  

그리고 핑 명령어와 콘솔 확인을 통해서 완전히 연결이 끊어진 것을 볼 수 있습니다.
```py
C:\Windows\System32>ping 10.0.10.114

Ping 10.0.10.114 32바이트 데이터 사용:
10.128.128.128의 응답: 대상 네트워크에 연결할 수 없습니다.
10.128.128.128의 응답: 대상 네트워크에 연결할 수 없습니다.
10.128.128.128의 응답: 대상 네트워크에 연결할 수 없습니다.
10.128.128.128의 응답: 대상 네트워크에 연결할 수 없습니다.

10.0.10.114에 대한 Ping 통계:
    패킷: 보냄 = 4, 받음 = 4, 손실 = 0 (0% 손실),

C:\Windows\System32>
```

![VPN 연결16](/assets/2023-07/Client_VPN/2023-07-27-67.png)
[VPN 연결16](/assets/2023-07/Client_VPN/2023-07-27-67.png)

![VPN 연결17](/assets/2023-07/Client_VPN/2023-07-27-68.png)
[VPN 연결17](/assets/2023-07/Client_VPN/2023-07-27-68.png)

+) 추가적인 Open VPN 프로그램을 이용할 때는 기존사항에서 변경해야 할 사항이 우선 있습니다.

Open VPN 도구를 이용하여 연결할 시, 아래 사진과 같이 downloaded-client-config.ovpn (rmi_cto.ovpn) 파일에서 DNS 이름에 해당되는 부분을 찾아서 수정해야 합니다. (두번째 사진처럼 앞에 이름을 넣고 . 을 찍어주면 됩니다!)

![VPN 연결18](/assets/2023-07/Client_VPN/2023-07-27-69.png)
[VPN 연결18](/assets/2023-07/Client_VPN/2023-07-27-69.png)

![VPN 연결19](/assets/2023-07/Client_VPN/2023-07-27-70.png)
[VPN 연결19](/assets/2023-07/Client_VPN/2023-07-27-70.png)

그리고 OpenVPN 프로그램을 열어서 아래 사진과 같이 해주시면, 정상적인 통신이 되는 것을 확인할 수 있습니다.  
![VPN 연결20](/assets/2023-07/Client_VPN/2023-07-27-71.png)
[VPN 연결20](/assets/2023-07/Client_VPN/2023-07-27-71.png)

![VPN 연결21](/assets/2023-07/Client_VPN/2023-07-27-72.png)
[VPN 연결21](/assets/2023-07/Client_VPN/2023-07-27-72.png)

![VPN 연결22](/assets/2023-07/Client_VPN/2023-07-27-73.png)
[VPN 연결22](/assets/2023-07/Client_VPN/2023-07-27-73.png)

![VPN 연결23](/assets/2023-07/Client_VPN/2023-07-27-74.png)
[VPN 연결23](/assets/2023-07/Client_VPN/2023-07-27-74.png)

![VPN 연결24](/assets/2023-07/Client_VPN/2023-07-27-75.png)
[VPN 연결24](/assets/2023-07/Client_VPN/2023-07-27-75.png)

![VPN 연결25](/assets/2023-07/Client_VPN/2023-07-27-76.png)
[VPN 연결25](/assets/2023-07/Client_VPN/2023-07-27-76.png)

![VPN 연결26](/assets/2023-07/Client_VPN/2023-07-27-77.png)
[VPN 연결26](/assets/2023-07/Client_VPN/2023-07-27-77.png)

![VPN 연결27](/assets/2023-07/Client_VPN/2023-07-27-78.png)
[VPN 연결27](/assets/2023-07/Client_VPN/2023-07-27-78.png)

![VPN 연결28](/assets/2023-07/Client_VPN/2023-07-27-79.png)
[VPN 연결28](/assets/2023-07/Client_VPN/2023-07-27-79.png)

![VPN 연결29](/assets/2023-07/Client_VPN/2023-07-27-80.png)
[VPN 연결29](/assets/2023-07/Client_VPN/2023-07-27-80.png)

![VPN 연결30](/assets/2023-07/Client_VPN/2023-07-27-81.png)
[VPN 연결30](/assets/2023-07/Client_VPN/2023-07-27-81.png)


### 로그 확인

CloudWatch 콘솔로 이동해서 만든 로그 그룹(/aws/clientvpn)을 클릭하고, 그 아래의 만든 로그 스트림(rmi_clinent_log)을 클릭합니다. 이렇게 해서 아래 사진과 같이 연결 로그를 확인할 수 있었습니다.

![Log 확인](/assets/2023-07/Client_VPN/2023-07-27-82.png)
[Log 확인](/assets/2023-07/Client_VPN/2023-07-27-82.png)

|프로젝트|설정값|
|---|---|
|연결 시도 상태|상태|
|연결 시작 시간|연결 시작 시간|
|클라이언트 IP|클라이언트 CIDR에서 할당된 IP|
|일반 이름|인증서의 CommonName|
|장치-ip|접속원 PC의 IP|


### 운영 환경 도중 Client 인증서 삭제 시

먼저 Windows 운영 체제를 사용한다면 터미널이나 CMD를 관리자 권한으로 실행해주세요.
(관리자 권한으로 실행하지 않을 시, revoke 작업을 해결하지 못할 수 있습니다.)

#### 명령어 입력
```py
## easyrsa를 시작할 수 있는 폴더로 이동 후 start
(해당 경로 및 easyrsa의 버전에 따라 명령어가 달라질 수 있습니다. 현재 아래의 명령어 같은 경우는
컴퓨터가 인식할 수 있게 몇 글자를 치고 tab 버튼을 눌러 자동 완성으로 한 사항입니다.)

C:\>cd "Program Files\OpenVPN\EasyRSA-3.1.0"

C:\Program Files\OpenVPN\EasyRSA-3.1.0>EasyRSA-Start.bat

## client.domain.tld 제거
# ./easyrsa revoke client.domain.tld

(도중에 yes 입력해서 계속 진행)

## EasyRSA 스크립트를 사용하여 인증서 폐지 목록(Certificate Revocation List, CRL)을 생성
# ./easyrsa gen-crl

(해당 과정을 성공적으로 거쳤다면, 아래와 같은 메시지가 출력이 됩니다. 거기서 경로를 알 수 있습니다.)
An updated CRL has been created.
CRL file: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/crl.pem

```
#### 명령어 출력
```py
Microsoft Windows [Version 10.0.22000.1696]
(c) Microsoft Corporation. All rights reserved.

C:\Users\kimhj>cd ..

C:\Users>cd ..

C:\>cd "Program Files\OpenVPN\EasyRSA-3.1.0"

C:\Program Files\OpenVPN\EasyRSA-3.1.0>EasyRSA-Start.bat

Welcome to the EasyRSA 3 Shell for Windows.
Easy-RSA 3 is available under a GNU GPLv2 license.

Invoke './easyrsa' to call the program. Without commands, help is displayed.

EasyRSA Shell
# ./easyrsa revoke client.domain.tld
* Notice:
Using Easy-RSA configuration from: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/vars

* Notice:
Using SSL: openssl OpenSSL 3.0.3 3 May 2022 (Library: OpenSSL 3.0.3 3 May 2022)


  Please confirm you wish to revoke the certificate
  with the following subject:

  subject=
    commonName                = client.domain.tld

  serial-number: F1AD62ACC67E777EA00A1AD885DAFEF4


Type the word 'yes' to continue, or any other input to abort.
    Continue with revocation: yes

Using configuration from C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/safessl-easyrsa.cnf.init-tmp
Revoking Certificate F1AD62ACC67E777EA00A1AD885DAFEF4.
Data Base Updated

* Notice:

IMPORTANT!!!

Revocation was successful. You must run gen-crl and upload a CRL to your
infrastructure in order to prevent the revoked cert from being accepted.


EasyRSA Shell
# ./easyrsa gen-crl
* Notice:
Using Easy-RSA configuration from: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/vars

* Notice:
Using SSL: openssl OpenSSL 3.0.3 3 May 2022 (Library: OpenSSL 3.0.3 3 May 2022)

Using configuration from C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/safessl-easyrsa.cnf.init-tmp

* Notice:

An updated CRL has been created.
CRL file: C:/Program Files/OpenVPN/EasyRSA-3.1.0/pki/crl.pem


EasyRSA Shell
#
```

#### 명령어 출력시 캡쳐화면
![운영 환경에서의 Client 인증서 삭제1](/assets/2023-07/Client_VPN/2023-07-27-83.png)
[운영 환경에서의 Client 인증서 삭제1](/assets/2023-07/Client_VPN/2023-07-27-83.png)

이제 생성된 CRL 파일의 내용을 AWS 환경에 적용해야 할 차례입니다.
다시 콘솔로 접속 후 VPC 콘솔에 접근한 뒤, 좌측 사이드 바에서 [Client VPN 엔드포인트]를 클릭해주세요.
그리고 해당 클라이언트 VPN 엔드포인트를 선택하고 작업 -> 클라이언트 인증서 CRL 가져오기를 클릭합니다.

![운영 환경에서의 Client 인증서 삭제2](/assets/2023-07/Client_VPN/2023-07-27-84.png)
[운영 환경에서의 Client 인증서 삭제2](/assets/2023-07/Client_VPN/2023-07-27-84.png)

여기선 아까 gen-crl 명령어 입력 시에 나왔던 crl.pem 파일이 있는 경로로 이동해줍니다.

그리고 해당 파일을 열어 내용을 복사해줍니다.

다시 콘솔로 돌아와서 세번째 사진과 같이 pem 파일의 내용을 인증서 취소 목록 란에 
붙여넣기 해주고 나서 하단에 클라이언트 인증서 CRL 가져오기를 클릭합니다.

![운영 환경에서의 Client 인증서 삭제3](/assets/2023-07/Client_VPN/2023-07-27-85.png)
[운영 환경에서의 Client 인증서 삭제3](/assets/2023-07/Client_VPN/2023-07-27-85.png)
![운영 환경에서의 Client 인증서 삭제4](/assets/2023-07/Client_VPN/2023-07-27-86.png)
[운영 환경에서의 Client 인증서 삭제4](/assets/2023-07/Client_VPN/2023-07-27-86.png)
![운영 환경에서의 Client 인증서 삭제5](/assets/2023-07/Client_VPN/2023-07-27-87.png)
[운영 환경에서의 Client 인증서 삭제5](/assets/2023-07/Client_VPN/2023-07-27-87.png)

아래와 같이 정상적으로 작용이 되었으면 콘솔 상단에 적용되었다고 초록색 팝업창이 뜹니다.

![운영 환경에서의 Client 인증서 삭제6](/assets/2023-07/Client_VPN/2023-07-27-88.png)
[운영 환경에서의 Client 인증서 삭제6](/assets/2023-07/Client_VPN/2023-07-27-88.png)

추가적으로 CRL에 포함된 인증서를 다시 VPN 툴인 AWS VPN Client와 OpenVPN에서 접속한 결과 아래와 같은 에러 메시지가 발생하는 것을 볼 수 있습니다.

![운영 환경에서의 Client 인증서 삭제7](/assets/2023-07/Client_VPN/2023-07-27-89.png)  
[운영 환경에서의 Client 인증서 삭제7](/assets/2023-07/Client_VPN/2023-07-27-89.png)  
![운영 환경에서의 Client 인증서 삭제8](/assets/2023-07/Client_VPN/2023-07-27-90.png)  
[운영 환경에서의 Client 인증서 삭제8](/assets/2023-07/Client_VPN/2023-07-27-90.png)  
![운영 환경에서의 Client 인증서 삭제9](/assets/2023-07/Client_VPN/2023-07-27-91.png)  
[운영 환경에서의 Client 인증서 삭제9](/assets/2023-07/Client_VPN/2023-07-27-91.png)  
![운영 환경에서의 Client 인증서 삭제10](/assets/2023-07/Client_VPN/2023-07-27-92.png)  
[운영 환경에서의 Client 인증서 삭제10](/assets/2023-07/Client_VPN/2023-07-27-92.png)  




### 리소스 삭제

삭제 순서는 아래와 같습니다.  
1. VPN 엔드포인트 삭제  
- 클라이언트 엔드포인트 선택 후 대상 네트워크 연결에서 대상 네트워크 연결 해제 클릭
- 클라이언트 VPN 엔드포인트 선택 후 작업 -> 클라이언트 VPN 엔드포인트 삭제  
위 과정은 15-20분 정도 소요됩니다.

2. ACM에 등록한 인증서 삭제
- ACM 콘솔 -> 인증서 나열 -> server, client.domain.tld 인증서 모두 삭제

3. 로그 출력 대상 삭제
- CloudWatch 관리 화면 이동
- 로그 -> 로그 그룹(/aws/clientvpn) 선택 -> 작업 -> 로그 그룹 삭제 클릭

4. EC2 인스턴스 삭제

5. 해당 VPC 삭제

6. 기타 리소스 삭제 기타 작성한 자원을 삭제합니다. 삭제하지 않으면 문제가 없습니다.  
- 연결 클라이언트 제거  
macOS/Windows 각 OS의 특정 단계에 따라 라는 AWS VPN Client이름의 앱을 삭제합니다.

- 작성한 인증서 삭제  
핸즈온을 순서대로 진행했을 경우, Windows는 C:\custom_folder, Linux/macOS ~/custom_folder/에는 , 다음의 파일을 할 수 있으므로, 불필요한 경우 삭제해 주세요.


## 출처 및 참고

[AWS Client VPN Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/be2b90c2-06a1-4ae6-84b3-c705049d2b6f/ja-JP/03-hands-on/03-01-common/04-certificate)

[AWS Client VPN이란](https://docs.aws.amazon.com/ko_kr/vpn/latest/clientvpn-admin/what-is.html)

[AWS Client VPN의 작동 방식](https://docs.aws.amazon.com/ko_kr/vpn/latest/clientvpn-admin/how-it-works.html)