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
#### Client VPN 이란?</br>

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
![VPC 생성](/assets/2023-07/Client_VPN/2023-07-27-1.png)</br>
[VPC 생성](/assets/2023-07/Client_VPN/2023-07-27-1.png)

3. VPC를 만들었다면 서브넷 4개를 만들어줍니다.
아래와 같은과정을 거쳐 서브넷을 만들어주세요.
새 서브넷 추가를 클릭하여 다른 서브넷을 만들 수 있으며, 서울 지역은 A와C 부분의 가용영역을 사용합니다.</br></br>
해당 과정을 잘 거쳤다면 두 번째 사진과 같이 서브넷 4개가 만들어진 것을 볼 수 있습니다.
![서브넷 구성](/assets/2023-07/Client_VPN/2023-07-27-2.png)</br>
[서브넷 구성](/assets/2023-07/Client_VPN/2023-07-27-2.png)</br>
![서브넷 구성](/assets/2023-07/Client_VPN/2023-07-27-3.png)</br>
[서브넷 구성](/assets/2023-07/Client_VPN/2023-07-27-3.png)</br>
### 인스턴스 생성


### CloudWatch Log 그룹 설정

### 인증서 만들기

### Client VPN 엔드포인트 생성

### VPN 연결

### 로그 확인

### 운영 환경 도중 Client 인증서 삭제 시

### 리소스 삭제

## 출처 및 참고자료




## 출처 및 참고

[EBS 개요](https://aws.amazon.com/ko/ebs/)

[볼륨 크기 조정 후 Windows 파일 시스템 확장](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/WindowsGuide/recognize-expanded-volume-windows.html)

[https://kim-dragon.tistory.com/4](https://kim-dragon.tistory.com/4)