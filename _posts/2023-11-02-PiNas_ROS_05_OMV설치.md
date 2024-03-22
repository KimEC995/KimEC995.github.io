---
title: PiNas_ROS_05_OMV설치
author: Kimec995
date: 2023-11-02 15:00:00 +09:00
categories: [ToyProject, PiNas]
tags: [RaspberryPi, ToyProject, NAS, Server, Developer_Kit]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/RaspberryPi_NAS/RaspberryPi_NAS_000.png
  alt: PiNas_ROS_05_NAS_설정하기
---
# PiNas
- 네트워크 설정

- PiNas 전체 목록 [링크]

> 23년 10월 30일. OpenMediaVault(이하OMV)를 설치하려고 했었다.

# OpenMediaVault 설치하기
현재 Debian Linux 12(bookworm)은 OMV6(최신버전)이 안된다.

```bash
[omvinstall] Unsupported version.  Only Debian 10 (Buster) and 11 (Bullseye) are supported.  Exiting...
```

그렇다. Debian 기반인 OMV는 최신 Debian이 나오면 최신 OMV 버전으로 받아야 하는데, 아직 12버전에 대응하는 최신버전은 릴리즈가 안된것이다! 결국 OS 이미지부터 다시 해야한단소리~ OMV7이 [예정되어 있는데](https://www.openmediavault.org/?p=3583) 언제나올지도 모르니 맘 편하게 6버전을 받는게 나을 것 같다.

이제와서 도커로 가긴 좀.. 기왕 여기까지 온거 OMV를 설치하자.

## 00. 이전버전 OS 설치
![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_41.png)

23년 10월 30일 기준의 [OMV 릴리즈노트](https://docs.openmediavault.org/en/stable/releases.html) 에 따라 Debian 11버전, 그리고 혹시 모르니 22년 5월 이전에 나온 Bullseye를 받자.

[이전버전 RaspberryPi Lite img](https://downloads.raspberrypi.com/raspios_lite_armhf/images/?_gl=1*1xlk935*_ga*OTI3NDcxMDY4LjE2OTcwMjk2MjA.*_ga_22FD70LWDS*MTY5ODY1NDA3Ni4zLjEuMTY5ODY1NDA5MS4wLjAuMA..)

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_42.png)

맨 위의 이미지를 받으면 된다.
OS 설치 방법은 [이전포스트](https://kimec995.github.io/posts/PiNas01/#2-os-%EC%84%A4%EC%B9%98) 참고.

## 1. OMV 6 설치
데비안 11 OS를 설치한 후..

터미널에서 입력.

```bash
sudo wget -O - https://github.com/OpenMediaVault-Plugin Developers/installScript/raw/master/install | sudo bash
```

진짜 오래걸린다.

## 2. 확인하기
먼저 확인을 위해 내부 IP로 접속해주자.

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_43.png)

우선 내부IP로 확인해보니 정상 설치된걸 확인할 수 있다.

기본 ID와 PW는 아래와 같다.
```bash
Username: admin
Password: openmediavault
```

## 3. 기존 설정 수정하기
OMV를 설치하며 이전에 설정한 내역들이 날아갔고 다르게 연결해야했다.

### 1. 공유기- 포트포워딩 수정

기존의 내부포트 번호를 OMV6의
![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_44.png)
**시스템 > Workbench > Port**
에 쓰인 번호를(본인이 수정하자)

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_45.png)
**ipTime 관리자 > NAT/라우터 관리 > 포트포워드 설정**
의 `내부포트` 를 수정해주자.

확인을 위해 모바일(데이터) 혹은 외부 네트워크에서 이전에 설정했던
```bash
DDNS.iptime.org:외부포트
```
를 입력해보자.

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_46.png)

모바일에서도 접속이 된다!!

### 2. HDD 연결
사실 이번 포스트 쓰면서 OS 자체를 날렸기 때문에 하드는 연결을 못했다. OMV에서 설정하자.

하드 연결을 확인하고
![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_47.png)
**저장소 > 디스크 > (안보이면)새로고침**

자동으로 두 저장소를 잡은 모습이다.
사실 이전에 했던 자동마운트 등은 안해도 됐던 것 같다... 여기서 다 해주네.... 약간 당황
연결만 하면 되고 따로 설정할 것은 없다.

## 3. SSH 연결
라즈베리 파이 내부의 Port 번호가 바뀌었기 때문에 파이보드의 설정을 바꾸거나 아니면 PuTTY  설정을 바꾸면 된다. 

### 3-1. SSH연결 수정: OMV6
![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_48.png)
**서비스 > SSH > Port**
해당 Port 번호를 처음 PuTTY에 설정 한 번호로 바꿔주면 된다.

### 3-2. SSH연결 수정: PuTTY
![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_21.png)
해당 이미지의 2번을 OMV에서 확인한 번호로 적자.

만약 OMV 확인이 귀찮으면 SSH의 기본 Port인 22번일 확률이 아주아주아주 높으니 22번으로 수정하자.

## 4. OMV6 환경설정하기
드디어 OMV를 설치하고 원격접속도 아주 잘된다.
이제 기본 환경설정으로 넘어가자!

세부사항이나 보안 등은 이후 포스트에서 작성하고 이번엔 기본만.

### 1. 관리자 설정
제일 중요.

웹 관리자의 암호를 수정하자.
![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_49.png)
**우측상단 사용자 아이콘 > Change Password**

변경 완료되면 로그아웃 후 다시 로그인 해본다.

### 2. 시간대 설정
![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_50.png)
**시스템 > 날짜 및 시간**

Asia/Seoul

### 3. 대시보드
![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_51.png)
**우측상단 사용자 아이콘 > 대시보드**

원하는 위젯을 찍고 저장하면

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_52.png)
된다.

우선 이만하면 기본 환경설정은 마무리된 것 같다. 이제 NAS도 만들었으니 한번 서비스를 배포해볼까.