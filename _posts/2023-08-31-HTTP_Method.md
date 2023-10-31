---
title: HTTP Methods(Get,Post)
author: Kimec995
date: 2023-08-31 11:13:00 +09:00
categories: [Internet, Concept]
tags: [Basic, Network, HTTP, Method]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/HTTP_IMG.png
  alt: HTTP Method(Get,Post)
---

HTTP 요청 메서드 GET 과 POST 가 자주 눈에 띈다.

HTTP 메서드엔 어떤 종류가 있고 어떤 일을 할까.

>23.08.05 - 시작\
>23.08.10 - GET/POST 구분 01\
>23.08.31 - 이동\
>23.10.24 - 수정 및 추가\
>23.10.30 - 내용 보강, 수정

## 1. HTTP 에서 Request Methods 의 역할

[HTTP의 특징](https://kimec995.github.io/posts/HTTP-Concept/)

[HTTP 메시지](https://kimec995.github.io/posts/HTTP-message/)

HTTP Request Methods(HTTP 요청 방식) 은 HTML 내부에서 정의된다.

이 때 Methods 의 종류, ID, Name 등을 정의하고 HTML과 함께 웹 브라우저로 전송된다.

웹 브라우저에서 서버에 요청 / 응답을 보낼 때에 사용되는 역할로,  서버는 명령어의 종류에 따라 특정 응답방식을 취한다.


## 2. Request Methods 의 종류

### GET

**요약**

**GET** 메서드는 지정된 리소스 만을 요청한다. 요청(검색)하는 데에만 사용하며 데이터를 포함하지 않는다.

---

**사용.**

보통 URL 에 쿼리문으로 요청한다.

```
https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=0&ie=utf8&query=GET+Methods
```

네이버 검색창에 "GET Methods" 를 검색 한 결과이다.

맨 뒷부분의 'query' 라는 매개변수 안에 'GET+Methods' 를 넣어 GET 요청을 넣은 모습이다.

`누구나 볼 수 있다.`

---

**특징.**

보이는 바와 같이 URL 에 요청을 직접 넣기 때문에 `보안이 취약`하고, 로그가 남는다.

또한 처리 속도 등 문제로 인해 제한을 해놨을 가능성이 매우 높아 `길이의 제한이 있다.`

Cachable 특성을 가지고 있어 [캐시 생성](https://developer.mozilla.org/en-US/docs/Glossary/Cacheable "캐시생성")이 가능하고, 이를 이용해 일반적으로 다른 Methods 에 비해 `전송 속도가 빠르다.`

[Idempotent(멱등 == 여러번 반복해도 결과가 같다)](https://developer.mozilla.org/en-US/docs/Glossary/Idempotent) 특성을 가지고 있어 읽기 전용으로 사용하면 `언제나 결과가 같다.`

즉, 새로고침 등 재전송 실행해도 언제나 결과가 같다.

`절대경로`

---

**사용 예제**

- 기존의 DB에 존재하던 데이터 조회, 검색 등 보안신경X / 간단한 데이터 처리 등


### POST

**요약.**

`POST` 메서드는 주로 HTML 폼 요소를 통해 데이터가 전달된다. 요청 내역을 처리한다.

---

**사용.**

사용자가 폼 내부에 데이터를 작성해 HTTP 내부 Body 에 담겨 전달된다.

내부에 담기기 때문에 `사용자가 확인할 수 없다.`

---

**특징.**

전송되는 데이터가 보이지 않게 처리되기 때문에 `보안이 뛰어나다.`

HTTPS 와 함께 사용하면 보안이 더욱 강화된다.

HTTP 내부 Body 와 함께 전송되기 때문에 `길이의 제한이 없다.` 많은 양의 데이터 혹은 복잡한 데이터 전달에 유리하다.

요청이 캐싱되지 않는다. 따라서 요청 발생시 `매번 새로운 요청을 응답해야한다.(상대적 느림)`

---

**사용 예제**

- 기존의 DB에 존재하던 데이터 수정, 추가, 삭제 등 복잡/ 민감/ 길이가 긴 데이터 처리.



 ### 그 외

종류가 좀 있다. 이후 포스트 작성 후 링크 걸 것


## 3. GET vs POST

GET 방식과 POST 방식은 아래와 같은 기준으로 나뉜다.

[ 요청에 대한 처리 로직에서 `데이터 변경`이 이루어지는가? ]

즉, 데이터를 수정/ 생성 / 삭제 하는 과정의 여부(CRUD 중 R과 CUD)가 두 방식을 나누는 것이다.

## 네트워크 공부

[목록](https://kimec995.github.io/categories/internet/)

## 자료출처

[자료 출처](developer.mozilla.or)
그리고 구글링과 기타 서적자료 갈무리

[HTTP Documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP "HTTP Documentation")

[HTTP - GET Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET "HTTP - GET Methods")

[HTTP - POST Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST "HTTP - POST Methods")