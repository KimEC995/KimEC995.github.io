---
title: 스트리밍) RaspPi FFMPEG(UDP, RTMP)
author: Kimec995
date: 2024-04-01 09:00:00 +09:00
categories: [Streaming]
tags: [RaspberryPi, FFMPEG, Linux, UDP, RTMP]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/RaspberryPi_IMG.png
  alt: 라즈베리 파이로 V4L2 WebCam을 읽고 스트리밍 하기
---
지난 포스트 들에서 환경 설정을 완료했다.

- 라즈비안 설치: [링크](https://kimec995.github.io/posts/RaspPi_OS_Raspbian_1.8.5/) 

- Windows FFMPEG 설치: [링크]

# 요구 사항
나는 데이터를 실시간 스트림 하고싶다. 

화면에서 편집 된 데이터(ex. 캡쳐화면에 그림판으로 '바보'를 적고 있는 모습)를 어떤 방법이던 전송해, Jetson에서 실시간으로 받아 Jetson 내부에서 처리, 출력을 하고싶다. 그러기 위해 이런 저런 방법들을 찾고 있고 이건 그것들 중 일부만 정리해서 올리는 포스트이다.

왜 Jetson의 Camera, HDMI로 안받고 굳이 귀찮게 네트워크에서 받느냐. 그 이유는
1. 이미 카메라 모듈 Input을 다 쓰고 있다.
2. USB Input도 다 썼다.
3. HDMI 인코더가 비싸서.
4. 대부분 개발키트가 그렇지만 HDMI단자는 Output이다.

라는 이유들로 기존의 Input 포트들을 줄이지 않으면서 데이터를 수신하도록 한다.
여러 방법들 중 이번 포스트에선 라즈베리 파이 + Windows에서 FFMPEG을 이용 해볼 예정이다.

테스트 버전으로 순서는
1. 라즈베리 파이 내부의 `.mp4`데이터를 송신
2. Windows에서 수신
3. 1,2가 만족스럽게 동작한다면 이후 2를 Jetson 테스트.
4. 3까지 만족스럽게 동작한다면 [HDMItoCSI](https://www.devicemart.co.kr/goods/view?no=14420262) 모듈을 이용해 외부 화면 입력.

프로토콜: UDP, RTMP

을 예상했었다. 

결론부터 말하면 3번부터 별로여서 폐기. 하지만 이후 사용 가능성이 남아 기록한다.

# 환경
- Transmitter: Raspberry Pi 3 Model B V1.2
- Reciver: Windows 10 PowerShell
- Raspbian v1.8.5
- FFMPEG 4.3.6
- RaspPi와 Windows는 동일 네트워크에 연결 됨.

# FFMPEG 설치, 확인
Raspbian v1.8.5 기준 FFMPEG의 FFServer 를 제외하고 나머지는 설치 완료되었다.
어차피 지금은 FFServer를 사용하지 않으니 넘어간다.

```bash
ffmpeg -version
```

```bash
pi@raspberrypi:~ $ ffmpeg -version
ffmpeg version 4.3.6-0+deb11u1+rpt5 Copyright (c) 2000-2023 the FFmpeg developers
built with gcc 10 (Raspbian 10.2.1-6+rpi1)
configuration: --prefix=/usr --extra-version=0+deb11u1+rpt5 --toolchain=hardened --incdir=/usr/include/arm-linux-gnueabihf --enable-gpl --disable-stripping --enable-avresample --disable-filter=resample --enable-gnutls --enable-ladspa --enable-libaom --enable-libass --enable-libbluray --enable-libbs2b --enable-libcaca --enable-libcdio --enable-libcodec2 --enable-libdav1d --enable-libflite --enable-libfontconfig --enable-libfreetype --enable-libfribidi --enable-libgme --enable-libgsm --enable-libjack --enable-libmp3lame --enable-libmysofa --enable-libopenjpeg --enable-libopenmpt --enable-libopus --enable-libpulse --enable-librabbitmq --enable-librsvg --enable-librubberband --enable-libshine --enable-libsnappy --enable-libsoxr --enable-libspeex --enable-libsrt --enable-libssh --enable-libtheora --enable-libtwolame --enable-libvidstab --enable-libvorbis --enable-libvpx --enable-libwavpack --enable-libwebp --enable-libx265 --enable-libxml2 --enable-libxvid --enable-libzmq --enable-libzvbi --enable-lv2 --enable-omx --enable-openal --enable-opencl --enable-opengl --enable-sdl2 --enable-mmal --enable-neon --enable-rpi --enable-v4l2-request --enable-libudev --enable-epoxy --enable-pocketsphinx --enable-libdc1394 --enable-libdrm --enable-vout-drm --enable-libiec61883 --enable-chromaprint --enable-frei0r --enable-libx264 --enable-shared --libdir=/usr/lib/arm-linux-gnueabihf --cpu=arm1176jzf-s --arch=arm
libavutil      56. 51.100 / 56. 51.100
libavcodec     58. 91.100 / 58. 91.100
libavformat    58. 45.100 / 58. 45.100
libavdevice    58. 10.100 / 58. 10.100
libavfilter     7. 85.100 /  7. 85.100
libavresample   4.  0.  0 /  4.  0.  0
libswscale      5.  7.100 /  5.  7.100
libswresample   3.  7.100 /  3.  7.100
libpostproc    55.  7.100 / 55.  7.100
```

Windows의 FFMPEG설치는 [링크](작성하기) 확인.

# 테스트
테스트는 라즈베리파이, Windows 각자의 IP 확인 후 진행한다.

## UDP

### 송신
라즈베리 파이에서
```bash
ffmpeg -i mp4TestVideo.mp4 -c:v libx264 -preset ultrafast -tune zerolatency -f mpegts udp://수신 주소:동일포트
```

- `-v verbose`: 출력 로그 보여주기
- `-i 파일이름`: 보내는 파일. `.mp4`, `.ts`  둘 다 되는 걸 확인함.
- `-c:v libx264`: 코덱 형식
- `-preset ultrafast -tune zerolatency` 빠르게
- `udp:// 수신 주소:포트`: IP는 수신 측 IP와 사용하지 않는 포트를 사용한다.


### 수신
Windows PowerShell에서 

```powershell
ffplay udp://송신 주소:동일포트
```

- `ffplay`: ffmpeg에서 지원하는 출력기.
- `udp:// 송신 주소:포트`: 송신 측 IP. 포트는 동일하다
	- 만약) 포트 번호 오류: 10048 -> 이미 사용 중 인 포트 [소켓오류링크](https://learn.microsoft.com/ko-kr/windows/win32/winsock/windows-sockets-error-codes-2)


## 결과
송신 후 3-10초의 딜레이가 있다. 프레임 도달 타이밍에 따라 다르다.

![image.png](\assets\img\postimg\Streaming\Streaming_001.png)

만약 프레임이 도달하지 않았다면 메시지는
```powershell
    Last message repeated 1 times
[h264 @ 000002908723a980] decode_slice_header error
[h264 @ 000002908723a980] non-existing PPS 0 referenced
[h264 @ 000002908723a980] decode_slice_header error
[h264 @ 000002908723a980] non-existing PPS 0 referenced
[h264 @ 000002908723a980] decode_slice_header error
[h264 @ 000002908723a980] non-existing PPS 0 referenced
[h264 @ 000002908723a980] decode_slice_header error
[h264 @ 000002908723a980] no frame!
[h264 @ 000002908723a980] non-existing PPS 0 referenced  0B f=0/0
```

이와 같다.

솔직히 UDP 프로토콜을 이용한건 '그래도 TCP보단 빠르겠지~ RTMP까지 쓸일이 있을까~' 싶었던건데 역시 격질 못해 일어난 판단 미스였다. 최소 3초 딜레이는 참을 수 없으니 RTMP 프로토콜을 이용해 본다.

## RTMP

## 결과
UDP보다는 빠르다. 최소 1초 - 2초.
하지만 1초 미만의 결과를 기대했는데, 그 이상이 나오면 좀 실망스럽다.


# 최종 결론
두 방법 모두 예상과는 다르다. 이제 두 가지 선택지가 있는데,

1. RTSP 프로토콜 사용하기.
2. 캡쳐 인코더 -> V4L2 사용하기.

캡쳐 인코더는 추가 결제가 필요하니, 다음 포스트에서는 1번 방법을 사용 해 본다.