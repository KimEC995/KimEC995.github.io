---
title: Django CSV 읽기 - Python+Pandas
author: Kimec995
date: 2023-09-25 00:21:00 +09:00
categories: [WebFrameWork, DataBase]
tags: [WebFrameWork, Django, 장고, CSV, Database, SQLite3]
pin: true
math: true
mermaid: true
---

2023년 09월. Django로 머신러닝 결과물을 서비스 하고 그래프 그리는 프로젝트를 하고 있다.

그러던 중 `CSV`의 데이터를 받아 사용자가 선택할 수 있도록 리스트를 띄워야 하는 상황이 되었다.

우선 DB에 CSV 데이터를 넣고 HTML Datalist 로 띄울 생각이었다.

그래서 열심히 구글링을 했고 열라 많은 자료들 사이에서 마음에 들었던 방법들을 짬뽕해 포스트 적는다.

참고로 두 가지 방법을 찾았는데 이번엔 두 번째.

--- 

## 전체 개요

1. 장고에서 `models.py` 에서 구성. 타입과 이름 지정
2. migrate
3. 사용DB: SQLite3
4. **Pandas로 읽고 Python으로 적용**
5. 확인

[이전 포스트](https://kimec995.github.io/posts/WFW-Django-CSV/)와 같지만 다른 방법이다.

아마 3번까진 동일할거니 [여기](#csv인식-할-파일-만들기)부터 보면 되겠다.


## 시작하기. CSV 생성
이 포스트를 위해 CSV를 만드는건 너무나도 비효율적이다.

따라서 이미 생성된 멋진 CSV 파일을 가져온다!

[CSV 예제 - Kaggle: Goodreads](https://www.kaggle.com/datasets/jealousleopard/goodreadsbooks)

![Djangoimportcsv_bookcsv_230910.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv_230910.png)

확인 결과 12개의 columns가 존재한다.

이 많은 열 중 몇 개만 확인하려 한다.

자세한 내용은 링크 확인.


## Django 시작하기.

```bash
$ pip install django
$ django-admin startproject ENVIRON
$ django-admin startapp book
```

```python
# settings.py
INSTALLED_APPS = [book]
```


프로젝트와 앱을 만들고 시작한다.

## CSV 파일 넣기
![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv02_230910.png)

`manage.py`(변경하지 않는 한 ROOT Directory) 와 같은 위치에 가져온 CSV파일을 넣는다.(지금보니 사진은 아니다.)

## 모델 생성

CSV파일을 다시 보자.
![Djangoimportcsv_bookcsv_230910.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv_230910.png)

보이는 바와 같이

bookID : 순번 - IntegerField\
title : 책 제목 - CharField\
authors : 저자명 - CharField\
average_rating : 평점 - FloatField\
isbn : 식별번호 - CharField\
isbn13 : 13자리 식별번호 - CharField\
language_code : 언어 - ChaerField\
num_pages : 페이지 수 - IntegerField\
ratings_count : 총 대여수 -  BigIntegerField\
text_reviews_count : 책 후기 - BigIntegerField\
publication_date : 출판일 - DateField\
publisher : 출판사 - CharField\

형식이다.

BigIntegerField 는 64비트로 정수를 표현하는 방식으로, -9223372036854775808 부터 9223372036854775807 까지의 큰 수를 표현할 때 사용한다.

다 하면 번거로우니 bookID, publication_date, publisher는 포함하지 않는다.

```python
# book: models.py
from django.db import models
from django.utils.translation import gettext as _

class Book(models.Model):
  title = models.CharField(_("title"), max_length=255)
  authors = models.CharField(_("authors"), max_length=255)
  average_rating = models.FloatField(_("average rating"))
  isbn = models.CharField(_("isbn"), max_length=150)
  isbn13 = models.CharField(_("isbn 13"), max_length=150)
  language_code = models.CharField(_("language code"), max_length=10)
  num_pages = models.IntegerField(_("number of pages"))
  ratings_count = models.BigIntegerField(_("rating count"))
  text_review_count = models.BigIntegerField(_("text review count"))
  publication_date = models.DateField(_("publication date"), auto_now=True)
  publisher = models.CharField(_("publisher"), max_length=150)
```

이만하면 되겠다.

## DB 생성하기

우선 만든 models.py를 migrate 한다.

```bash
$ python manage.py makemigrations

$ python manage.py migrate
```

이후 db.squlite3 를 확인하기 위해 앱을 설치한다.

`SQLite` 검색

![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv03_230910.png)

나는 이미 받아서 `Install` 이 안보이는데, 그 버트을 누르면 설치된다.

설치 이후 `db.sqlite3` 를 확인한다. 보통 RootDirectory에 있다.

![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv04_230910.png)

우클릭 하고 `Open Database` 를 누르면

![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv05_230910.png)

하단에 `SQLITE EXPLORER` 탭이 보인다.

![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv06_230910.png)

migrate가 정상적으로 완료되었다면 `book_book` 이라는 Table이 생성된 것이 확인된다.

Table 옆 화살표를 누르면 이미지의 우측 창 처럼 SQL Table이 보인다.

당연히 아직은 아무것도 없다.

지금 한건 Model의 형식만 지정한거고, CSV 파일과 연결하지 않았기 때문이다.


## CSV인식 할 파일 만들기

CSV 파일과 같은 위치에

`CSV파일과같은이름.py`를 생성한다.

굳이 같은 이름일 필요는 없지만, 헷갈리니까...

이후 코드를 작성한다.

```python
#파일이름은 Books.py로 생성
import pandas as pd
import sqlite3
import csv

#CSV 파일 로드
df = pd.read_csv("Books.csv(CSV파일이름.csv)")

# DB랑 연결
database = "db.sqlite3"
conn = sqlite3.connect(database)
dtype={
    "title" : "CharField"
    , "authors" : "CharField"
    ,"average_rating" : "FloatField"
    , "isbn" : "CharField"
    , "isbn13" : "CharField"
    , "language_code" : "CharField"
    , "num_pages" : "IntegerField"
    , "ratings_count" : "BigIntegerField"
    , "text_review_count" : "BigIntegerField"
}

df.to_sql(name='book_book(DB이름)', con=conn, if_exists='replace', dtype=dtype, index=True, index_label="id")

conn.close()
```

dtype은 Dictionary 형태로 지정, Key값은 CSV의 열 이름, Value는 Django DataField 로 지정한다.

그리고 `index=True, index_label="id"` 로 지정한다.

`models.py`를 migrate 한 후 생성되는 `migrations` 디렉토리를 살펴보면 분명 id를 선언한 적 없는데 id 선언이 되어있는게 확인된다.

Django 특성상 id는 반드시 존재해야 하기 때문에 선언하지 않았다면 너무 친절하게 자동으로 추가해준다... 하지만 지금처럼 `bookID` 같이 ID관련 열을 제거하거나 없다면 Django에서는 오류로 인식한다.

따라서 csv를 Pandas로 읽는 지금 Pandas의 index는 'id'라는 이름으로 가져올 것이다.


## 파일 실행시키기

```bash
python Books.py(방금생성한파일)
```

엔터를 누르면 무언가 휙휙 변하고...

![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv12_230910.png)

엄청 긴 열땜에 한 화면에 못담았지만, DB에 들어간 모습을 볼 수 있다.

이후 용량을 위해 CSV와 생성한 파일은 지워도 DB에서 읽을 수 있다.

또한 수정하려면 생성한 파일을 다시 실행하면 된다.


## 마무리
이번엔 Python으로 Pandas를 사용해 CSV를 직접 읽고 DB에 저장하는 코드를 작성해 보았다.

이전 
[CSV -> DB -> Django 파일 읽기 - SQLite3](https://kimec995.github.io/posts/WFW-Django-CSV/) 과 비교해 어떤게 더 나을지는 사용자의 몫이다. 잘 배우자.


## 참고
[Django Model Field](https://docs.djangoproject.com/en/4.2/ref/models/fields/)

수많은 구글링