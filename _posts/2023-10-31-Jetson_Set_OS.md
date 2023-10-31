---
title: JetsonNano OS와 환경설정
author: Kimec995
date: 2023-10-31 15:00:00 +09:00
categories: [Jetson]
tags: [Jetson, Developer_Kit]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/JetsonNano_Logo.png
  alt: JetsonNano OS 설치하기
---

개발보드는 종류가 참 많다. 아두이노같은 진짜 장난감용 보드부터 Intel NUC같은 산업용 PC까지 정말정말 많다. 그 중 젯슨보드를 사용해 보려 한다.
젯슨보드는 CUDA언어를 사용하는 보드로 그래픽카드를 활용해 AI분석에서 강세를 보인다. 어찌저찌 몇 개를 업어왔고 생긴 김에 갖고 놀다 좀 익숙해지면 토이프로젝트를 진행해보려 한다.

## 0. 준비물

- Jetson Nano

- 16GB MicroSD Card

- HDMI to HDMI

- 5V 2A C타입 충전기

- 키보드, 마우스, 모니터

### 1. Jetson Nano

얻어온거라 스펙을 몰라서 좀 찾아보니 내가 받은건 2GB모델이었다. 테스트니 크게 개의치 않는다. 우선 지금 생각나는 추후 프로젝트는 기타 운지법 분석? 해보면 또 생각날지도.

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_04.png)


### 2. 16GB MicroSD Card

2GB모델은 일반적인 4GB와는 달리 MicroSD의 용량이 커야하는데 굳이 16을 선택하는 이유는

1. 많으니까

2. 프로젝트가 아닌 OS 설치 테스트여서

이다.

우선 4GB던 2GB던 공식페이지에선 32를 권장한다. 참고.

### 3. 충전기, 키마, 모니터

지금은 사무실에서 짬내서 하는거니 사무실 비품을 활용한다!! 이후 집에 가져가면 그때 다시 생각해야지.

## 1. 포멧하기

우선 사용할 MicroSD카드를 준비한다.

열어보니 언제 만들었는지 기억도 안나는 파티션이 분할되어있다...

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_02.png)

하나로 뭉치자. FAT32로.

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_03.png)

Micro SD 준비 끝!

## 2. OS 이미지 굽기

이제 이미지를 준비하자.

[공식다운로드 링크](https://developer.nvidia.com/embedded/downloads#?tx=$product,jetson_nano)

공식 다운로드 홈페이지에서 Nano OS 이미지를 검색한다.

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_01.png)

나는 2GB모델이라 전용 이미지를 선택했다. 길고 긴 다운로드 후 

[Rufus](https://rufus.ie/ko/#google_vignette) 에서 작업.

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_05.png)

17분 걸렸다.

## 2. 사용자 설정하기 - 디스플레이 연결

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_06.png)

카드 끼고 포트들 연결하면 설정창이 나온다.

순서는

- 소프트웨어 동의

- 언어, 레이아웃 등 선택

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_07.png)

한국어가 궁금해서 한국어로 설정해봤다.

- 사용자 설정

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_08.png)

- 파티션 설정

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_09.png)

일단 최대크기로

- SWAP File 여부

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_10.png)

SWAP File은 (.SWP) 확장자를 사용하는 가상메모리로 사용되는 파일이다. 메모리가 부족하거나 여러 상황 등으로 애플리케이션이 비 정상적으로 종료되면 임시저장하는 용도로 사용된다.

지금은 우선 생성했다. 필요 없어지면 나중에 지워야지.

- Nvpmodel 

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_11.png)

Jetson보드의 전력관리모드 설정. 추후 변경이 가능하니 우선은 기본값으로 설정했다.

- 부팅!

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_12.png)

이미지 다운과 굽는게 오래걸렸어서 부팅은 빠르다고 느껴진다. 설정을 완료하면 해당 화면이 반겨준다.

---

그나저나 지금 젯슨보드는 모니터, 키마 등 공간차지가 엄청나다. 개발환경이 이러면 나중에 보통 골치가 아니니 공간을 줄이자. 두 가지 방법으로 공간을 줄일 수 있다.

1. 헤드리스 연결

2. SSH 연결

헤드리스 연결은 물리적인 케이블 연결, SSH연결은 네트워크 연결이다. SSH는 네트워크 리소스를 사용하는 단점이(환경에 따라 다름) 헤드리스는 케이블로 묶이는 만큼 공간제약의 단점이 존재한다.

둘 다 진행하며 비교하겠다.

## 번외- 헤드리스 연결

간단하다. 젯슨 포트 중 Micro-USB를 사용하면 연결할 수 있다.

### 1. 장치 연결 후 확인하기

연결 후 장치관리자를 통해 물리 포트 번호를 확인한다.

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_13.png)

내 보드는 COM3번으로 연결됐다.

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_14.png)

자세한 사항은 더블클릭으로 확인할 수 있다.

'Details' 로 들어가 'Hardware ids' 탭으로 확인하자. 값이 VID_0955 / PID_7020이면 Jetson보드가 맞다.

### 2. 터미널 연결하기

[PuTTY](https://www.putty.org/) 로 Port연결하자.

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_15.png)

Serial 연결로 Port 번호를 작성한다. 속도는 115200.

OPEN 하고 로그인 하면

![image.png](\assets\img\postimg\Jetson\Jetson_Nano_16.png)

**헤드리스 연결 완료!**

## 번외- SSH 연결

작업 할 PC와 Jetson보드를 동일 네트워크에 연결한다.

근데 네트워크 문제가 발생했다... 이 부분은 네트워크 해결 후 마저 작성하자ㅠㅠ

## 참고

[엔비디아 공식홈페이지: Get Started with Jetson Nano](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-2gb-devkit#prepare)

[초기설정](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-2gb-devkit#setup)