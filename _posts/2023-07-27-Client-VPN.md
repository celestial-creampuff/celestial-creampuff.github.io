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

#### Client VPN 이란?

AWS Client VPN은 AWS 리소스 및 온프레미스 네트워크의 리소스에 안전하게 액세스할 수 있는 클라이언트 기반 관리형 VPN 서비스입니다. 클라이언트 VPN을 사용하면 OpenVPN 기반의 VPN 클라이언트를 사용하여 어디서나 리소스에 액세스가 가능합니다.

## Hands-On

해당 핸즈온은 아래와 같은 특징을 가지고 있습니다!

#### 구성도

(추가하기)

#### 실습환경

- Chrome 브라우저 사용
- 로컬 = Windows 환경
- ap-northeast-2(서울 리전)

### VPC 환경 설정

#### VPC 구축

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



### VPN 연결



### 로그 확인



### 운영 환경 도중 Client 인증서 삭제 시



### 리소스 삭제



## 출처 및 참고자료





## 출처 및 참고

[AWS Client VPN Workshop](https://catalog.us-east-1.prod.workshops.aws/workshops/be2b90c2-06a1-4ae6-84b3-c705049d2b6f/ja-JP/03-hands-on/03-01-common/04-certificate)

[볼륨 크기 조정 후 Windows 파일 시스템 확장](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/WindowsGuide/recognize-expanded-volume-windows.html)

[https://kim-dragon.tistory.com/4](https://kim-dragon.tistory.com/4)