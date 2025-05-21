---
title: PDU) PDU란
tags:
  - Network
  - Protocol
  - PDU
---
 
연관: [[패킷]], [[프레임 - Frame|프레임]], [[프로토콜]], [[OSI 7 Layer & TCP,IP 4 Layer|OSI 7계층]]

# PDU
> Protocol Data Unit -> 데이터 단위의 약속

[[네트워크]] 통신에서 각 계층의 데이터 단위(PDU)는 하위 계층으로 내려가면서 헤더를 더해가는데, 이를 [[캡슐화 - Encapsulation|인캡슐화]]라고한다.

![[PDU의 인캡슐화.png]]

---
## PDU에서의 헤더 개념
각 계층에서 데이터를 [[캡슐화 - Encapsulation|캡슐화]]할 때 상위 계층에서 받아온 PDU에 그 계층에서 사영하는 [[프로토콜]]의 헤더를 붙인다. 이 헤더는 PDU의 [[메타데이터]]를 담고 있어서 대량의 정보들 사이에서 필요한 정보를 쉽게 추출할 수 있도록 만든다.

#### 네트워크 계층(L3)의 헤더(IP)
[[OSI 7 Layer & TCP,IP 4 Layer|네트워크 계층(L3)]]에서 [[세션]]앞에 붙이는 메타데이터. [[IP - Internet Protocol|IP]]헤더 라고도 한다. **호스트간 주소 지정 및 [[Routing Protocol|라우팅]] 정보** 그리고 **[[패킷]]의 생존 시간**이 저장되어있다.

- 종류
	- [[IPv4 헤더]]
	- [[IPv6 헤더]]
