---
title: 네트워크 장비 -  Router
tags:
  - Network
  - NetworkDevice
aliases:
  - 라우터
  - Router
---
연관: [[OSI 7 Layer & TCP,IP 4 Layer#네트워크 계층|네트워크 계층(3층)]]

# Router

**서로 다른 [[LAN - 근거리 통신망|LAN]]을 연결하고 [[IP브로드캐스트 - Broadcast#브로드캐스트 도메인(범위)|Broadcast Domain]]을 구분하는 [[OSI 7 Layer & TCP,IP 4 Layer#네트워크 계층|네트워크 계층(3층)]] 장비**.

하는 일은
- 네트워크 경로 설정([[00_라우팅 프로토콜|Routing Protocol]])
- 경로 관리([[00_라우터 - Router#라우팅 테이블|Routing Table]])
- 패킷 전송([[00_Switch#Switching|Switching]])
- 네트워크 보안([[VPN]], [[NAT]])
- QoS
- 등등

최적 경로로 데이터 [[Packet|패킷]]을 전송하고 그 후에는 응답에 신경쓰지 않는다. 따라서 잘 도착 했는지, 못 도착 했는지를 구분하지 않는다.