---
title: RaspberryPi 3 Model B Raspbian v1.8.5
author: Kimec995
date: 2024-03-18 09:00:00 +09:00
categories: [Linux]
tags: [RaspberryPi, Linux, Developer_Kit]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/RaspberryPi_IMG.png
  alt: 라즈베리파이 OS Raspbian v1.8.5
---
[지난 포스트(링크)](https://kimec995.github.io/posts/RaspPi_OS/) 에서 라즈베리 파이에 Ubuntu 18.04를 설치했다.
근데 생각해보니 내 환경에서 Ubuntu를 이용하는 것 보다 라즈비안을 이용하는게 더 적절할 것 같아 변경했다.

# 하드웨어
- Raspberry Pi 3 Model B V1.2
- 16GB Micro SD


# 포멧 후 설치
## 1. 포멧
[SD카드 포멧](https://www.sdcard.org/downloads/formatter/)
설치 후 포멧.

지난 포스트의 SD카드여서 다시 포멧했다.

![image.png](\assets\img\postimg\RaspberryPi_OS\RaspberryPi_OS_001.png)

## 2. Raspberry Pi Imager 설치
1.8.5 설치
[Raspbian OS](https://www.raspberrypi.com/software/)

![image.png](\assets\img\postimg\RaspberryPi_OS\RaspberryPi_OS_004.png)

인스톨러여서 설치하는 PC기준의 OS에 맞게 설치한다.

## 3. Raspberry Pi Imager 설정

Imager 설치 완료 후 SD카드를 PC와 연결하고 설정한다. 

![image.png](\assets\img\postimg\RaspberryPi_OS\RaspberryPi_OS_005.png)

![image.png](\assets\img\postimg\RaspberryPi_OS\RaspberryPi_OS_006.png)


동시에 SSH 설정

![image.png](\assets\img\postimg\RaspberryPi_OS\RaspberryPi_OS_007.png)

설정 완료 후 라즈베리에 장착 후 테스트 진행한다.