---
title: "[AWS] 기초부터 시작하는 AWS 구축하기 4"
date: 2023-08-20 03:00:00 -0400
categories: AWS VPC NAT_Gateway
tags:
    - AWS
    - VPC
    - NAT Gateway
toc: true
toc_sticky: true
---

## 학습목표
이번 게시물의 학습목표는 아래와 같다.
- NAT Gateway의 개념을 이해한다.
- NAT Gateway가 왜 필요한지 공부하며 구축할 때 어떻게 하는지 기본적으로 알아본다.
- 가상 네트워크를 구축할 때 어떠한 요소가 필요한지 이해해본다.

## 1. NAT Gateway란?
저번 시간에는 Internet Gateway가 외부 인터넷과 VPC를 통신 수단인 관문이라고 배웠다.<br>
그렇다면 NAT Gateway란 무엇일까?<br><br>
**NAT Gateway** 란 원래 온프레미스에서 NAT을 뜻한다. 이것의 약자를 풀면 Network Address Translation이다. 즉, AWS에서 NAT Gateway는 네트워크 주소 변환 서비스이며, 이 서비스를 통해 프라이빗 서브넷의 인스턴스가 VPC 외부의 서비스에 연결할 수 있지만, 외부 서비스에서는 이러한 인스턴스와의 연결을 시작할 수 없도록 하기 위해 사용하고 있다.
<br><br>
프라이빗 서브넷은 애시당초에 인터넷 게이트웨이가 연결되어 있다고 한들, 외부와의 통신은 못하게끔 설정되어 있다. 하지만 프라이빗 서브넷에 배치된 리소스도 결국 통신이 되려면 NAT가 필요하다.
<br><br>

## 2. NAT Gateway의 요금이 있다고?
NAT Gateway는 이제 비용이 과금되는 서비스이다.<br>
이 글을 작성한 시점에서 NAT Gateway는 서울리전 기준으로 시간당 0.059$이며, 내가 이 글을 작성하기 위해 테스트 하고 있는 리전인 오하이오 에서는 0.045$가 시간당 부과된다.<br>(730시간 * 0.059$ = 43.07$.....) <br><br> 그래서 운영환경이 아닌 테스트 환경에서 프라이빗 리소스를 외부와 연결시키기 위해 NAT Gateway를 사용한다면, 목적이 이루었거나 중단 시 NAT는 삭제하고 다시 만들고 라우팅 테이블에 연결하여 테스트 하는 것을 권장한다.

## 3. NAT Gateway는 어떻게 쓸 수 있을까?
NAT Gateway는 아까 말했듯이 Private Subnet이 인터넷과 통신하기 위해서 Private Subnet에서 외부 인터넷에 요청하는 Outbound Traffic을 받아 IP 주소를 변환해 인터넷 게이트웨이로 보낸다는 사실을 알게 되었다.<br>

그리고 NAT Gateway는 아래와 같이 쓸 수 있다.
- AWS NAT Gateway 서비스를 이용한다. 물론 비용은 아까 말했듯이 과금된다.
- EC2를 NAT 서비스로 사용할 수 있다.(이건 나도 안해봤다.. 나중에 해보는걸로 하자!)
<br>
제일 중요한 것은 NAT Gateway는 **Public 서브넷에 위치**해야 한다. 그래야 중간단에서 트래픽을 오고가게 해주기 때문이다. <br>
그리고 이런 설정을 하기 위해서는 Routing Table 설정도 필요하다. 이 부분은 비용이 과금되니 집중해서 빨리 해보도록 하자!

## 4. NAT Gateway 설정
1. VPC 콘솔에서 좌측에서 NAT 게이트웨이를 클릭한 다음, NAT 게이트웨이 생성을 클릭한다.<br>
(여담이지만 맨 우측에 i 버튼을 누르면 리소스에 대해 간단한 설명을 볼 수 있다. 그러니 만약 모르겠으면 클릭해서 보는 것도 추천한다!)
![NAT Gateway 설정하기 1](/assets/2023-08/NAT_Gateway/스크린샷%202023-08-20%20오후%204.03.43.png)<br><br>
2. NAT 게이트웨이의 이름을 입력해주고, 배치하고 싶은 서브넷을 선택한다. 여기서 배치는 Public 서브넷에 배치해야 한다. 나의 경우 퍼블릭 서브넷 1에 위치시키고 연결 유형은 퍼블릭을 선택해주었다.<br>
그리고 탄력적 IP 할당은 필수로 해줘야 한다. 그래서 탄력적 IP를 안받은 상태라면 아래 내 화면처럼 탄력적 IP 할당을 클릭해준다.<br> 그러면 화면 위에 탄력적 IP주소가 할당되었다고 한다. 그리고 NAT 게이트웨이 생성을 클릭한다!
![NAT Gateway 설정하기 2](/assets/2023-08/NAT_Gateway/스크린샷%202023-08-20%20오후%204.04.37.png)<br><br>
3. NAT 게이트웨이가 정상적으로 생성이 되었다. 이때 상태는 pending 상태이며, 이때 동안의 Routung Table을 생성해보도록 하자!
![NAT Gateway 설정하기 3](/assets/2023-08/NAT_Gateway/스크린샷%202023-08-20%20오후%204.11.06.png)<br><br>

이제 기본적인 VPC 구축은 거의 끝이 보이고 있다.
Routing Table만 설정하면 기본적인 VPC 구실은 한다고 보면 된다!

<br>
끝.




## 참고 및 출처

[NAT 게이트웨이](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/vpc-nat-gateway.html)

[NAT Gateway](https://kimmanbo.notion.site/NAT-Gateway-7ab439d0c343413ba9b53d0a61877184)
