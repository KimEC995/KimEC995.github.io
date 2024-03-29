---
title: RaspberryPi 3 Model B Ububntu 18.04
author: Kimec995
date: 2024-03-04 14:00:00 +09:00
categories: [Linux]
tags: [RaspberryPi, Linux, Developer_Kit]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/RaspberryPi_IMG.png
  alt: 라즈베리파이 OS Ubuntu 18.04
---
라즈베리 파이 모델 3B에 Ubuntu 18.04를 설치했다.

# 하드웨어
- Raspberry Pi 3 Model B V1.2
- 16GB Micro SD

# 포멧 후 설치
## 1. 포멧
[SD카드 포멧](https://www.sdcard.org/downloads/formatter/)
설치 후 포멧.

SD카드가 젯슨 용으로 썼던 거라 파티션 많음.

![image.png](\assets\img\postimg\RaspberryPi_OS\RaspberryPi_OS_001.png)

## 2. ubuntu 설치
ubuntu 18.04 설치
[Ubuntu Mate - Download - Browse Downloads](https://releases.ubuntu-mate.org/)

경로: /archived/18.04/arm64/

![image.png](\assets\img\postimg\RaspberryPi_OS\RaspberryPi_OS_002.png)


[rufus](https://rufus.ie/ko/#google_vignette) 사용

![image.png](\assets\img\postimg\RaspberryPi_OS\RaspberryPi_OS_003.png)


## 3. 완료 후  SSH 세팅
모니터, 키보드, 마우스 세팅 후 부팅

화면에 Ubuntu Mate가 뜨면 기본 설정

설정 완료 후

1. 패키지 정보 업데이트
```bash
sudo apt update
sudo apt upgrade
```

2. ssh server 설치
```bash
sudo apt install openssh-server
```

3. 활성화
```bash
sudo  systemctl enable ssh
sudo  systemctl restart ssh
```

4. 설치 확인
```bash
sudo systemctl status ssh
```

활성화 안되면 `Activate:inactive(dead)`
활성화 되면 `active(running)`

5. PUTTY에서 접속 확인
된다~
