---
title: "[AWS] 기초부터 시작하는 AWS 구축하기 - Bastion Host EC2를 통한 Private 서브넷에 있는 EC2 접근하기(Windows 환경 - Putty편)"
date: 2023-09-20 04:00:00 -0400
categories: AWS EC2 Computing Server 
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
우리는 이미 Bastion Host의 의의를 알고 있으니, Windows 환경에서는 어떻게 접근하는지 알아보도록 하자!<br>
Windows 환경에서 Bastion을 통해서 Private Subnet 안에 있는 EC2에 접근하는 방법은 크게 두 가지가 있다.
1. Windows 10~11 환경에서 사용할 때 **터미널**을 이용하거나,
2. Putty를 사용하여 포트 포워딩을 통해 들어가거나.

<br>
이렇게 두 가지가 있는데, 해당 섹션에서는 Putty편을 다룰 것이다!<br>

**터미널** 편은 전 게시물을 참고하면 될 것 같다.🙂

## 들어가기에 앞서
일단 이 과정을 하기 전에 우리는 먼저 VPC를 구축하고 기본적인 구성을 완료하였다. 또한 Bastion Host 역할을 하는 EC2를 퍼블릭 서브넷에 생성까지 했다. 

그리고 이 과정은 이미 프라이빗 서브넷 안에 EC2가 있다고 가정하고, 핸즈 온을 시작한다.

만약 해당 과정을 안거쳤다면 **기초부터 시작하는 AWS 구축하기**를 참고해서 천천히 하나씩 따라와보자🙂



## Bastion Host를 통한 내부 EC2 접속 (Putty 편)
이번에는 putty를 통해 접속하는 방법을 알아가보도록 하자.
 
Putty로 Private EC2에 접속하는 방법은 두 가지가 있다.<br> 
첫 번째 방법은 Putty를 통해 Bastion에 들어간 다음, Bastion에서 로컬에 있는 키 파일을 복사하여 들어가는 방식이고<br>

두 번째는 바로 로컬에서 Private EC2로 접속하는 방법이다.
<br>
<br>
첫 번째부터 알아가보도록 하자!

### 첫 번째 방법 - Putty
1. 우선 아래의 웹 사이트에 putty를 다운로드 하자! (만약, putty가 미리 설치되어 있으면 1번은 건너뛰어도 된다.) <br><br>
https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
<br><br>

2. Puttygen을 검색해서 열고, Load 버튼을 클릭한다.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림1.png)<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림2.png)<br><br>

3. putty를 통하여 Bastion-EC2에 접속하기 위해서는 사전에 Bastion-EC2 전용으로 만들어뒀던 키 페어가 필요하다. 따라서 그것을 열기를 클릭해야 한다! (확장자는 All Files를 해야 목록이 보이는 것 같다..?)<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림3.png)<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림4.png)<br><br>
그리고 새로운 키를 저장할 때는 DEV-Bastion.ppk와 같이 확장자를 .ppk로 입력하고 저장을 클릭한다.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림5.png)<br><br>

4. 이제 putty에 들어가서 좌측에 SSH -> Auth 부분에서 Browse…를 클릭한다.<br>해당 과정은 키 페어를 찾는 부분이며, 전 단계에서 puttygen을 사용하여 .ppk를 만들었던 경로로 이동해서 알맞은 키를 선택해야 한다.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림6.png)<br><br>

5. AWS 콘솔에서 Bastion-EC2의 퍼블릭 IPv4 주소를 복사해주자. 그리고 putty로 돌아와서 좌측에서 Session을 클릭하고 Host Name에 퍼블릭 IPv4 주소를 붙여넣고 Open을 클릭하자.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림7.png)<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림8.png)<br>
(위 그림에서 표시된 Saved Sessions의 이름을 입력하고 Save를 누르면 다음부터는 접속하고자 할 때 편리하게 접속을 할 수 있다!!)<br>   
6. Putty에 접속 후 아래 화면에서 Accept를 클릭합니다.
![](/assets/2023-08/Bastion-Host/09_new/그림9.png)<br><br>
7. Login as에 ec2-user를 입력하면 Putty를 통해 Bastion-EC2에 접속이 성공적으로 이루어졌다. 아래부터는 Bastion Host를 이용하여 Private EC2에 접속 (Windows CMD편)과 유사하다고 보면 된다!<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림10.png)<br><br>
```py
## key 폴더 생성
mkdir key

## 내부 EC2에 접속하기 위하여 key 폴더 안에 DEV-Private.pem 파일 생성
touch key/DEV-Private.pem

## key 폴더 안에 DEV-Private.pem 파일 수정
vim key/DEV-Private.pem

### 이때 로컬에 있던 DEV-Private.pem 파일의 내용을 복사해서 Bastion-EC2에서 수정중인 DEV-Private.pem 파일에 복사 후 저장 (wq!)

## 키 권한 축소
chmod 400 key/DEV-Private.pem

## 내부 EC2 접속하기
ssh -i <키 파일 경로> ec2-user@<내부 EC2의 Private IP>

### 내부 EC2 접속하기 (예시)
ssh -i key/DEV-Private.pem ec2-user@10.200.10.123
```
<br>
접속 화면은 아래와 같다!


