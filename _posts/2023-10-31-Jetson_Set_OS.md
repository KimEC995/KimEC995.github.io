---
title: JetsonNano OS 설치하기
author: Kimec995
date: 2025-10-31 14:00:00 +09:00
categories: [Jetson]
tags: [Jetson, Developer_Kit, CUDA]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/JetsonNano_Logo.png
  alt: JetsonNano OS 설치하기
---

젯슨보드는 비싸다. 비싸기만 한게 아니라 아두이노, 라즈베리에 비해 구하기도 번거롭다. 물론 제품 퍼포먼스 자체가 다르니 그럴 수 밖에 없고 비싼거도 타 제품에 비해 저렴한 편이지만 하여튼간에 눈이 안갔다. 근데 이게 웬걸. 어찌저찌 몇 개를 업어왔고 생긴 김에 갖고 놀다 좀 익숙해지면 토이프로젝트를 진행해보려 한다.

## 0. 준비물

- Jetson Nano

- 16GB MicroSD Card

- HDMI to HDMI

- 5V 2A C타입 충전기

- 키보드, 마우스, 모니터

### 1. Jetson Nano

얻어온거라 스펙을 좀 찾아보니 내가 받은건 2GB모델이었다. 테스트니 크게 개의치 않다. 젯슨보드는 CUDA언어를 사용하는 보드로 그래픽카드를 활용해 AI분석에서 강세를 보인다. 우선 지금 생각나는 추후 프로젝트는 기타 운지법 분석?

### 2. 16GB MicroSD Card

2GB모델은 일반적인 4GB와는 달리 MicroSD의 용량이 커야하는데 굳이 16을 선택하는 이유는

1. 많으니까

2. 프로젝트가 아닌 OS 설치 테스트여서

이다.

우선 4GB던 2GB던 공식페이지에선 32를 권장한다. 참고.

### 3. 충전기, 키마, 모니터

지금은 사무실에서 짬내서 하는거니 사무실 비품을 활용한다!! 이후 집에 가져가면 그때 다시 생각해야지.

## 1. OS 이미지 굽기

우선 사용할 MicroSD카드를 준비한다.

열어보니 언제 만들었는지 기억도 안나는 파티션이 분할되어있다...

![image.png](/\assets\img\postimg\Jetson\Jetson_Nano_02.png)

하나로 뭉치자. FAT32로.

![image.png](/\assets\img\postimg\Jetson\Jetson_Nano_03.png)

Micro SD 준비 끝!

이제 이미지를 준비하자.

[공식다운로드 링크](https://developer.nvidia.com/embedded/downloads#?tx=$product,jetson_nano)

공식 다운로드 홈페이지에서 Nano OS 이미지를 검색한다.

![image.png](/\assets\img\postimg\Jetson\Jetson_Nano_01.png)

다운로드 후 

[Rufus](https://rufus.ie/ko/#google_vignette) 에서

## 참고

[엔비디아 공식홈페이지: Get Started with Jetson Nano](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-2gb-devkit#prepare)