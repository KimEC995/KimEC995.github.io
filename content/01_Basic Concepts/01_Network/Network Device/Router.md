---
title: 네트워크 장비) Router
tags:
  - Network
  - NetworkDevice
  - Router
aliases:
  - 라우터
  - Router
---
연관: [[OSI 7 Layer & TCP,IP 4 Layer#네트워크 계층|네트워크 계층(L3)]]

# Router

**서로 다른 [[LAN - 근거리 통신망|LAN]]을 연결하고 [[브로드캐스트#브로드캐스트 도메인|브로드캐스트 도메인]]을 구분하는 [[OSI 7 Layer & TCP,IP 4 Layer#네트워크 계층|네트워크 계층(3층)]] 장비**.
물론 가끔은 동일한 LAN을 연결할 때도 있다. 

하는 일은
- 네트워크 경로 설정([[Routing Protocol|라우팅 프로토콜]])
- 경로 관리([[라우팅 테이블|Routing Table]])
- 패킷 전송([[라우터의 사전적 의미와 그 구분|Routing]])
- 네트워크 보안([[VPN]], [[NAT]])
- QoS
- 등등

**라우터는 [[OSI 7 Layer & TCP,IP 4 Layer|L3 계층]] 장비이기 때문에 [[IP - Internet Protocol#비신뢰성|IP의 비신뢰성 특징]]을 지닌다**. 이말은 최적 경로로 데이터 [[패킷|패킷]]을 전송하고 그 후에는 응답에 신경쓰지 않는다. 따라서 잘 도착 했는지, 못 도착 했는지를 구분하지 않는다.

라우터의 사전적 의미는 [[라우터의 사전적 의미와 그 구분]]을 보라.

---
## 라우팅 프로토콜
> Routing Protocol -> [[Routing Protocol|라우팅 프로토콜]]

라우터간의 정보를 교환하고 [[라우터의 최적 경로|최적 경로]]를 찾아 설정, 전송하는 **[[패킷]]을 전송하는 과정**을 말함.

---
## 라우팅 테이블
> Routing Table -> [[라우팅 테이블]]

[[Routing Protocol|라우팅 프로토콜]]에서 선출한 [[라우터의 최적 경로|최적 경로]]들을 모아두는 테이블.