![](/assets/2023-08/Bastion-Host/09_new/그림11.png)<br><br>

이렇게 putty를 통해서 Private Subnet에 배치된 EC2에 접속을 시도하였고, 정상적으로 잘 되는 것을 확인할 수 있었다.👀 <br><br>
이제 putty를 통하여 포토포워딩 방식을 알아보도록 하자!
<br><br><br>

### 2. 두 번째 방법 - Bastion Host를 통한 내부 EC2 접속 (Putty를 통한 포트 포워딩 편)

이번 섹션에서는 Putty를 보다 더 자세히 다룰 것이니 꿀팁도 있으니까 한번 봐보도록 하자!

(먼저, 각각의 EC2에 할당된 키 페어가 다를 시, 방법 1에서 설명했던대로 Puttygen을 통하여 모든 키 페어 파일들을 .pem에서 .ppk로 바꿔야 한다.이 과정을 거친 후에, 아래의 방법을 따라야한다.)<br>
(Pageant를 이용하면 서버에 접속 시 필요한 키 페어 파일을 putty 프로그램에서 보관을 하는 것 같다. 그리고 키 페어 파일을 필요로 하는 서버에 맞게 연결시켜줘서 사용자가 서버에 접속할 때 마다 일일이 키 페어 파일을 탐색할 필요가 없게 된다.)<br><br>

