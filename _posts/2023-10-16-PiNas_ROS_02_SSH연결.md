---
title: PiNas_ROS_02_SSH 연결
author: Kimec995
date: 2023-10-16 16:00:00 +09:00
categories: [ToyProject, PiNas]
tags: [RaspberryPi, ToyProject, NAS, Server, Developer_Kit]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/RaspberryPi_NAS/RaspberryPi_NAS_000.png
  alt: 토이프로젝트)PiNas_02_SSH 설정하기
---
# PiNas
- 라즈베리 파이 SSH 연결

- PiNas 전체 목록 [링크]

> 23.10.16 작성

# 2. 라즈베리 파이 라즈비안 SSH 연결

## 1. sudo 설정

root계정의 비번부터 설정하자.
```bash
~$sudo passwd root

바꿀 PW입력
```

루트계정으로 전환
```bash
su root
```

이제 root 계정으로 접속했다.

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_14.png)

## 2. 인터넷 확인

root 계정에 접속하던 안하던 상관없다.

```
~# ifconfig
```

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_15.png)

현재 라즈베리 파이에 랜선을 연결해 `eth0`의 inet이 보인다.

만약 무선 연결 이면 `wlan0` 에 나올 것.

## 3. SSH 설정하기

### 1. SSH 활성화
라즈베리 파이 설정창 띄우고

```
sudo raspi-config
```

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_16.png)

`Interface Options` 에 접속,

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_17.png)

마침 `SSH`가 첫 번째에 있다.

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_18.png)

SSH연결 주의문. 그냥 `YES`

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_19.png)

됐다.

### 2. SSH 설정 변경하기

터미널로 돌아가

root 계정 젒속
```bash
~$ su root
```

etc/ssh 경로이동
```bash
cd /
cd etc/ssh
```

sshd.config 파일 수정
```bash
vi sshd_config 

혹은

nano sshd_config
```

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_20.png)

기본 Port는 22이지만, 보안이 엄청 취약해지니까 주석은 놔두고 아래에 새 Port 번호를 부여하자.

- 0 ~ 65535 : 포트 번호
- 0 ~ 1023 : 알려진 번호
- 80 : HTTP 통신
- 443 : HTTPS 통신

[포트와 도메인](https://kimec995.github.io/posts/HTTP-PORT_DNS/)

1번부터 1023까진 알려진 번호라서 가능한 안쓰는게 좋고(사용하고 있지 않으면 써도 되는데 찾기 귀찮다.) 1024 ~ 65535 사이의 숫자 중 원하는 숫자를 사용하자.

최하단부에

```
AllowUsers 사용자이름
```
을 작성 후 저장.

### 3. SSH 접속하기

이제 메인 PC에서 접속하자.

만약 image를 라즈베리로 실행하거나 특별히 지정 안했다면

```bash
ID: pi
PW: raspberry
```

이고 설정 했다면 라즈베리 터미널에 접속한 계정을 쓰자.

PuTTY 실행.[다운받기](https://www.putty.org/)

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_21.png)

1. [인터넷 확인하기](#2-인터넷-확인하기2.) 에서 찾은 IP 주소를 입력

2. 설정한 Port 번호 입력.

3. 접속 할 때 마다 입력하기 귀찮으니 설정 저장.

4. Open 누르기(확인하고 싶으면 Load)

![image.png](/\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_13.png)

메인 PC에서 접속!

간단하게 이거저거 가지고 놀아보자.

현재 23.10.16 기준 설정에서 root 권한을 따로 지정하지 않아도 SSH에서 root 접속이 가능하다. 예전엔 권한 지정해줬던 것 같은데 업데이트 한걸까.