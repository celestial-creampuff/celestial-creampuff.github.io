---
title: "[AWS] 기초부터 시작하는 AWS 구축하기 5"
date: 2023-08-20 04:00:00 -0400
categories: AWS VPC Routing_Table
tags:
    - AWS
    - VPC
    - Routing Table
toc: true
toc_sticky: true
---

## 학습목표
이번 게시물의 학습목표는 아래와 같다.
- Routing Table의 개념을 이해한다.
- Routing Table이 왜 필요한지 공부하며 구축할 때 어떻게 하는지 기본적으로 알아본다.
- 가상 네트워크를 구축할 때 어떠한 요소가 필요한지 이해해본다.

## 1. Routing Table이란?
여태까지 Internet Gateway와 Nat Gateway를 만들고 VPC와 연결하면서 여기까지 왔다. 하지만 제대로 역할을 해주기 위해서는 Routing Table에서 설정을 해야 완벽한 기능을 할 수 있다. 이 Routing Table이란 무엇일까?<br>

**Routing Table(RTB, 라우팅 테이블)** 을 설명하기에 앞서 온프레미스 장비에서는 라우터라고 있다. 이 라우터는 네트워크 요청에 의해 데이터가 들어왔을 때 어디로 라우팅을 할 지 설정해준다고 한다.<br> 여기서 Route는 영어를 한국어로 번역하면 목적지이고 라우팅 테이블은 각 목적지에 대해 이정표인 셈이다. 우리가 고속도로를 달릴 때 어디로 향하는지, 어디로 빠져야 할 지 위에 간판이 종종 보일텐데 그런 것이라 생각하면 편하다.<br>
즉, **라우팅 테이블**이란 서브넷 또는 게이트웨이의 네트워크 트래픽이 어디로 전송될 지 위치를 결정하는 규칙 세트이자 집합이며 목적지에 대한 이정표이다.

## 2. Routing Table은 왜 설정해줘야 할까?
방금전에 내가 라우팅 테이블을 고속도로 위의 표지판이라고 비유했는데 말 그대로 라우팅 테이블은 제대로 설정을 해줘야 한다. 왜냐하면 네트워크 트래픽은 라우팅 테이블을 보고 오고 가기 때문에 제대로 설정하지 않으면 트래픽이 엉뚱한 곳으로 가거나 원했던 목적지가 아닌 다른 곳으로 가서 통신이 이루어지지 않는 답답함을 느낄 수 있다.

나는 여기서 Routing Table을 2개를 만들 것이다!
하나는 퍼블릭 서브넷 전용 라우팅 테이블이고, 다른 하나는 프라이빗 서브넷 전용의 라우팅 테이블이다.

## 3. Public Subnet 라우팅 테이블 구축
Public Subnet에 대한 라우팅 테이블부터 만들어보자!

1. 먼저 VPC 콘솔에서 라우팅 테이블에서 라우팅 테이블 생성을 클릭한다. 아래 화면은 내가 잘못 캡쳐해서... 우측 상단에 주황색으로 된 생성 버튼을 클릭하면 된다!
![Public Subnet RTB 구축 1](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.12.57.png)<br><br>
2. 첫 설정은 이름을 아래처럼 입력해주고 배치할 VPC를 선택하고, 하단에 라우팅 테이블 생성을 클릭하면 된다.
![Public Subnet RTB 구축 2](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.13.34.png)<br><br>
3. 이렇게 퍼블릭 서브넷에 대한 라우팅 테이블이 생성되었다. 하지만 여기서 끝이 아니다ㅜㅜ<br>
만들어준 라우팅 테이블에서 우리는 라우팅과 어떤 서브넷을 연결할 것인지 설정해줘야 한다.<br>
라우팅에서 라우팅 편집을 클릭해보자.
![Public Subnet RTB 구축 3](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.14.00.png)<br><br>
4. 강조 표시는 안했지만 아래 화면처럼 좌측 하단에 라우팅 추가를 눌러주고 대상에 0.0.0.0/0을 입력해준다.<br> 그리고 그 옆 대상에 인터넷 게이트웨이를 클릭한다.<br>
왜냐하면 퍼블릭 라우팅의 경우 모든 대상(0.0.0.0/0)에 대해서 인터넷 연결을 해줘야 하며 그 대상은 VPC와 인터넷을 연결해야 하는 인터넷 게이트웨이가 되기 때문이다.
![Public Subnet RTB 구축 4](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.14.29.png)<br><br>
5. 그렇다면 아래와 같이 내가 만들어줬던 VPC가 보일텐데, 그것을 클릭하고 우측 하단에 변경 사항 저장을 클릭한다.
![Public Subnet RTB 구축 5](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.14.52.png)<br><br>
6. 이번에는 서브넷을 연결해주자.<br>
먼저 서브넷 연결에 들어가 서브넷 연결 편집을 클릭하자.
![Public Subnet RTB 구축 6](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.15.21.png)<br><br>
7. 그리고 해당 Roting Table은 퍼블릭 서브넷 전용이니까 내가 퍼블릭 서브넷으로 설정해준 서브넷들을 체크하고 연결 저장을 클릭한다.
![Public Subnet RTB 구축 7](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.15.32.png)<br><br>
8. 그러면 아래와 같이 명시적 서브넷 연결이 아래와 같이 뜨면 성공이다!
![Public Subnet RTB 구축 7](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.15.47.png)<br><br>

