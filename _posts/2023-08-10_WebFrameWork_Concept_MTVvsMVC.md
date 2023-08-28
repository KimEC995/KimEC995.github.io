---
title: MTV vs MVC
author: Kimec995
date: 2023-08-10 08:38:00 +09:00
categories: [WebFrameWork/Concept]
tags: [WebFrameWork, Concept, MTV, MVC, Pattern]
pin: true
math: true
mermaid: true
---

# MTV / MVC Pattern

웹 개발을 처음 배우면 MTV 혹은 MVC 라는 말이 참 많이 나온다.

이게 뭐길래 자꾸만 말이 나오는걸까.

많고 많은 디자인 패턴들 중 많이 쓰이는 MTV /MVC 패턴에 대해 알아보고자 한다.

---

## 1. MTV / MVC 란?

### 1-1. 웹 개발 디자인 패턴

이 세상엔 참 많은 웹 개발 디자인 패턴이 존재한다.

애초에 [웹 개발 디자인 패턴] 이란건 또 뭘까.

**위키 정의 - 디자인 패턴**

프로그램 개발에서 자주 나타나는 과제를 해결하기 위한 방법 중 하나.

과거 소프트웨어 개발 과정에서 발견된 노하우를 축적, 명명하여 이후 활용하기 좋은 형태로 묶어서 정리한 것.

한글로 소프트웨어 개발 방법론 이라고도 한다.

MTV / MVC 는 웹 개발 디자인 패턴 중 하나(혹은 두개) 이다.

즉, 이 패턴들은 우리를 괴롭히는게 아닌 오히려 돕는 정리된 결과물 이라는 것이다.

### 1-2. 그래서 MTV / MVC 가 뭔데.

위 내용을 보면 MTV/MVC 는 어느 날 갑자기 하늘에서 똑 떨어진 것이 아닌, 많은 선배 개발자님들의 시행착오 끝에 찾아낸 최적의 아키텍쳐라는 결론이 나온다.

이 방법은 [사용자에게 보이는 화면] 과 [내부적으로 계산되는 로직] 으로 나누어져 서로에게 영향을 끼치지 않도록 시스템을 개발하고 유지보수를 편리하게 할 수 있다는 장점이 있다.

[사용자에게 보이는 화면- 프리젠테이션 로직] 과 [내부적으로 계산되는 로직- 비즈니스 로직] 으로 나뉘어서 로직의 개발이 완료되지 않아도 화면 개발이 가능하며, 로직이 변경되어도 사용자에게 보이는 화면은 변하지 않는다.

따라서 개발할 때도 화면과 로직이 서로 의존성을 가지지 않는 것이 추후 보수 등을 생각하면 이상적이다. 

MVC 와 MTV , 아주아주 비슷하지만 정말 약간만 다른, 실질적으론 같은 아키텍쳐이다.

**- MVC**(**M**odel , **V**iew, **C**ontroller)

**- MTV**(**M**odel , **T**emplate, **V**iew)

뭔가 내용물이 비슷해 보인다. Model 과 View 는 공통, Controller / Template 는 달라 보인다. 사실일까?

일단 들어가기 전, MTV 모델을 설명하는 이미지를 보며 이해해보자.

![MTV.png](/\assets\img\2023-08-10_WebFrameWork_Concept_MTVvsMVC\image-2.png)

우선 순번을 생각하면.

1.  사용자는 HTTP Response(HTML) 이 렌더 된 화면을 보게 된다.
2.  사용자는 페이지 이동이나 CRUD 작업 등을 해 HTTP Request 를 보낸다.
3.  HTTP Request는 Urls 를 통해 적절한 View 내부의 함수를 찾아간다.
4.  필요에 따라 Views 에서는 Models 에 접근한다.
5.  필요에 따라 Models 에서는 Database 에 접근해 데이터를 가공한다.
6.  필요에 따라 Models 에서 Views 로 데이터를 전달한다.
7.  Views 에서 내부 함수 처리가 완료되었다.
8.  Views에서 Template 에 접근해 필요한 HTML 파일을 렌더한다.
9.  Views 에서 처리 완료 된 결과물을 웹 서버로 전달한다.
10.  사용자는 2번의 Request 가 적용 된 웹 페이지를 본다.

왕 간단하게 훑어만 봤는데 처음 접하면 어렵게 느껴질 것이다.

많이많이 실습하고, 헷갈릴 때 마다 다시 읽어보자!

---

#### 1-3. **MVC**(**M**odel , **V**iew, **C**ontroller)

MVC 는 위에서 말했든 디자인 패턴 중 하나로,

하나의 프로젝트(웹 서비스 등) 을 세 가지 영역으로 나누어 관리 한다는 뜻이다.

세 영역은 이름에서 보이듯

-   Model
-   View
-   Controller

로 구성되어있다.

우선 Model 에 대해 설명하겠다.


#### 1-3-2 . Model

Model은 우선 [어떤 것] 을 의미한다.

대표적으로 Database 가 있는데, 알고리즘, 쿼리 등등도 있을 수 있다.

Model은 MVC 패턴에 따르면 View(MTV=Template) 와는 바로 연결될 수 없고 반드시 Controller(MTV=Views) 를 거져서 HTTP로 갈 수 있다.

