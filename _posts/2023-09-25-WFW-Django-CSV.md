---
title: Django 에서 CSV 파일 읽기1 
author: Kimec995
date: 2023-09-25 00:21:00 +09:00
categories: [WebFrameWork, DataBase]
tags: [WebFrameWork, Django, environ, 장고, 환경설정, CSV, Database]
pin: true
math: true
mermaid: true
---

2023년 09월. Django로 머신러닝 결과물을 서비스 하고 그래프 그리는 프로젝트를 하고 있다.

그러던 중 `CSV`의 데이터를 받아 사용자가 선택할 수 있도록 리스트를 띄워야 하는 상황이 되었다.

우선 DB에 CSV 데이터를 넣고 HTML Datalist 로 띄울 생각이었다.

그래서 열심히 구글링을 했고 열라 많은 자료들 사이에서 마음에 들었던 방법들을 짬뽕해 포스트 적는다.

참고로 두 가지 방법을 찾았는데 우선 첫 번째 작성먼저.

# Django에서 DB에 csv 파일 적용하기. - SQLite3 사용

## 전체 개요

1. 장고에서 `models.py` 에서 구성. 타입과 이름 지정
2. migrate
3. 사용DB: SQLite3
4. 모델 인스턴스 생성
5. 확인 및 적용

## 시작하기. CSV 생성
이 포스트를 위해 CSV를 만드는건 너무나도 비효율적이다.

따라서 이미 생성된 멋진 CSV 파일을 가져온다!

[CSV 예제 - Kaggle: Goodreads](https://www.kaggle.com/datasets/jealousleopard/goodreadsbooks)

![Djangoimportcsv_bookcsv_230910.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv_230910.png)

확인 결과 12개의 columns가 존재한다.

이 많은 열 중 몇 개만 확인하려 한다.

자세한 내용은 링크 확인.


## Django 시작하기.

```Shell
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

다 하면 번거로우니 publication_date, publisher는 포함하지 않는다.

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


## SQLite Terminal 사용하기

![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv07_230910.png)


구글 검색창에 `SQLite Terminal` 을 검색하면

![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv08_230910.png)

[Command Line Shell For SQLite 페이지](https://sqlite.org/cli.html)가 나온다.

`DownLoad` 탭으로 들어가 자신에게 맞는 OS로 다운로드한다.

나는 Window 10 64bit 를 사용하는데 그냥 번들로 받았다.
![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv09_230910.png) 

압축을 풀고 모든 파일(사실 sqlite3만 해도 된다)을 프로젝트 디렉토리에 넣는다. 경로는 Root Directory(manage.py와 같은 위치)로.

![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv10_230910.png) 

(이거도 사진은 안그러네...뭐냐...)


이후 두 가지 방법이 있다.


1. 윈도우 cmd 터미널을 통해 sqlite3.exe에 접근한다.

```Shell
이동 -> 파일명.형식 DB이름

sqlite3.exe db.sqlite3
```

하면 sqlite 터미널이 나온다.

2. bash를 통해 접근한다.

```Shell
python manage.py dbshell
```

뭐가 되었든 sqlite3 DB에 접근한다.

터미널이
```
sqlite> 
```

로 변했으면 접근한거다.

## CSV 변환

그 전에 모델 업뎃도 했겠다, DB 업뎃 전 마지막 점검이다.

![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv11_230910.png) 

해당 CSV에는 열 이름들이 포함되어있는데, 어차피 모델에서 지정해 주었기 때문에 삭제한다.

또한 위에서 포함하지 않기로 한 publication_date, publisher열 도 같이 삭제한다.

한 가지 주의점은 만약 당신이 열을 삭제했더라도, `,` 구분은 남아있을 수 있으니 잘 관찰하자. 
만약 남아있다면 귀찮게 하나하나 지우지 말고 엑셀프로그램 아무거나 열어 빈 열을 삭제한다.

이후 파일을 실행할 파일과 같은 위치로 옮긴다.


## DB를 csv 모드로 변환

```Shell
sqlite> .mode csv
```

아무것도 변하지 않아보이겠지만 아니다. CSV를 인식 할 준비가 되었다.

```Shell
sqlite> .import CSV파일이름.csv 앱이름_DB 테이블(models.py에서 선언한것)이름

sqlite> .import books.csv book_book
```

주의점은 뒤 DB이름을 풀네임으로 작성해야 하는 것이다.

앞이나 뒤만 작성하면 알 수 없는 새로운 테이블이 생성되니 관리하려면 꼭 형식을 지키자.

엔터를 누르면...

![image.png](\assets\img\postimg\Djangoimportcsv_bookCSV\Djangoimportcsv_bookcsv12_230910.png)

엄청 긴 열땜에 한 화면에 못담았지만, DB에 들어간 모습을 볼 수 있다.

이후 용량을 위해 CSV는 지워도 DB에서 읽을 수 있다.

## 마무리
우선 SQL에서 직접 접근해 csv파일을 불러오는 방법을 확인했다.

이 다음 포스트는 인스턴스를 직접 생성해 DB에 저장하는 방법을 해보려 한다.


## 참고
[Django Model Field](https://docs.djangoproject.com/en/4.2/ref/models/fields/)

수많은 구글링