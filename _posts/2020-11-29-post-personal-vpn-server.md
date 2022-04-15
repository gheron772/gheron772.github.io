---

title: "개인 VPN 서버 구축 가이드"
description: "본 가이드는 우회, 검열을 피하기 위해 Vultr, OpenVPN을 기반으로 자신만의 Ubuntu VPN 서버를 설정하는 방법을 설명합니다."
categories:
  - IT
tags:
  - 개인 VPN
  - 개인 VPN 서버
  - AWS VPN
  - Cloud VPN
  - VPS VPN
  - OpenVPN
  - Vultr
---

 본 가이드는 우회, 검열을 피하기 위해 Vultr, OpenVPN을 기반으로

자신만의 Ubuntu VPN 서버를 설정하는 방법을 설명합니다.



## 1. Vultr

 VPN 서비스를 구축하기 위해서 서버로 사용할 리눅스 머신이 필요합니다.

본가이드에서는 VPS 서비스인 Vultr을 사용합니다.



<a href="https://www.vultr.com/?ref=8955219-8H"><img src="https://www.vultr.com/media/logo_ondark.png" width="349" height="84"></a>

위 배너를 통해 가입시 가입자에게 100$ 글쓴이에게 35$ 크레딧이 지급됩니다.

<br/>

가입전 [SpeedTest](https://www.vultr.com/resources/faq/?ref=8955219-8H#downloadspeedtests){:target="_blank"} 페이지를 통해 사용하려는 리전의 속도를 테스트 해 볼 수 있습니다.

![SpeedTest](/assets/images/personal-vpn-server/SpeedTest.png)

<br/>

가입을 위해서 메인페이지 우측 상단의 Sign up을 눌러 가입을 진행 합니다.

![Sign-up](/assets/images/personal-vpn-server/Sign-up.png)

<br/>

이메일 인증 후 로그인 하게되면 서버 생성을 위해 결제가 필요합니다.

배너를 통해 진행하였다면 10$ + 100$ 크레딧으로 22개월 사용이 가능합니다. (110$ / 5$ = 22)

![billing](/assets/images/personal-vpn-server/billing.png)

<br/>

## 2. 서버 설치

서버생성을 위해서 좌측의 Product탭 이동 후 + 버튼을 클릭 합니다.

![add-server](/assets/images/personal-vpn-server/add-server.png)

<br/>

Cloud computer와 Tokyo 지역을 선택 후 넘어갑니다.

![add-server2](/assets/images/personal-vpn-server/add-server2.png)

<br/>

Ubuntu 20.04 x64와 5$ 플랜을 선택 후 하단의 Deploy Now를 눌러 서버 생성을 완료 합니다.

![add-server3](/assets/images/personal-vpn-server/add-server3.png)

<br/>

## 3. VPN 서비스 설치

 서버 초기 계정은 root 패스워드는 Overview의 좌측 하단 눈모양 버튼을 누르면 확인 할 수 있습니다.

우측 상단의 모니터 모양 버튼을 누르면 서버에 명령을 내리는 터미널을 사용할 수 있습니다.

![server-setting](/assets/images/personal-vpn-server/server-setting.png)

<br/>

**주의! : root계정 패스워드를 단순한 패스워드로 변경하거나**

**외부에 노출시킬경우 공격대상이 될 수 있습니다.**

![terminal](/assets/images/personal-vpn-server/terminal.png)

<br/>

root계정으로 로그인한 후에 아래 명령어를 실행하여 OpenVPN 설치 스크립트를 실행합니다.

진행 과정은 Enter만 입력하여 모두 기본값으로 설정하면 됩니다.

`wget git.io/vpn --no-check-certificate -O openvpn-install.sh; bash openvpn-install.sh`

![install-script](/assets/images/personal-vpn-server/install-script.png)

<br/>

스크립트의 수행 결과로 /root/client.ovpn 파일이 생성 되었습니다.

client.ovpn 파일은 OpenVPN연결시 Key로 사용됩니다. (외부에 노출되지 않도록 주의!)

![install-res](/assets/images/personal-vpn-server/install-res.png)

<br/>

해당 파일을 PC로 가져오기 위해 sftp 클라이언트인 Filezilla를 사용합니다. [Filezilla Down](https://filezilla-project.org/download.php?type=client){:target="_blank"}

내 서버의 IP주소는 Vultur의 Overview 페이지에서 확인 가능합니다.

서버연결시 입력해야 하는 정보는 아래와 같습니다.



Host - sftp://서버 IP주소

Username - root

password - 본인 서버의 비밀번호



![filezilla](/assets/images/personal-vpn-server/filezilla.png)

<br/>

VPN 연결을 위한 Client를 받습니다. [OpenVPN](https://openvpn.net/community-downloads-2/){:target="_blank"}
![down-vpn](/assets/images/personal-vpn-server/down-vpn.png)

<br/>

설치가 완료되면 트레이 아이콘중 모니터 모양을 찾아 우클릭하여 파일 불러오기를 선택 합니다.

![load](/assets/images/personal-vpn-server/load.jpg)

서버에서 받은 client.ovpn파일을 열어줍니다.

![open-ovpn](/assets/images/personal-vpn-server/open-ovpn.png)

<br/>

파일을 열고난 후 다시 트레이 아이콘을 우클릭 하면 `연결`버튼이 활성화 됩니다. 클릭해 줍니다.

![connect](/assets/images/personal-vpn-server/connect.jpg)

<br/>

## 3. 적용여부 확인

검색엔진에서 내 IP를 조회하면 VPN을 통한 접속여부를 확인 할 수 있습니다.

내 서버의 IP로 표시될 경우 VPN을 통한 접속 상태임을 알 수 있습니다.

![ip](/assets/images/personal-vpn-server/ip.png)

<br/>

## 4. 모바일

모바일에서도 Open VPN연결이 가능하며

핸드폰으로 client.ovpn파일을 옮겨 아래 앱에서 불러와 사용하면 됩니다.

Android : [OpenVPN Connect](https://play.google.com/store/apps/details?id=net.openvpn.openvpn&hl=ko&gl=US){:target="_blank"}

IOS : [OpenVPN Connect](https://apps.apple.com/us/app/openvpn-connect/id590379981){:target="_blank"}
