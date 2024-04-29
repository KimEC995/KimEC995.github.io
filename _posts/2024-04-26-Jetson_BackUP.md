---
title: Jetson Xavier Backup
author: Kimec995
date: 2024-04-26 10:00:00 +09:00
categories: [Jetson]
tags: [Jetson, Developer_Kit]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/JetsonNano_Logo.png
  alt: Jetson Xavier Backup
---
지금 하는 프로젝트는 [e-CAM](https://www.e-consystems.com/usb-cameras/imx290-low-light-usb-camera.asp) 를 제어하는데, 환경은 Ubuntu 18.04, JetPack 4.6 이어야 한다. 한 가지 문제가 있다면 L4T가 점점 업데이트 되면서 JetPack 4.6은 아마 앞으로 L4T를 이용해 다운이 힘들어 질 것이다. 따라서 지금의 환경을 JetPack에서 제공하는 `flash`를 이용해 백업 할 예정이다.

# JetPack 4.6 BackUp

참고 문서 [링크](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/SD/FlashingSupport.html#to-clone-a-jetson-device-and-flash)


## 백업 파일 생성
### Device
> 정상 작동 및 드라이버들 확인 후

강제 복구모드 진입
1. 중간 버튼 3초
2. 상태
	1. 켜져있음 -> 왼 쪽
	2. 꺼져있음 -> 오른 쪽

### Host
SDKmanager version: `2.1.0.11660`

위치
```bash
절대 경로/Linux_for_Tegra
```

```bash
sudo ./flash.sh -r -k APP -G <clone> <board> mmcblk0p1
```

- `<clone>`: 복사본 이름 -> 아무거나

- `<board>` : `jetson-agx-xavier-devkit` [링크](https://docs.nvidia.com/jetson/archives/r34.1/DeveloperGuide/text/IN/QuickStart.html#jetson-modules-and-configurations)

![image.png](\assets\img\postimg\Jetson\Jetson_BackUP01.png)


```bash
sudo ./flash.sh -r -k APP -G <clone>.img jetson-agx-xavier-devkit mmcblk0p1
```

`.conf`는 빼자.

### 생성 위치
현재 위치
두 개 생김
## 복구

### 파일 이동
Nvidia JetPack(BSP)참고
![image.png](\assets\img\postimg\Jetson\Jetson_BackUP02.png)
생성 된 `.img` 파일을 BSP로 옮김

```bash
sudo cp <clone>.img 저 위의 위치/bootloader/system.img
```

### 플래시

#### 플래시 됐었으면
```bash
sudo ./flash.sh -r -k APP jetson-agx-xavier-devkit mmcblk0p1
```

#### 안됐었으면
```bash
sudo ./flash.sh -r <board> mmcblk0p1
```