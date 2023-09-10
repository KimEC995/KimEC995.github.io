---
title: GET 방식과 POST 방식의 차이와 구분
author: Kimec995
date: 2023-08-31 11:13:00 +09:00
categories: [HTML]
tags: [WebFrameWork, HTML, Method, GET, POST]
pin: true
math: true
mermaid: true
---

# GET 방식과 POST 방식의 차이와 구분

HTTP 요청 메서드 GET 과 POST 가 자주 눈에 띈다.

얼추 무엇을 하는지는 감으로만 알았지, 정확한 정의가 무엇인지 알아보기 위해 포스팅 한다.

>23.08.05 - 시작\
>23.08.10 - GET/POST 구분 01\
>23.08.31 - 이동


## 목차

1. [Http 에서 Request Methods 의 역할](#1-http-에서-request-methods-의-역할)
2. [Request Methods 의 종류](#2-request-methods-의-종류)
3. [GET vs POST](#3-get-vs-post)


## 시작하기 전에

자료 출처 = developer.mozilla.or\
그리고 구글링과 기타 서적자료 갈무리

*   [HTTP Documentation](https://developer.mozilla.org/en-US/docs/Web/HTTP "HTTP Documentation")
*   [HTTP - GET Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/GET "HTTP - GET Methods")
*   [HTTP - POST Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods/POST "HTTP - POST Methods")

---
---

## 1. HTTP 에서 Request Methods 의 역할

### HTML과 HTTP의 차이는?

시작하기 전 간단하게 HTML과 HTTP의 차이를 짚고 넘어가자.

- **HTML**(**H**yper**t**ext **M**arkup **L**anguage) : 웹 페이지 제작을 위한 마크업 언어. 구조와 컨텐츠를 정의하여, `사용자가 볼 수 있는 화면`

- **HTTP**(**H**yper**t**ext **T**ransfer **P**rotocol): 웹 페이지와 서버의 통신을 위한 프로토콜. 요청과 응답을 위주로 하며, `사용자가 볼 수 없는 화면`

정도이다.

웹 브라우저와 웹 서버 간의 동작을 포함해 이 둘을 요약하면

1. **웹 브라우저**에서 HTML을 렌더하여 화면이 표시된다.

2. 클릭이나, 텍스트 입력을 하면 HTML은 HTTP 생성을 유도한다.

3. **웹 브라우저**에서 이벤트를 이용해 HTTP를 생성한다.

4. HTTP 내부에는 데이터 요청 또는 응답이 들어있다.

5. **서버**로 HTTP가 전송된다.

6. **서버**에서 HTTP를 해석해 **웹 브라우저**로 전달한다.

7. **웹 브라우저**에서 HTTP를 해석해 HTML을 렌더한다. (다시 1로)

이렇게 된다.

---
---

### HTTP Request Methods 의 역할

HTTP Request Methods(HTTP 요청 방식) 은 HTML 내부에서 정의된다.

이 때 Methods 의 종류, ID, Name 등을 정의하고 HTML과 함께 웹 브라우저로 전송된다.

웹 브라우저에서 서버에 요청 / 응답을 보낼 때에 사용되는 역할로,  서버는 명령어의 종류에 따라 특정 응답방식을 취한다.

종류는 여러가지로, 우월한 방식은 없고 각 Methods를 적재적소에 사용하면 된다.

---
---

## 2. Request Methods 의 종류

### GET

**요약**

**GET** 메서드는 지정된 리소스 만을 요청한다. 요청(검색)하는 데에만 사용하며 데이터를 포함하지 않는다.

---

**사용.**

보통 URL 에 쿼리문으로 요청한다.

```python

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

---

**사용 예제**

- 기존의 DB에 존재하던 데이터 조회, 검색 등 보안신경X / 간단한 데이터 처리 등

---
---

### POST

**요약.**

`POST` 메서드는 주로 HTML 폼 요소를 통해 데이터가 전달된다.

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

* * *

 ### 그 외

종류가 좀 있다. 이후 포스트 작성 후 링크 걸 것

---
---

## 3. GET vs POST

GET 방식과 POST 방식은 아래와 같은 기준으로 나뉜다.

\[ 요청에 대한 처리 로직에서 `데이터 변경`이 이루어지는가? \]

즉, 데이터를 수정/ 생성 / 삭제 하는 과정의 여부(CRUD 중 R과 CUD)가 두 방식을 나누는 것이다.