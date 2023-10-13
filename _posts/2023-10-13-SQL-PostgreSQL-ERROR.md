---
title: ERROR- IMPORT CSV(Permission denied SQL state 42501)
author: Kimec995
date: 2023-10-13 13:10:00 +09:00
categories: [SQL, Concept]
tags: [SQL, PostgreSQL, Basic, Concept]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/SQL_ALL/PostgreSQL_00.png
  alt: PostgreSQL ERROR
---

SQL 간이 테스트 중 CSV 파일 IMPORT를 안했어서 시도했다 오류 수정하느라 실패했다.

급한 마음이 원인이었어서 좀 허무한데 잊지 않기 위해 기록

## Permission denied SQL state 42501

환경설정을 완료하지 않고 CSV 파일을 IMPORT 하면 해당 오류가 나온다.

![image.png](\assets\img\postimg\SQL_PostgreSQL\Error00_00.png)

따라서 환경설정

![image.png](\assets\img\postimg\SQL_PostgreSQL\Error00_03.png)

File > Preferences > Paths > Binary Path
에서 맞는 버전의 ProgresSQL의 BIN 경로를 잘 잡아준다. **SAVE** 하고 다시 IMPORT 하면..

![image.png](\assets\img\postimg\SQL_PostgreSQL\Error00_01.png)

> 이미지는 잘못찍었는데, 같은 이름의 CSV 파일이 맞았다.

`columns` 에러다.

아, Header를 못 읽는구나~ 하고 체크해주면

![image.png](\assets\img\postimg\SQL_PostgreSQL\Error00_04.png)

![image.png](\assets\img\postimg\SQL_PostgreSQL\Error00_05.png)


쨔잔~ 여전히 못읽고있다~~

### 시도한것
- Delimiter, Quote, Escape 등 수정
- Columns 직접 입력
- psql 터미널에서 직접 입력
- PowerShell 에서 권한 수정
- CSV 파일 수정

## 해결

**PGADMIN4** 재부팅!

아하~~~~!!

환경설정에서 Binary Path를 다시 잡아야 하는데 안하고 그냥 밀고가서 안됐던거구나~

급한 마음에 엄청 기초를 실수했다. 이제 다신 이런일이 있지 않게 노력하자!!