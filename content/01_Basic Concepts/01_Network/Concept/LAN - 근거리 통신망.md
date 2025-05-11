---
title: 네트워크) LAN - 근거리 통신망
tags:
  - Network
aliases:
  - LAN
  - 근거리 통신망
draft: false
---
연관: [[네트워크|네트워크]], [[WAN - 광역 통신망|WAN]]

# LAN
> Local Area Network -> 근거리 [[네트워크|네트워크]]

**LAN**은 일반적으로 **물리적이거나 논리적으로 한정된 네트워크 영역**을 말한다. 보통은 하나의 [[Router|라우터]] 하위에 있는 [[네트워크]]를 LAN으로 간주하지만, 반드시 라우터의 수만으로 LAN을 구분하진 않는다.

중요한 기준은 [[IP - Internet Protocol|IP]]주소와 [[서브넷 - Subnet#서브넷 마스크|서브넷 마스크]]이다. **같은 서브넷 마스크와 네트워크 주소를 공유하는 장비들은 동일한 [[브로드캐스트#브로드캐스트 도메인|브로드캐스트 도메인]]** 에 속하며, 이 범위를 하나의 LAN이라고 볼 수 있다. 따라서 네트워크의 구성 방식에 따라 라우터가 여러 대 있어도 동일한 서브넷 안에 있다면 하나의 LAN으로 간주할 수 있다.

**LAN**이 모여 [[네트워크#MAN|MAN]]이나 [[WAN - 광역 통신망|WAN]]이 된다.

![[whatisLAN.png]]

---
## LAN 프로토콜
LAN 프로토콜에는 Ethernet, Token Ring, FDDI 등이 있다.

#### 이더넷 / 패스트 이더넷
> [[이더넷 프로토콜 - Ethernet Protocol|Ethernet]] / FastEthernet

