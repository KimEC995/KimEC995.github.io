---
title: PiNas_ROS_03_HDD in Raspbian
author: Kimec995
date: 2023-10-19 14:00:00 +09:00
categories: [ToyProject, PiNas]
tags: [RaspberryPi, ToyProject, NAS, Server, Developer_Kit]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/RaspberryPi_NAS/RaspberryPi_NAS_000.png
  alt: 토이프로젝트)PiNas_03_하드연결
---
# PiNas
- 하드 드라이브 설정
- 참고) 굳이 안해도 OMV쓰면 내부에서 해결 가능

- PiNas 전체 목록 [링크](https://kimec995.github.io/posts/ToyPorject_PiNas/)

> 23.10.16 작성
> 23.10.16 - 구매\
> 23.10.17 - 작성\
> 23.10.19 - 케이블 받음

---
# 3. Raspberry Pi 4 + HDD

## 1. HDD 준비하기

사용 하려는 HDD 는 SATA와 5V 450mA / 12V 700mA 전원이 필요하다. 

그리고 라즈베리 파이4 의 USB 포트는 1.1 A(최대) 까지 지원한다.

[라즈베리파이 Datasheet](https://datasheets.raspberrypi.com/rpi4/raspberry-pi-4-datasheet.pdf)

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_22.png)

생각이 부족했다.. 일반적으로 SATA to USB 케이블은 두 가지가 있다. 하나는 USB 통합, 또 하나는 외부전원 케이블이다. 외부전원을 이용하면 하나의 하드는 하나의 전원을 사용한다. 만약 동일 방법으로 3~4개의 하드를 연결하면 최소 3~4개의 전원이 필요하다. SAS컨트롤러를 사용할 수 없고, DAS는 너무 비싸고 통합 케이블은 확장이 어렵다. 기왕이면 하나의 전원에서 2~3개의 SATA 포트가 나오면 한다. 그렇다고 시중에서 파는 PiNas용 독을 구매하자니 돈은 돈대로 쓰는 것 같고 공부도 안되는 느낌이라 지양하고 싶다. 어쩔 수 없다. 지금은 우선 하나만 연결하고, 케이스 만들 때 커넥터를 같이 만들어야겠다. 전원을 묶고 포트를 만들자.

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_23.png)

인식불가문제.. 이거저거 해봤지만 하드드라이브가 너무 오래되어 물리적으로 망가졌다... 고치는데 시간이 더 걸릴 것 같으니 다른 하드로 교체.

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_24.png)

포멧하고 새 파티션 부여했다. 열어보니 Ubuntu가 들어있던데 예전에 외주할 때 썼었나?

## 2. Raspberry Pi + HDD 확인 및 포멧

파이보드에 하드 물리기.

그 전에 현재 파이보드의 마운트 상태를 확인하자.

```bash
sudo lsblk
```
![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_26.png)

그 다음 하드를 물리고 다시 확인하면

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_27.png)

잘 인식된다!
`sda` 하단에 두 개의 파티션이 인식된다. 근데 다시 생각해보니 OS사용은 다른 카드에서 하는데 파티션을 두개로 나눌 필요가 있었나? 하나로 합치자.

```bash
sudo fdisk /dev/sda
```

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_28.png)

우선 파티션 두 개를 지우고

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_29.png)

최대크기로 하나를 생성한다.

저장은 `w`. 시간이 좀 걸린다.

확인하면 

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_30.png)

하나의 하드에 하나의 파티션!

```bash
sudo blkid
```

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_31.png)

파티션의 PARTUUID는 있지만, UUID는 인식되지 않는다. 포멧하자.

```bash
sudo mkfs.ext4 /dev/sda1
```

ext4 형식으로 포멧

![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_32.png)

UUID 생성!

### 3. 마운트하기
식별자를 알았으니 파일 시스템 트리에 연결해야한다.

```bash
mkdir hdd_test
```
난 경로를 'home/pi/hdd_test' 로 정했다.

```bash
sudo mount /dev/sda1 ~/hdd_test/
```

```bash
# mount: (hint) your fstab has been modified, but systemd still uses the old version; use 'systemctl daemon-reload' to reload.
```

리로드 해야한다.

```bash
sudo systemctl daemon-reload
```

```bash
sudo mount /dev/sda1 ~/hdd_test/
```

```bash
pi@raspberrypi:~ $ df -h
# Filesystem      Size  Used Avail Use% Mounted on
# udev            3.6G     0  3.6G   0% /dev
# tmpfs           782M  1.2M  781M   1% /run
# /dev/mmcblk0p2   15G  1.8G   12G  14% /
# tmpfs           3.9G     0  3.9G   0% /dev/shm
# tmpfs           5.0M   16K  5.0M   1% /run/lock
# /dev/mmcblk0p1  510M   61M  450M  12% /boot/firmware
# tmpfs           782M     0  782M   0% /run/user/1000
# /dev/sda1       458G   28K  435G   1% /home/pi/hdd_test
```

```bash
pi@raspberrypi:~ $ sudo lsblk
# NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
# sda           8:0    0 465.8G  0 disk
# └─sda1        8:1    0 465.8G  0 part /home/pi/hdd_test
# mmcblk0     179:0    0  14.8G  0 disk
# ├─mmcblk0p1 179:1    0   512M  0 part /bo
```

마운트 완료!! 근데 재부팅 할 때마다 마운트하기 번거로우니 자동 마운트를 설정한다.

### 4. 자동 마운트- fstab

fstab 파일을 수정해 자동 마운트를 설정하자.

```bash
#!/bin/bash
pi@raspberrypi:~ $ sudo vi /etc/fstab
```
![image.png](\assets\img\postimg\RaspberryPi_NAS\RaspberryPi_NAS_33.png)

UUID와 경로, 설정을 적는다.

재부팅

```bash
sudo reboot
```

```bash
pi@raspberrypi:~ $ sudo lsblk
# NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
# sda           8:0    0 465.8G  0 disk
# └─sda1        8:1    0 465.8G  0 part /home/pi/hdd_test
# mmcblk0     179:0    0  14.8G  0 disk
# ├─mmcblk0p1 179:1    0   512M  0 part /boot/firmware
# └─mmcblk0p2 179:2    0  14.3G  0 part /
pi@raspberrypi:~ $

```

마운트 명령하지 않아도 UUID를 읽어 자동 마운트 한다!!


## 참고
[(서적)라즈베리파이로 시작하는 핸드메이드 IoT](https://product.kyobobook.co.kr/detail/S000001934230)

[라즈베리파이 공식문서](https://www.raspberrypi.com/tutorials/nas-box-raspberry-pi-tutorial/)

[라즈베리파이 HTML 웹페이지 배포](https://www.seeedstudio.com/blog/2020/06/23/setup-a-raspberry-pi-web-server-and-easily-build-an-html-webpage-m/)

[라즈베리파이 Datasheet](https://datasheets.raspberrypi.com/rpi4/raspberry-pi-4-datasheet.pdf)

[geeksvoyage님 블로그](https://geeksvoyage.com/raspberry%20pi4/hdd-for-pi4/)