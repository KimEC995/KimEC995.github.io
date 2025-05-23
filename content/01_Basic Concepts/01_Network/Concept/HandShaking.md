---
title: 네트워크) 핸드셰이킹
tags:
  - Network
  - GeneralKnowleage
aliases:
  - 핸드셰이킹
  - HandShaking
  - 3Way-Handshaking
  - 4Way-Handshaking
---
# HandShaking

본래 악수라는 뜻에 맞게 특정 채널이 정상적인 **통신이 시작하기 전**에 두 개의 실체간의 확립된 통신 과정을 정비하는 **자동화된 협상 과정**이다. 무슨 필살기같이 멋진 이름인데, 사실은 통신 전 준비단계이다. 보통 [[TCP]]의 데이터 교환 과정 전, 혹은 [[DHCP]]의 IP주소 교환 전에 이뤄진다.

교환 횟수에 따라 `3Way` 혹은 `4Way` 로 불린다. 물론 3, 4회로만 끝나야 하는건 아니고, [[TLS]]의 연결에선 암호화 데이터를 교환하는 과정에서 4회 이상 주고받기도 한다.

---
## 3Way Handshaking
![[3-Way-동작(8).png]]

사진은 [[TCP]]프로토콜 등에서 사용하는 3회의 간보기 후 서로 통신하는 방법. -> [[3Way-Handshake 의 동작 과정 - HTTP & HTTPS]]

안정성으로 따지면 4Way가 안정적이지만(마지막에 서버가 확답을 줌) 3Way를 사용하는 이유는 빠른 [[네트워크]]구성을 위해 하나의 통신이라도 줄인 것이다. 어떤 면에선 취약점인데, 속도의 장점도 있다.

---
## 4Way Handshaking
사용: [[DHCP#DHCP의 동작|DHCP의 동작]], [[3Way-Handshake 의 동작 과정 - HTTP & HTTPS#종료|TCP 연결 종료시]]

![[3-Way-동작(11).png]]

4회 교환하는 과정이다.