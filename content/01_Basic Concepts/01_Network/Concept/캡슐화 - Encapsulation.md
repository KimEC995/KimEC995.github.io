---
title: 네트워크) 캡슐화
tags:
  - Network
  - PDU
aliases:
  - 캡슐화
  - 디캡슐화
  - 인캡슐화
  - Encapsulation
  - Decapsulation
---
연관: [[PDU]], [[OSI 7 Layer & TCP,IP 4 Layer|OSI 7계층]]

# 캡슐화
> Encapsulation

[[네트워크]]에서 캡슐화는 데이터를 하위 계층의 프로토콜에 맞게 포장하는 과정 즉, **상위 [[PDU]]에 각 계층의 헤더를 더하는 과정**을 말함. 

![[PDU의 인캡슐화.png]]

|    계층     |     PDU 이름     |      인캡슐화 할 데이터      |
| :-------: | :------------: | :------------------: |
|   응용 계층   |   데이터 (Data)   |       사용자 데이터        |
|   전송 계층   | 세그먼트 (Segment) |    TCP/UDP 헤더 추가     |
|  네트워크 계층  |  패킷 (Packet)   |       IP 헤더 추가       |
| 데이터 링크 계층 |  프레임 (Frame)   |  MAC 헤더 + FCS 등 추가   |
|   물리 계층   |   비트 (Bits)    | 전기적 신호 or 광신호 등으로 변환 |
반대로 이걸 풀어내는걸 디캡슐화(Decapsulation) 라고 한다.