1. Putty를 다운로드 받을 때 같이 배포되었던 pagent.exe를 실행한다.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림12.png)<br><br><br>
2. 실행 시, 아무런 프로그램이 출현하는 것이 없다!(이게 정상이다..)<br> 단 아래와 같이 더 보기를 누르면 Pageant가 실행되는 것을 볼 수가 있는데 그것을 더블클릭 해보자.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림13.png)<br><br><br>
3. 그러면 Pageant 창이 뜨는 것을 확인할 수 있다. 여기서 Add Key를 눌러 키 페어를 모두 추가해주자.<br>(**.pem** 형식이 아닌 **.ppk** 형식만 인식이 됩니다. 이 과정을 위해서 먼저 puttygen에서 .pem 파일들을 .ppk로 변환시켜준 이유가 이것 때문이다.)<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림14.png)<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림15.png)<br><br><br>
4. 아래와 같이 추가되었으면 close 버튼을 눌러서 빠져나오도록 하자.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림16.png)<br><br><br>
5. Putty 프로그램을 실행하여 SSH -> Auth에서 Authentication parameters에서는 SSH 인증 키를 안전하게 전달하고 재사용할 수 있도록 하는 기능을 제공한다. **Allow agent forwarding**을 체크하자.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림17.png)<br><br><br>
6. 이번에는 Auth -> Tunnels에서 Source port와 Destination을 설정해야 한다.<br><br>Source port는 현재 로컬에서 쓰지 않는 포트들을 쓰며(나의 경우 10000~10005를 사용했다.) Destination은 자신이 접속하고자 하는 내부 EC2의 Private IPv4(ex: 10.200.10.203)를 입력하고 ssh를 통하여 접속하는 것이기 때문에 22번 포트를 옆에 붙여야 한다. <br><br>(예시로 들면 Destination은 작성할 때 xx.xxx.xx.xxx:22가 된다.) <br>그리고 Add를 눌러 Forwarded ports:에 정상적으로 추가되었는지 확인해주자.<br><br>
(여기서 왜 현재 쓰지 않는 포트를 사용하는 이유가 궁금할 것이다..! <br>Source port에서 현재 로컬에서 쓰지 않는 포트를 사용해야 하는 이유는 해당 포트를 Local과 Bastion 사이에 보안을 위해 안전하게 연결을 하기 위해 대다수가 사용하지 않는 포트를 사용해야 하기 때문이다.<br>또한 Destination은 Bastion을 통해서 들어가고자 하는 내부 서버의 프라이빗 IP와 포트 번호를 설정해야 포트 포워딩이 제대로 설정이 된다.)<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림18.png)<br><br><br>
7. 다음으로 Connection에서 Data에 들어가 Login details 중 Auto-login username에서 ec2-user 를 입력한다.<br><br>(Amazon Linux 기준이며, Data에서 Auto-login username을 미리 설정해놓으면 서버에 접속 시, 맨 처음에 유저ID를 입력해야 하는 수고를 덜 수 있기때문에 나름 꿀팁이다?)<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림19.png)<br><br><br>
8. Session으로 이동해서 Host Name에는 현재 접속하고자 하는 Bastion EC2의 Host Name을 입력하고 ssh를 통한 접속이기 때문에 Port는 22번으로 한다.<br><br> 그리고 훗날 접속을 편하게 하기 위해서는 세션을 저장해야 하는데, Saved Sessions에 이름을 입력 후 옆에 Save를 클릭하면 된다.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림20.png)<br><br><br>
9. 8번 과정을 마쳤다면 Open을 클릭하면 아래 화면과 같이<br> Bastion Host로 지정한 EC2에 정상적인 접속이 되는 것을 확인할 수 있다!<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림21.png)<br><br><br>
10. 9번에서 열었던 Bastion-Host는 냅둔 채(종료 X) 내부에 배치된 EC2에 접속하기 위해 새로운 Putty프로그램을 실행한다.(꼭 Bastion-Host는 종료하지 말고 냅두고 새로운 Putty를 열어야 한다..! 만약 종료하면 내가 접속하고자 하는 세션에 접속이 안될 것이다!)<br><br> 그리고 SSH -> Auth에서 Authentication parameters에서 Allow agent forwarding을 체크한다.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림22.png)<br><br><br>
11. Connection -> Data에서 Login details -> Auto-login username에서는 ec2-user를 입력한다.<br>(Amazon Linux 기준이므로 우분투를 사용하거나 다른 OS 사용 시 username이 다를 수 있다는 것은 주의해주자👀)
![](/assets/2023-08/Bastion-Host/09_new/그림23.png)<br><br><br>
12. 내부 EC2로 접속하기 위해 Host Name에는 localhost를 입력하고 Port는 아까 Bastion Host EC2 설정 시 Tunnels에서 지정했던 Source port를 입력하면 된다.<br> 그리고 다음부터 접속을 편히 하기 위해 Saved Sessions 란에 이름을 지정해주고 Save를 클릭하여 세션과 정보를 저장해주자.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림24.png)<br><br><br>
13. 12번 화면에서 내부 EC2 세션 정보를 다 입력하고 저장했다면 Open 버튼을 누르면 아래와 같은 팝업창이 뜰 것이다.<br> 여기서 Accept를 누른다.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림25.png)<br><br><br>
14. 이렇게 Private Subnet 안에 있는 EC2에 들어갈 수 있게 되었다.<br><br>
![](/assets/2023-08/Bastion-Host/09_new/그림26.png)<br><br>



## 마치며
길다면 길고 짧다면 짧을 Bastion Host를 통해서 Private Subnet에 위치한 EC2에 접속하는 방법을 알아봤다..<br> (글 쓰느라 진짜 힘들어죽었다...)
<br><br>
아무튼 도움이 많이 되었으면 좋겠다!!

끝!
<br><br><br>
## 참고 및 출처

[Bastion Host의 이해와 AWS에서의 구성 (Proxy)](https://err-bzz.oopy.io/f5616e26-79ca-4167-b2eb-140de69b9b54)

[Bastion Host 란](https://dodomp0114.tistory.com/7)

[Bastion Host란 무엇인가?](https://skstp35.tistory.com/363)

[EC2 Bastion Host를 통해 Private Subnet EC2에 접속](https://dev.classmethod.jp/articles/access-private-subnet-ec2-via-ec2-bastion-host/)