---
title: HTTP의 특징
author: Kimec995
date: 2023-10-20 15:10:00 +09:00
categories: [Internet, Concept]
tags: [Basic, Network, HTTP]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/Internet_Network_IMG.png
  alt: HTTP의 특징
---

[이어서](https://kimec995.github.io/posts/HTTP-URI-%ED%9D%90%EB%A6%84/)

**HTTP**(**H**yper**t**ext **T**ransfer **P**rotocol): 웹 페이지와 서버의 통신을 위한 프로토콜의 한 종류. 요청과 응답을 위주로 한다. 지금은 인터넷의 거의 모든 것을 HTTP 메시지에 담아 주고 받는다. HTML, IMG, 음성, API, 서버 간 데이터 이동 등등등. 서버도 TCP으로 직접 연결하는 경우는 거의 없다.

## 1. HTTP의 특징

HTTP가 거의 모든 곳에서 사용될 수 있는 이유는 뭘까? 그 특징을 알아보자.

### 1. 간단함.
HTTP는 간단하고 사용하기 쉬운 프로토콜이다. 요청과 응답이 텍스트 기반으로 이루어져 어떤 프로그램이건 읽고 이해하기 쉽다.

### 2. 무상태(Stateless).
HTTP는 상태를 유지하지 않는다. 무슨 말이냐 하면 같은 서버-클라이언트 여도 어제 보낸 HTTP메시지와 오늘 보낸 HTTP 메시지는 완전히 다르다. 각각의 요청은 별개로 처리되며 서버는 클라이언트의 상태를 기억하지 않는다. 이로 인해 세션 상태를 유지하는 특별한 메커니즘이 필요한데, 이를 쿠키(Cookies) 또는 세션(Session)이라고 부른다.

### 3. 연결상태(Connectionless).
위의 내용과 같이 HTTP는 기본적으로 연결을 유지하지 않는다. 클라이언트가 서버에 요청을 보내면 서버는 응답을 보내고 연결을 끊는다. 이런 특성으로 특정 상황에서 각 요청마다 새로운 연결을 만들어야 할 때 오버헤드가 발생할 수도 있다. 그래서 HTTP/1.1 버전 에선 연결을 유지하기도 한다. 

예를 들어 금일 18시에 배민에서 치킨 무료 선착순 1000명 이벤트를 하고, 서버가 감당할 수 있는 사람이 1000명이고 2000명이 몰린다 가정 했을 때 개발자는 서버는 분명히 부하에 걸릴테니 앞단에 정적 페이지를 만들어 연결 상태를 끊거나 기타 방법으로 연결 상태를 컨트롤 할 수 있게 되는 것이다.

> **추가**

> 그렇다면 왜 PC 부팅 이후 최초로 구글에 신호를 요청/응답 받는 시간과 두 번쨰로 요청/응답 받는 시간에 차이가 생길까. 그 이유는 TCP의 3Way HandShake에서 찾을 수 있다. 최초로 클라이언트와 서버가 가상 연결이 된 이후 최초로 요청/응답을 받았다고 그 연결이 아예 끊기는 것은 아니다. 특정시간(서버에서 컨트롤)이 지나거나 PC를 종료하지 않는 이상 가상연결은 유지되기 때문에 그 시간이 차이가 난다.

### 4. 무결성(Integrity).
HTTP는 데이터 무결성을 제공하지 않는다. 무슨 말이냐 하면 내가 `HELLO WORLD`를 전송 했는데, `WORLD HELLO` 로 받는 등 데이터가 도중에 바뀔 수도 있다는 말이다. 이를 해결하기 위해 TCP 나 보안적인 면에서 HTTPS를 사용하기도 한다.

### 5. 텍스트 기반.
HTTP 메시지는 텍스트 기반이기 때문에 사람도 읽을 수 있다. 이는 디버깅 / 네트워크 트래픽 분석 등에 도움이 된다.

### 6. 요청 메서드.
HTTP 요청은 여러가지 메서드(ex.[GET,POST](https://kimec995.github.io/posts/GET-vs-POST/) 등)를 사용할 수 있다. 각 메서드는 서버에게 요청의 목적을 알린다.

### 7. 상태 코드.
서버는 요청에 대한 결과로 상태 코드를 반환한다.(ex.404, 200 등) 상태 코드는 요청이 성공적으로 처리 되었는지, 오류가 발생 하였는지 등을 나타낸다.

### 8. 헤더(Header).
HTTP 요청과 응답 메시지에는 헤더가 포함된다. 이는 추가 정보를 제공하며 서버-클라이언트 간의 데이터 전송 방식을 정의한다.

### 9. URL(Uniform Resource Locator).
HTTP 요청은 URL을 사용해 웹 리소스(ex.영상, 이미지, 소리 등)을 지정한다.

### 10. 확장 가능성.
HTTP는 확장 가능한 프로토콜이다. 새로운 기능이나 확장이 필요하다면 새로이 정의해 사용할 수 있다.

## HTTP의 종류

#### HTTP/0.9
초기버전. 매우 간단한 프로토콜로 현재의 HTTP와는 많이 다르다. 메시지는 문자열 형태로 전송되며 헤더나 다른 메타 데이터를 포함하지는 않는다. GET 메서드만 지원했으며 무상태를 지향했다.

#### HTTP/1.0
HTTP/0.9에서 헤더를 추가해 여러 정보를 포함시켰으며 요청 메서드가 확장되었다(POST 등 추가). 또한 상태코드 기능이 도입되어 상태를 확인할 수 있게 되었다.

#### HTTP/1.1
현재 우리가 가장 많이 사용하는 버전. 이후 2나 3도 나왔지만 성능 개선에 가까워 HTTP는 거의 1.1을 의미한다.

- 지속적인 연결: 한번의 TCP연결을 통해 여러 개의 요청과 응답을 주고 받을 수 있다. 따라서 네트워크의 성능이 개선되었다.

- 파이프라인화: 여러 요청을 병렬로 전송, 응답을 비동기적으로 받아 성능을 향상시켰다.

- **호스트 헤더 필드**: 여러 도메인이 하나의 웹 서버에 호스팅 될 수 있게 되었다. 예를 들어 특정 서버의 IP가 `1.1.1.1` 일 때 호스트 헤더가 없는 경우

```sql
GET / HTTP/1.0
```

이렇게만 보내진다. 이 때 어떤 도메인을 요청하는지 서버는 알 수 없다.

하지만 호스트 헤더가 있는 경우 

```sql
GET / HTTP/1.1
Host: example000.com
```

혹은 동일 서버의 다른 도메인의 경우

```sql
GET / HTTP/1.1
Host: example001.com
```

로 도메인 식별이 가능해진다. 이로써 요청에 맞는 응답을 보낼 수 있게된다.

- 캐시 관리: 이전에도 있었지만 더 강력한 캐시 관리 기능을 도입해 중복된 요청을 줄이고 최적화한다. 이로써 네트워크 대역 폭을 절약하고 빠른 페이지 로딩을 지원하게 되었다.

- 인증과 보안: 다양한 인증 메커니즘과 보안 기능을 도입해 안전한 통신을 지원한다.

#### HTTP/2
HTTP/1.1의 다음 주 버전으로 성능 향상에 초점이 맞춰져 개발되었다. 이전 버전과 비교해 빠른 페이지 로딩과 최적화된 연결 관리를 제공한다.

#### HTTP/3
HTTP의 최신버전 중 하나. 성능과 보안에 초점이 맞춰져 개발되었다. TCP 대신 QUIC(Quick UDP Internet Connections) 를 기반으로 더 빠르고 안전한 데이터 전송이 가능해졌다.

## HTTP 공부 시리즈

[Internet개념 - 인터넷 네트워크란](https://kimec995.github.io/posts/Internet_Network/)

[Internet개념 - PORT, DNS](https://kimec995.github.io/posts/HTTP-PORT_DNS/)

[URI와 웹 브라우저의 흐름](https://kimec995.github.io/posts/HTTP-URI-%ED%9D%90%EB%A6%84/)

[HTTP의 특징](https://kimec995.github.io/posts/HTTP-Concept/)

[HTTP 메시지](https://kimec995.github.io/posts/HTTP-message/)

## 참고

[강의-모든개발자를 위한 HTTP](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)

![image.png](\assets\img\postimg\Internet_Network\Internet_Network_00.png)

[위키문서](https://ko.wikipedia.org/wiki/%ED%86%B5%ED%95%A9_%EC%9E%90%EC%9B%90_%EC%8B%9D%EB%B3%84%EC%9E%90)

[ietf](https://www.ietf.org/rfc/rfc3986.txt)