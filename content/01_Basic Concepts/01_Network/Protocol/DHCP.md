---
title: 프로토콜) IP자동 할당 프로토콜 - DHCP
tags:
  - Protocol
  - Network
---
 
연관: [[OSI 7 Layer & TCP,IP 4 Layer#응용 계층|응용 계층(7층)]], [[IP - Internet Protocol|IP 주소]]

# DHCP
> Dynamic Host Configuration Protocol

**동적으로 [[IP - Internet Protocol|IP]]를 할당해주는 [[UDP]]를 이용하는 프로토콜.** 
[[OSI 7 Layer & TCP,IP 4 Layer#응용 계층|응용 계층(7층)]]에서 사용한다.

UDP 프로토콜 중 67번, 68번 포트를 사용한다.  67번은 서버용, 68은 클라이언트 포트용으로 구분된다.

주로 전문 서버 장비들이 하지만, [[00_CISCO Packet Tracer|CISCO 장비]]는 [[Router|라우터]]에서도 이 프로토콜을 사용할 수 있다.

PC들은 부팅 되고 나면 [[OSI 7 Layer & TCP,IP 4 Layer|TCP/IP 네트워크]]에 참여하기 위해 자신이 사용해야 할 IP 주소 정보([[IP - Internet Protocol#IP의 주소|IP 주소]], [[서브넷 - Subnet|Subnet Mask]], 기본 [[게이트웨이]], [[DNS|DNS 서버]])를 찾는다. PC나 Host가 지정된 IP 주소를 가지는게 아니라, PC가 부팅되면 **DHCP Server**에게서 Dynamic 한 방식(자동)으로 IP 주소를 가져오는 방식. 이를 **동적 IP 주소 할당**이라고 한다.

결국 IP 주소 배정을 자동으로 해주고, 관리의 편함을 제공하는 프로토콜.

DHCP는 임대(Lease)의 서비스 이기 때문에 사용 기간 설정도 가능하다.

---
## DHCP의 동작
DHCP는 4단계 과정을 거쳐 클라이언트에게 IP 주소를 할당하는데, 이를 [[HandShaking#4Way- Hand Shaking|4 Way Hand Shaking]]이라고 한다.

![[DHCP(1).png]]

#### 1. DHCP Discover(확인)
클라이언트는 서버에게 `Discover` 메시지를 전송하며 **IP 임대 요청을 시작한다.**

이때 [[브로드캐스트#브로드캐스팅|브로드캐스팅]]으로 메시지를 전송해 주변의 모든 DHCP 서버를 확인한다.
그리고 메시지를 받은 서버는 IP 주소 중복을 방지하기 위해 [[ICMP|ICMP Echo]]를 전송할 수 있다.

![[DHCP(2).png]]
#### 2. DHCP Offer(응답)
`Discover`메시지를 수신한 DHCP 서버는 이후 `Offer` 메시지를 전송하며 **IP 임대 응답** 메시지를 보낸다.

![[DHCP(3).png]]

#### 3. DHCP Request(최종 요청)
`Offer`메시지를 받은 클라이언트는 마지막으로 `Request`요청을 브로드캐스팅으로 전송한다. 이 때 메시지 내용 중 어떤 서버를 선택했는지 표기함으로써 모든 DHCP 서버가 알 수 있도록 한다.

![[DHCP(4).png]]

#### 4. DHCP Ack
최종적으로 서버는 클라이언트에게 `Ack` 메시지를 전송해 **IP를 임대한다**.

![[DHCP(5).png]]