## 4. Private Subnet 라우팅 테이블 구축
퍼블릭 라우팅 테이블을 구축했을 때와 마찬가지로 다시 라우팅 테이블 생성을 클릭하여<br> 이번에는 프라이빗 라우팅 테이블을 만들어줬다.
![Private Subnet RTB 구축 1](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.16.34.png)<br><br>
2. 라우팅에서 라우팅 편집을 클릭한다.
![Private Subnet RTB 구축 2](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.16.44.png)<br><br>
3. 좌측 하단에 라우팅 추가를 누르고 0.0.0.0/0을 입력해주고, 이번에는 NAT 게이트웨이을 클릭한다.<br> 이것도 퍼블릭 라우팅 테이블과 이유가 유사하지만, 프라이빗 라우팅 테이블의 경우 모든 대상(0.0.0.0/0)에 대해 아웃바운드 인터넷 연결 역할을 해줘야한다.
![Private Subnet RTB 구축 3](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.17.18.png)<br><br>
4. 그러면 내가 만들었던 NAT Gateway가 뜨는데 해당 NAT 게이트웨이를 클릭하고 변경 사항 저장을 클릭한다.
![Private Subnet RTB 구축 4](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.17.27.png)<br><br>
5. 이렇게 라우팅이 설정되었다. 
![Private Subnet RTB 구축 5](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.17.40.png)<br><br>
6. 다음에는 서브넷 연결을 편집해보자.
![Private Subnet RTB 구축 6](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.17.52.png)<br><br>
7. Private Subnet 전용 Routing Table인 만큼 내가 Private Subnet으로 배치하고 싶었던 서브넷들을 체크해서 연결 저장을 클릭해줬다!
![Private Subnet RTB 구축 7](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.18.13.png)<br><br>
8. 이렇게 프라이빗 서브넷 라우팅 테이블까지 모두 설정을 완료하였다..🫠
![Private Subnet RTB 구축 8](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.18.39.png)<br><br>

여담이지만 아까 NAT Gateway를 만들었을 때 상태가 Pending 이었는데 지금은 상태가 Availiable로 되어 있는 것을 볼 수 있다!
![NAT Gateway 여담](/assets/2023-08/Routing_Table/스크린샷%202023-08-20%20오후%204.26.00.png)
<br><br>

<br>
이렇게 라우팅 테이블까지 구축하면서 길면 길만한 VPC 기본적인 구축을 끝냈다. 그리고 내가 원하는 리소스를 서브넷에 적절히 배치하면서 연결하는 작업을 하면 된다! <br>

사실 VPC를 구성하는데에는 이보다 더욱 많은 요소를 배치할 수 있다.(VPC Flow Log, VPN 구성, S3 Endpoint, VPC EndPoint 등등)

<br>
해당 서비스들은 더욱 공부하고 테스트해보고 블로그에 작성해보는 걸로 하자!

<br>
조금 여담이지만 이것이 AWS Best Practice가 아닐 수도 있다...<br>
<br>
끝!!




## 참고 및 출처

[라우팅 테이블](https://docs.aws.amazon.com/ko_kr/vpc/latest/userguide/VPC_Route_Tables.html)

[Routing Table](https://www.notion.so/Routing-Table-e7dcb51216f74e5fba4144cd1d5724b4)

[[AWS] 가장쉽게 VPC 개념잡기](https://medium.com/harrythegreat/aws-가장쉽게-vpc-개념잡기-71eef95a7098)

[[AWS] VPC란?](https://blog.kico.co.kr/2022/03/08/aws-vpc/)