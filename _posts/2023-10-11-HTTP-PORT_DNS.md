---
title: Internet개념 - PORT, DNS
author: Kimec995
date: 2023-10-11 12:40:00 +09:00
categories: [Internet, Concept]
tags: [Basic, Network, HTTP]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/Internet_Network/Internet_Network_00.png
  alt: Internet개념 - 인터넷 네트워크란
---

[이전포스트](https://kimec995.github.io/posts/Internet_Network/)에서 TCP와 UDT는 PORT를 사용해 IP의 한계를 보완한다 했다. PORT란 뭘까?

## PORT
만약 내가 하나의 PC로 카톡도 하고, 줌 연결도 하고, 유튜브도 보면 TCP/IP는 통신을 어떻게 구분할까? 카톡 채팅이 줌 채팅으로 안가고, 유튜브의 내용이 줌으로 전송되지 않으려면 각 어플리케이션마다 고유한 주소가 있어야 하는데, 이를 PORT 라고 한다.

PORT 번호는 `http://127.0.0.1:8000/` 에서 `:` 뒤에 붙는다. 번호는 0번부터 65535번까지 할당할 수 있다.

- 0 ~ 65535 : 포트 번호
- 0 ~ 1023 : 알려진 번호
- 80 : HTTP 통신
- 443 : HTTPS 통신

## DNS(Domain Name System)
도메인은 IP가 변경되어도 고유 주소를 찾을 수 있는 정보이다.

극단적으로 만약 내가 [구글](www.google.com)(`www.google.com`)에 접속할 때 IP로 접속한다면 어제는 `199.199.199.199`인데 오늘은 `345.345.345.345` 일 수도 있다. 귀찮은건 둘째치고 바뀐 IP를 어떻게 알아야 하는가. 이런 한계를 극복하기 위해 도메인 이름을 관리하는 시스템을 만들었다.

IPTime 같은 공유기 회사에서는 자사의 도메인을 기반으로 DDNS(Dynamic Domain Name System) 을 지원하기도 한다.

## 네트워크 공부

[목록](https://kimec995.github.io/categories/internet/)

## 참고

[강의-모든개발자를 위한 HTTP](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)

![image.png](\assets\img\postimg\Internet_Network\Internet_Network_00.png)

[위키문서](https://ko.wikipedia.org/wiki/%ED%86%B5%ED%95%A9_%EC%9E%90%EC%9B%90_%EC%8B%9D%EB%B3%84%EC%9E%90)

[ietf](https://www.ietf.org/rfc/rfc3986.txt)