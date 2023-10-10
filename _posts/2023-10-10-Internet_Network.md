---
title: Internet개념 - 인터넷 네트워크란
author: Kimec995
date: 2023-10-10 13:55:00 +09:00
categories: [Internet, Concept]
tags: [Basic, Network, HTTP]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/SQL_ALL/Internet_Network_IMG.png
  alt: Internet개념 - 인터넷 네트워크란
---

인터넷 네트워크는 전 세계적으로 연결된 컴퓨터 네트워크의 집합을 의미한다.

사실 21세기를 청년층으로 살면 인터넷을 모르는게 더 어렵겠지만, 인터넷을 정의할 수 있는 사람은 얼마나 될까.

이번엔 웹개발 공부를 위해 인터넷 네트워크에 대해 공부해보려 한다.

## 0. 서론_인터넷의 통신

만약 두 대의 PC가 서로 통신하려면 어떻게 해야할까?

바로 옆에 붙어있다면 케이블로 연결해 데이터를 주고 받을 수 있을 것이다.

그럼 두 PC가 엄청 멀리 떨어져있다면?

당연히 인터넷을 통해 통신할 수 있을 것이다.

인터넷은 어떻게 멀리 떨어진 두 PC, 그리고 수많은 컴퓨터를 어떤 규칙으로 어떻게 연결할까?

## 1. IP(Internet Protocol)

IP는 어디서 많이 들어 봤을 것이다.

Django를 할 떄도, Ubuntu를 설치할 때도, Jekyll을 활성화 할 때에도 항상 123.456.789.101 이런 형식의 숫자들이 보였다.

이는 컴퓨터 네트워크에서 데이터를 주고 받기 위한 표준 규약으로, 네트워크 상에서 컴퓨터나 기기를 식별하기 위해 사용하고 결론적으로 데이터가 올바른 목적지에 도달할 수 있게 만들어준다. 단위는 패킷(Packet)을 사용한다.

### IP Packet
![image.png](\assets\img\postimg\Internet_Network\Internet_Network_01.png)

[사진출처](https://ko.wikipedia.org/wiki/IPv4)

최하단의 `Data`를 제외하고 상단의 정보가 IP Packet이다. 내용은 읽으면 알겠지만, 보내는 IP 주소, 받는 IP 주소 등등 기타 정보들이다.

![image.png](\assets\img\postimg\Internet_Network\Internet_Network_02.png)

[사진출처](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)

노드들에서는 패킷을 이리저리 토스하며 받는 IP에 도달할 때 까지 반복한다.

하지만 너무 복잡한 인터넷에서 이 방법은 단점이 있다.

### IP 의 한계

- 비 연결성: 만약 패킷을 받는 대상이 없거나 끊겨도 전달한다. 왜냐하면 IP는 대상 서버의 상태를 알 수 없기 때문이다.

- 비 신뢰성: 패킷이 사라지거나, 순서가 꼬여 올 수 있다. 만약 해저 인터넷 케이블이 상어에게 먹혀서 끊기면 패킷은 사라진다. 또한 짧은 시간에 많은 패킷을 전달 할 경우 서로 다른 노드 루트를 타서 순서가 꼬일 수 있다.

- 프로그램 구분: 같은 IP를 사용하는 서버에서 통신하는 어플리케이션이 복수라면? 

서버-클라이언트가 계속 패킷을 주고 받으며 반복하다보면 이런 한계에 도달할 수 있다. 이를 보완하기 위해..

## TCP(Transmission Control Protocol, 전송 제어 프로토콜)

**인터넷 프로토콜 스택의 4 계층**

|계층|이름|
|:--:|:--:|
|어플리케이션 계층|HTTP, FTP|
|OS) 전송 계층|TCP, UDP|
|OS) 인터넷 계층|IP|
|네트워크 인터페이스 계층|랜드라이버 등|

이 있다.

순서는

![image.png](\assets\img\postimg\Internet_Network\Internet_Network_03.png)

1. 사용자가 어플리케이션에 데이터와 정보를 입력한다.
2. **어플리케이션 계층** 에서 HTTP를 통해 SOCKET 라이브러리로 전달
3. **전송 계층** TCP를 통해 정보와 메시지 데이터 생성 및 분할
4. **인터넷 계층** 에서 IP를 통해 패킷을 생성, TCP 데이터를 포함한다.

5. **네트워크 인터페이스 계층**에서 랜선 등을 통해 서버로 전달한다.

이 순서는 IP에서도 동일한데, TCP는 대체 무슨 역할인걸까.

![image.png](\assets\img\postimg\Internet_Network\Internet_Network_04.png)

TCP에서는 IP에선 부족했던 PORT 정보, 전송 제어, 순서 등의 정보를 추가한다. 이름 그대로 전송을 제어하고 IP의 한계를 극복했다.

### IP 의 한계: TCP 해결(요약) 

|특징|IP|TCP|
|:--:|:--:|:--:|
|**연결성**| 연결확인X | 가상 연결 확인 후 메시지 전송<br>(3Way Handshake) |
|**신뢰성**| 인식 불가 | 인식 가능<br>(응답0)|
|**구분**| IP 통합 | PORT 구분(TCP,UDP)|

마지막 PORT 구분은 TCP, UDP 모두 지원하는 기능이며, TCP의 80번과 UDP의 80번은 다른 포트이다. 이 포트를 지정하는건 **어플리케이션 계층** 에서 한다.

## UDP (User Datagram Protocol, 사용자 데이터그램 프로토콜)

사실 TCP와 비교했을 때 기능이 거의 없다.

단, PORT의 차이가 존재함.

기능이 없다면 안좋다고 생각할 수 있지만 오히려 그게 장점이다. 기능이 없기 때문에 어엄청 복잡한 TCP 튜닝보다 UDP 튜닝이 훨씬 빠르고 쉽다!


## 출처
[강의-모든개발자를 위한 HTTP](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)

![image.png](\assets\img\postimg\Internet_Network\Internet_Network_00.png)



그리고 수많은 구글링