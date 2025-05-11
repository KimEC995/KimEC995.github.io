---
title: 프로토콜) 이더넷(Ethernet) 프로토콜
tags:
  - Network
  - Protocol
aliases:
  - 이더넷
  - Ethernet
---
연관: [[LAN - 근거리 통신망#LAN 프로토콜|LAN 프로토콜]], [[OSI 7 Layer & TCP,IP 4 Layer|데이터 링크 계층(2층)]]

# 이더넷
> Ethernet

[[00_ What Is Internet|인터넷]]과 매우 유사한 이름이지만 둘은 엄밀히 다른 개념이다. 현재까지도 [[네트워크|네트워크]] 환경에서 절대 다수를 차지하는 프로토콜로 유선 인터넷을 사용한다면 이더넷을 쓰는 환경이라고 생각하면 된다.

주로 [[LAN - 근거리 통신망|LAN]]에서 사용되지만, 기술의 발전으로 [[WAN - 광역 통신망|WAN]]에서도 확장 되어 사용하기도 한다. 아래는 LAN 기준으로 작성한다.

[[OSI 7 Layer & TCP,IP 4 Layer#데이터 링크 계층|데이터 링크 계층(2층)]] 뿐만 아니라 [[OSI 7 Layer & TCP,IP 4 Layer#물리 계층|물리 계층(1층)]]도 포함하는 기술 중 하나로, 데이터를 [[프레임 - Frame|프레임]] 단위로 전송하는 [[프로토콜]]이다.
1층 에서는 실제 전기 신호, 광신호, 케이블, 커넥터, 전송 속도 등을 다루고,
2층에서는 프레임의 형식, MAC 주소, 에러 검출, 흐름 제어 등을 다룬다.

주로 [[UTP Cable]]을 이용해 연결한다.

---
## 이더넷의 종류
이더넷은 1977년 개발 이후 지금까지도 [[TCP]]/[[IP - Internet Protocol|IP]] 그리고 [[00_HTTP|HTTP]]와 결합 되어 건재하다.

#### 이더넷
> Ethernet

#### 패스트 이더넷
> Fast Ethernet

- 속도: 100Mbps.
- 표준명: IEEE 802.3u
- 케이블: [[UTP Cable|UTP]] 2쌍 Cat5

[[00_CISCO Packet Tracer|CISCO Packet Tracer]]에서 기본적으로 사용하는 인터페이스. 물론 패킷 트레이서는 저가형 장비가 대부분이라 패스트 이더넷을 기준으로 제공한다.

과거에는 많이 쓰였지만, 요즘엔 [[이더넷 프로토콜 - Ethernet Protocol#기가비트 이더넷|기가비트 이더넷]]에 밀려 잘 안쓰인다. 굳이 쓴다면 프린터용 LAN 포트 정도..?

#### 기가비트 이더넷
> Gigabit Ethernet

- 속도: 1,000Mbps = 1Gbps
- 표준명: IEEE 802.3z (광)
- 케이블: [[UTP Cable|UTP]] 4쌍 Cat5e

요즘 대세.. 라고는 하지만 좀 뒤에 나온 [[이더넷 프로토콜 - Ethernet Protocol#10기가비트 이더넷|10기가비트 이더넷]]에 밀려 저가형 공유기나 [[HUB]]등에서 사용한다.

#### 10기가비트 이더넷
> 10Gigabit Ethernet

#### 테라비트 이더넷
> Terabit Ethernet

2017년에 표준 제정된 그나마 따끈따끈한 이더넷