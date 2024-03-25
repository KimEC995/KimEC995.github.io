---
title: PiNas_ETC_WAP 다중 엑세스 포인트
author: Kimec995
date: 2024-03-25 13:00:00 +09:00
categories: [ToyProject, PiNas]
tags: [RaspberryPi, ToyProject, NAS, Server, Developer_Kit, WAP]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/RaspberryPi_NAS/RaspberryPi_NAS_000.png
  alt: 토이프로젝트)PiNas_ETC)다중엑세스포인트
---
> 24.03.22 나스 이동

내가 가장 많은 시간을 보내는 곳은 사무실이다.
따라서 나스 시스템을 사무실로 옮겨야 관리가 편하지 않을까?

가장 중요한 관리자님의 허락도 받았겠다 진행 해보자.

# 번외 - 다중 액세스 포인트
현재 사무실 인터넷은 여러 개의 방화벽과 공유기로 이루어져있다.
여기서 토이 프로젝트용 네트워크를 분리하면 서로 간섭이 없고, 내가 admin으로 작업할 수 있게 된다.

간단하게 도식화 하면

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_55.png)

이런 모양 쯤 되겠다.

- **사무실 공유기 1호**
	- 전체 네트워크를 관리한다.
	- 보안도 겸한다.
- **개인 공유기**
	- 내 개인 토이프로젝트들 *만* 관리, 접속. 
- **사무실용 다른 서비스**
	- 문자 그대로 사무실에서 관리하는 다른 서비스들. 

뭐 크게 작업이랄 것도 없지만 구성을 해보자.

## 재료
### 공유기
네트워크 포인터 생성을 위해 우선 공유기가 필요하다.
당근에서 사려고 약속 장소에 도착하니 상대 아저씨가 자느라 못 판댄다... 뭘까..... 그냥 남는거 하나 사용.

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_56.png)

### PiNas
기존의 파이나스를 가져와 새로 연결했다.

## 공유기 관리자 연결
먼저 공유기 설정부터 만져주자
### 확인
마침 무선 공유기라 모바일에서 초기화 확인이 가능했다.

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_57.png)


모바일에서 인터넷 접속, 주소로 이동한다.
```bash
내부 IP 주소: 192.168.0.1

ID: admin
PW: admin
```

접속하면 외부 IP주소를 알 수 있다.

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_58.png)

```bash
외부 IP: 192.168.이미지의 주소
```

이후 관리자님께 허가 받고 사무실 인터넷에 접속 한 후..

### 외부 설정
스샷을 못찍었네..

나중에 옮길거라 우선 DHCP먼저 설정했다. 주소를 고정 한 후

그 후 포트 할당.

다시 내부로 돌아와..

### 내부 설정
모바일에서 접근.

페이지를 지정하기 전, ID와 PW를 바꾼다.
안그럼 관리자 페이지 지정이 안됨.

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_59.png)

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_60.png)

관리자 계정 설정 후

포트를 열었으니 관리자 페이지에 접속할 수 있는 포트를 열어준다.

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_61.png)

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_62.png)


이제 PC에서 접근해보자

### 접속 확인
주소는
```bash
외부 접속 IP: 관리 페이지 포트
```

접속 완료!

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_63.png)

그 후 무선 네트워크 설정 등 맘대로~

### 관리자 추가

### DDNS 추가
![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_64.png)


## PiNas 새로 연결
관리자 설정까지 완료 했으니 PiNas를 다시 연결하자.

### 연결 확인
![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_65.png)

### PiNas DHCP 설정
![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_66.png)

### 포트포워딩
![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_67.png)

### 나스 접속 확인
![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_68.png)

접속 완료~
### 충격 사실
ID, PW 다 까먹어... 자동로그인의 단점...ㅠㅠㅠㅠ

다시 설치하자..ㅠ 하는김에 이번엔 **리눅스+Docker**