---
title: 문자열 에러(django-environ 충돌)
author: Kimec995
date: 2023-08-29 16:10:00 +09:00
categories: [WebFrameWork, Error]
tags: [WebFrameWork, Django, Error]
pin: true
math: true
mermaid: true
---

# Error 230829 : 문자열 에러(django-environ 충돌)

어제 [Django-environ](https://django-environ.readthedocs.io/en/latest/quickstart.html) 을 설정했었다.

그로 인한 에러 발생

1. 멀쩡하던 문자열 인식 에러 발생

2. 인자 오류

로, 원래 멀쩡했던 DB 관련 코드


```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

` 'NAME': BASE_DIR / 'db.sqlite3', TypeError: unsupported operand type(s) for /: 'str' and 'str'`

오류가 나온다.

아마 Django-environ 에서 `BASE_DIR` 를 새로 설정해서 그런듯

패스문제?

아니면 변수가 겹치나?

아니면 타입 선언을 새로해야하나?

좀 더 파악해보자

### 원인

- 패스문제는 반쯤 문제였다.

- 변수 겹침은 없었다.

- 타입 선언 문제 <---------------- 이거

23년 08월 29일 현재, [Django-environ](https://django-environ.readthedocs.io/en/latest/quickstart.html) 에서 Quick Start 를 시작하면 기본으로 제공하는 패스가 있는데 출력 결과 `str` 타입이었다.

생각해보니 파이썬 패스 출력 결과는 `str` 으로 보이지만, 사실은 패스를 선언하는 특별한 경로객체이다.

당연히 타입 에러가 날 수밖에.

### 해결

방법은 두 가지


```python
# 1번 / str로 경로를 받는 경우
# settings.py

# str 경로
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

# `/`에서 str 연결 변경 => `/` > `,`
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR, 'db.sqlite3',
    }
}
```


```python
# 2번 / 경로 객체로 경로를 받는 경우
# settings.py

# str 경로
BASE_DIR = Path(__file__).resolve().parent.parent

# SECRET_KEY 타입 변경
SECRET_KEY = env.str('SECRET_KEY')
```

개인적으론 2번 방법이 마음에 든다.

경로 객체를 사용하는 경우가 많을까, 아니면 `str` 객체를 쓰는 경우가 많을까 생각하면 전자일거라는 판단.

필요한 경우에만 바꿔주자.