-   프레임 워크 와 Database 를 연결 시켜주는 역할. 데이터의 형태를 나타낸다.(필드를 지닌다)
-   각각의 모델은 Database 의 Table 과 Mapping 됨

- **프레임 워크와 Database를 연결시켜주고, 데이터의 형태를 나타낸다.**

MVC 구조 상 View와 Controller 는 Database 에 직접 연결할 수 없다. 이는 유지 보수를 위한 것으로, Model 을 생성해 Db 에 연결하게 만든다.

어떤 Database 를 사용할지는 사용자의 상황에 따라 다르다.

대표적으로는 SQLite, MySQL, SQL Server 등등 다양한 Database 가 존재한다.

Model은 Database 그 자체가 아니다. Database 내부의 Table 에 좀 더 가깝다.

즉, 데이터를 지닌 하나의 데이터 덩어리임을 설정해야 한다.

데이터의 형태에 따라 데이터 필드를 정의한다.

![MTV.png](/\assets\img\2023-08-10_WebFrameWork_Concept_MTVvsMVC\image.png)
출처: Model field reference

정의를 완료했다면 Model을 바탕으로 Database에 만들어 줘야 한다.

**각각의 모델은 Database 의 Table 과 Mapping 됨**

웹 프레임워크를 다루면 하나의 프로젝트 내부에 다른 종류와 이름을 가지는 Database의 Table 를 다수 만들게 된다.

그것들을 어떻게 구분할까.

간단하다, 각 Model 마다 Mapping 된 Table 을 가지게 하면 구분이 쉬워진다.

#### 1-3-3 . VIEW (MTV=Template)

View 는 사용자에게 \[보여지는 것\] 이다.

HTML 파일과 데이터들이 렌더되어 보여지는 것이다.

![MTV.png](/\assets\img\2023-08-10_WebFrameWork_Concept_MTVvsMVC\image-2.png)

이 그림처럼 View(Template) 는 오직 Controller(View) 를 통해서만 Response할 수 있다.

단순히 HTML 파일 그 자체로 보여지는 것이 아니라, Controller(Views)에 의해 데이터를 전달하거나 요청 받는 매개체로 이용된다.

#### 1-3-4 . Controller (MTV=Views)

Controller(Views) 는 말 그대로 앞의 Model 과 View를 \[컨트롤\] 하는 역할이다.

\[내부적으로 동작하는 로직\] 을 담당하는 부분이다.

위에도 적었듯, View(Template)를 가공해 사용자에게 보여주기도 하고, 필요에 따라 Model에 접근해 데이터 입출력을 하기도 한다.

Controller 내부에서는 Request 를 통해 웹에서 요청을 받는다.

그리고 해당 요청은 [GET/POST](https://kimec995.tistory.com/32) 등의 Method 형식으로 받는데, 이에 접근해 특정 데이터를 가져올 수 있다.

#### 1-4 . MTV(Model Templae View) 란?

MVC 에 대한 설명이 길었다. 그렇다면 MTV는 무엇일까?

1-1 에서도 설명했었듯이 MTV와 MVC 는 아주아주 유사해 거의 하나의 아키텍쳐로 본다 했었다.

- Model : Database 연결을 담당한다.

- Template: 웹 화면을 담당한다.

- View: 내부 로직을 담당한다.

MVC 랑 잘못쓴게 아닐까 싶을 정도로 유사하다.

아주 약간의 차이가 있는데,

1. MTV 아키텍쳐를 사용하는 프레임 워크는 django 이지만 / MVC 아키텍쳐를 사용하는 프레임 워크는 Flask, Spring 등 많다.

결국 django 용 아키텍쳐 이고, 이름만 다른 녀석이 더 많이 쓰인다는 것이다.

2. MTV의 Template 와 MVC 의 View 역할의 차이.

Templat를 웹 페이지에 보이기 위해서는 Views 에서 모델링을 해줘야 한다.

다만 역할의 기준이 좀 다르다.

Controller(MVC) 의 역할이 쪼개져 Views(MTV)에서는 모델링과 로직 계산을 , django 프레임워크 자체적으로 브라우저와 상호작용 한다.

하지만 큰 개념으로 보면 결국 같다..

#### 1-5. 어쩌다가 구분하게 되었는가?

사실 MVC 가 더 많이 쓰이는 이유는 MVC 가 먼저 나왔기 때문이다.

MVC 의 유래를 살펴보면 \[[Trygve Reenskaug이 1979년](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC) [최초로 소개했다](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC)\]  고 한다.

그리고 MTV를 사용하는 django는 2005년 [Adrian Holovaty](http://www.holovaty.com/), [Simon Willison](https://simonwillison.net/) 에 의해 개발되었다.

당연하게도 MVC 구조를 이용하다 무언가 문제점/ 혹은 개선점 을 발견해 MTV 구조를 만든 것이 아닐까.

이 부분은 더 많은 공부가 필요할 것 같다.(추가예정)

이후 포스트에선 Django 툴 하나만을 가지고 MTV 구조를 파헤쳐 볼 생각이다.
