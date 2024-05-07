---
title: 라즈베리파이)GPIO Ctl) WiringPi
author: Kimec995
date: 2024-05-05 19:00:00 +09:00
categories: [RaspberryPi]
tags: [RaspberryPi, Linux, C++]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/RaspberryPi_IMG.png
  alt: 라즈베리파이)GPIO Ctl) WiringPi
---
[라즈베리) Cpp 테스트, 컴파일](https://kimec995.github.io/posts/RP_Cpp/)가 됐으니 GPIO 제어하자. 라즈베리파이의 GPIO는 `WiringPi`로 제어한다. GPIO는 [GPIO) 다용도 입출력 핀](https://kimec995.github.io/posts/GPIO/) 참고

# 환경
- 라즈베리파이 3B+
- 16GB microSD
- Raspbian 12 (bookworm)

# 주의
**라즈베리파이의 GPIO는 0.0 - 3.3V 까지의 전압 범위**를 지닌다.
만약 다른 모듈(5V를 Input하는) 을 아무 생각 없이 물리면 파이보드가 날아갈 수도 있으니 반드시 체크하자.

# 설치
## git down
```bash
sudo apt-get install git-core
```

## `wiringPi` clone
주소: https://github.com/WiringPi/WiringPi

```bash
git clone https://github.com/WiringPi/WiringPi.git

cd WiringPi

git pull origin master
```

```bash
smartfarm@raspberrypi:~ $ git clone https://github.com/WiringPi/WiringPi.git
Cloning into 'WiringPi'...
remote: Enumerating objects: 2083, done.
remote: Counting objects: 100% (958/958), done.
remote: Compressing objects: 100% (270/270), done.
remote: Total 2083 (delta 759), reused 798 (delta 666), pack-reused 1125
Receiving objects: 100% (2083/2083), 923.85 KiB | 4.64 MiB/s, done.
Resolving deltas: 100% (1392/1392), done.
smartfarm@raspberrypi:~ $ ls
Bookshelf  Codes  Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos  WiringPi
```

## `WiringPi` build
```bash
cd WiringPi

./build
```

# 사용
## 핀맵
출처: [공식홈페이지](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#raspberry-pi-3-model-b)

![image.png](..\assets\img\postimg\RPSF4DnM\RPSF_GPIO_WiringPi_01.png)

![image.png](..\assets\img\postimg\RPSF4DnM\RPSF_GPIO_WiringPi_02.png)

## 자주 쓰는 명령어들
### help
도움말
```bash
gpio -h
```
![image.png](..\assets\img\postimg\RPSF4DnM\RPSF_GPIO_WiringPi_03.png)

### readall
현재 핀 상황 읽기
```bash
gpio readall
```

![image.png](..\assets\img\postimg\RPSF4DnM\RPSF_GPIO_WiringPi_04.png)

가운데 `Physical` 줄은 물리적 표현.
현재 모드는 전부 `IN`으로 설정 되어 있다.

### 추가 중
사용 하다 좀 많이쓰는건 이후 추가하기

## 제어
### 라이브러리 호출
```cpp
# include <wiringPi.h>
```
헤더 파일, 소스 파일 위치: `설치했던 경로/WiringPi/wiringPi`

![image.png](..\assets\img\postimg\RPSF4DnM\RPSF_GPIO_WiringPi_05.png)

### 라이브러리 setup
```cpp
wiringPiSetup();
```

```cpp
/*
 * wiringPiSetup:
 *  Must be called once at the start of your program execution.
 *
 * Default setup: Initialises the system into wiringPi Pin mode and uses the
 *  memory mapped hardware directly.
 *
 * Changed now to revert to "gpio" mode if we're running on a Compute Module.
 *********************************************************************************
 */
```

오류 발생 시 값은 `-1`

## GPIO 테스트

LED를 1초 간격으로 On / Off 로 테스트 한다.
### 핀
- GND
- GPIO 2
- LED
- 1K Res

![image.png](..\assets\img\postimg\RPSF4DnM\RPSF_GPIO_WiringPi_06.png)


### 소프트웨어
우선 GPIO 번호를 찾자
```bash
gpio readall
```

```bash
+-----+-----+---------+------+---+---Pi 3B--+---+------+---------+-----+-----+
 | BCM | wPi |   Name  | Mode | V | Physical | V | Mode | Name    | wPi | BCM |
 +-----+-----+---------+------+---+----++----+---+------+---------+-----+-----+
 |     |     |    3.3v |      |   |  1 || 2  |   |      | 5v      |     |     |
 |   2 |   8 |   SDA.1 |   IN | 1 |  3 || 4  |   |      | 5v      |     |     |
 |   3 |   9 |   SCL.1 |   IN | 1 |  5 || 6  |   |      | 0v      |     |     |
```

나는 지금 BCM 2번::wPi 8번에 신호를 연결했다.

따라서 LED번호를 8로 지정.

```cpp
# include <wiringPi.h>
# include <iostream>

# define LED 8

int main()
{
    if(wiringPiSetup() == -1)
    {
        std::cout << "Status: -1" << std::endl;
        return -1;
    }

    pinMode(LED, OUTPUT);
    for(int i = 0; i < 100; i++)
    {
        digitalWrite(LED, 1);
        delay(1000);
        std::cout << "LED ON" << std::endl;
        digitalWrite(LED, 0);
        delay(1000);
        std::cout << "LED OFF" << std::endl;
    }
    return 0;
}
```

코드는 어려울 부분이 없다.

이제 Make

```bash
g++ -o LEDTEST GPIOPi.cpp -lwiringPi
```

플래그로 라이브러리의 위치도 포함하자. 아직은 외부 라이브러리가 한 개이니 그냥 `g++`로 진행.

아무튼 생성 후 확인

```bash
./LEDTEST
```

잘 깜빡이면 라이브러리 설치, 확인 완료.

### 기타
`pinMode(LED, OUTPUT)` 이후 다시 `gpio readall`을 하면 처음엔 `IN` 모드였던 wPi8번이 `OUT`으로 바뀐 걸 확인할 수 있다.

- 이전
![image.png](..\assets\img\postimg\RPSF4DnM\RPSF_GPIO_WiringPi_04.png)

- 이후
![image.png](..\assets\img\postimg\RPSF4DnM\RPSF_GPIO_WiringPi_07.png)
