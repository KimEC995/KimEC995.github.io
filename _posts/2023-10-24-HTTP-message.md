---
title: HTTP 메시지
author: Kimec995
date: 2023-10-24 16:10:00 +09:00
categories: [Internet, Concept]
tags: [Basic, Network, HTTP]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/Internet_Network/Internet_Network_00.png
  alt: HTTP 메시지
---

[이어서](https://kimec995.github.io/posts/HTTP-Concept/)

## HTTP 메시지란
HTTP 메시지는 응답 혹은 요청시에 서버-클라이언트 간의 대화라고 생각하면 편하다. 응답과 요청 메시지의 구조는 다르다. [참고](https://datatracker.ietf.org/doc/html/rfc7230#section-3)

![image.png](\assets\img\postimg\Internet_Network\Internet_Network_09.png)

구조는
- 시작
- 헤더
- 공백(CRLF)
- 메시지

로 이루어져있다.

## 시작라인

매우 단순한 구조로 손쉬운 확장이 가능하다.

### 요청 메시지

```
GET /search?q=helloWorld&hl=ko HTTP/1.1
Host: www.google.com
```

```
method(ex.GET,POST..) SP(공백)request-target SP HTTP-version CRLF(엔터)
Header
```

#### HTTP method

<span style="color:black; background-color:#fff5b1;">**GET**</span> /search?q=helloWorld&hl=ko HTTP/1.1

종류: GET, POST, PUT, DELETE ...

서버가 수행할 동작을 지정.

[GET vs POST](https://kimec995.github.io/posts/GET-vs-POST/)

---

#### HTTP request-target(요청대상)

GET <span style="color:black; background-color:#fff5b1;">**/search?q=helloWorld&hl=ko**</span> HTTP/1.1

절대경로("/") 로 시작하는 경로

---

#### HTTP version

GET /search?q=helloWorld&hl=ko<span style="color:black; background-color:#fff5b1;">**HTTP/1.1**</span> 

HTTP의 버전 명시

---

#### HTTP Header

<span style="color:black; background-color:#fff5b1;">**Host: www.google.com**</span>

Header-field = field-name: SP field-value SP

field-name에는 대소문자 구분이 없다.

HTTP 전송에 필요한 모든 부가정보가 포함되어있다. ex) 메시지 바디 내용, 크기, 클라이언트 정보등등등. 표준 헤더가 너무 많고 필요시 임의 헤더 추가도 가능하다.

### 응답 메시지

```
HTTP/1.1 200 OK
Content-Type: text/html;charset=UTF-8
Content-Length: 3423

<html>
  <body>...</body>
</html>
```

```
HTTP-version SP status-code SP reason-phrase CRLF

MSG
```

---

#### HTTP version

<span style="color:black; background-color:#fff5b1;">**HTTP/1.1**</span>  200 OK

Content-Type: text/html;charset=UTF-8

Content-Length: 3423

HTTP의 버전 명시

---

#### HTTP status-code

HTTP/1.1 <span style="color:black; background-color:#fff5b1;">**200**</span> OK

Content-Type: text/html;charset=UTF-8

Content-Length: 3423

HTTP의 연결 상태

- 200: 성공
- 400: 클라이언트 요청 오류
- 500: 서버 내부 오류

---

#### HTTP reason-phrase

HTTP/1.1 200 <span style="color:black; background-color:#fff5b1;">**OK**</span>

Content-Type: text/html;charset=UTF-8

Content-Length: 3423

HTTP의 연결 상태(인간말)

---

#### HTTP Header

HTTP/1.1 200 OK

<span style="color:black; background-color:#fff5b1;">**Content-Type: text/html;charset=UTF-8**</span>

<span style="color:black; background-color:#fff5b1;">**Content-Length: 3423**</span>

Header-field = field-name: SP field-value SP

field-name에는 대소문자 구분이 없다.

HTTP 전송에 필요한 모든 부가정보가 포함되어있다. ex) 메시지 바디 내용, 크기, 클라이언트 정보등등등. 표준 헤더가 너무 많고 필요시 임의 헤더 추가도 가능하다.

## 네트워크 공부

[목록](https://kimec995.github.io/categories/internet/)

## 참고

[강의-모든개발자를 위한 HTTP](https://www.inflearn.com/course/http-%EC%9B%B9-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/dashboard)

![image.png](\assets\img\postimg\Internet_Network\Internet_Network_00.png)

[위키문서](https://ko.wikipedia.org/wiki/%ED%86%B5%ED%95%A9_%EC%9E%90%EC%9B%90_%EC%8B%9D%EB%B3%84%EC%9E%90)

[ietf](https://www.ietf.org/rfc/rfc3986.txt)