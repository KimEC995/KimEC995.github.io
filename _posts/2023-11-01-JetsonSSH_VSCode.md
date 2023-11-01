---
title: JetsonNano SSH VSCode연결하기 
author: Kimec995
date: 2023-11-01 10:00:00 +09:00
categories: [Jetson]
tags: [Jetson, Developer_Kit]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/JetsonNano_Logo.png
  alt: JetsonNano SSH VSCode연결하기 
---

작업할 때 마다 PuTTY로 연결하면 번거롭다. VSCode와 Jetson보드 SSH를 연결하자.

## 0. 확장자 설치

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_21.png)

VSCode의 확장자 `Remote-SSH` 를 설치하자. 자세한 제원은 [링크](https://code.visualstudio.com/docs/remote/ssh) 확인. 사실 VSCode 돌아가면 거의 다 된다.

## 1. SSH 설정

설치를 완료하면 좌측 탭에 `Remote Explorer` 라는 아이콘이 생성된다. 클릭.

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_22.png)

`+` 버튼을 누르면 검색창에

```bash
ssh ID@IP주소
```

를 입력한다.

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_23.png)

SSH 설정 관련 파일의 경로를 찾는다. 특별히 변경하지 않았으면 아래와 같다.

```bash
C: > Users > 사용자명 > .ssh > config
```

우측 하단에 알람이 뜨고 파일을 열어보면

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_24.png)

입력했던 정보를 토대로 새로운 SSH가 생성된걸 확인할 수 있다.

**구성**

- Host: 주소 이름. 알아볼 수 있도록 아는 이름으로 바꿔주자 `ID@IP주소`

- HostName: IP주소. 당연하지만 바꾸면 안된다.

- User: 사용자 ID. 접속해야하니까 바꾸지 말자.

**번외**

만약 Port가 기본 설정(22)가 아니라면

- Port: Port번호

만약 인증키가 필요하다면

- IdentityFile: 인증키 파일(.pem) 파일의 경로.

전부 작성했다면 저장하고 새로고침하자. 새로 생성한 Host를 기준으로 탭이 생성된다.

### 2. SSH 연결하기

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_25.png)

확인 되었다면 빨간박스 중 아무거나 눌러도 된다.

좌측은 해당 창에서 열기, 우측은 새 창에서 열기다.

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_26.png)

OS를 선택하자. Jetson은 Linux 기반이니까 `Linux` 선택.

비번을 입력하고 좀 기다리면

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_27.png)

좌측 하단의 SSH 주소와 함께 환경이 열린다. 이후 사용은 VSCode와 동일하다.

## Jetson 시리즈

[시리즈 목록](https://kimec995.github.io/categories/jetson/)

## 참고

[VSCode 공식지원-SSH](https://code.visualstudio.com/docs/remote/ssh)