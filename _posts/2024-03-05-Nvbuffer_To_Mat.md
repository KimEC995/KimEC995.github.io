---
title: Nvbuffer를 OpenCV에서 사용하기
author: Kimec995
date: 2024-03-05 14:00:00 +09:00
categories: [Jetson]
tags: [Jetson, Linux, OpenCV]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/JetsonNano_Logo.png
---
## 환경
- 언어: Cpp
- OS: Ubuntu 18.04
- OpenCV: 4.1.1
- L4T 32.2.3

## 요구 사항
Jetson의 영상처리는 EGL의 Nvbuffer를 통해 이루어진다.
해당 버퍼는 벡터 배열로 생성되었으며 색상 타입에 따라 배열의 형태가 바뀐다.

YUV444 형태로 버퍼를 생성하고 있으며, 버퍼 생성 후 OpneCV를 이용해 데이터 가공을 하려 한다.

현재 문제는

1. OpenCV의 Mat데이터는 YUV로 생성 후 가공이 어렵다.
2. 버퍼를 Mat데이터로 직접 변경이 어렵다.
3. YUV를 Mat으로 변경한 후 다시 GrayScale로 변경해야 하는데(혹은 반대), 리소스 소모가 심하다.

따라서 버퍼에서 Raw형식으로 변환 해 사용하려고 한다. 이 때 Y데이터만 추출할 수 있다면 GrayScale로 변환하는 수고를 덜 수 있다.

---

# Nvbuffer2Raw

[Nvidia Documents 링크](https://docs.nvidia.com/jetson/l4t-multimedia/group__ee__nvbuffering__group.html#ga4dc119baf7b91f212a326cd397446af6)

```cpp
int NvBuffer2Raw(int dmabuf_fd
                ,unsigned int plane
                ,unsigned int out_width
                ,unsigned int out_height
                ,unsigned char * ptr 
                )	
```
찾아보던 중 버퍼를 Raw데이터로 출력하는 함수를 찾았다.

## OpenCV 용 변환하기.

### 1. 빈 YPlane 배열 포인터 생성하기
```cpp
// 메모리 생성
int create_empty_Yplane()
{
    ...

    YPlaneArrPtr = (unsigned char *)malloc(WIDTH * HEIGHT);
    if (YPlaneArrPtr == nullptr) 
    {
        std::cerr << "YplanearrPtr Malloc failed." << std::endl;
        return 1;
    }
    YPlaneArrMat.create(HEIGHT, WIDTH, CV_8UC1);
    return 0;
}

// 메모리 파괴
void handle_exit()
{
    free(YPlaneArrPtr);

    ...
}

...

int main(int argc, char **argv)
{
  다른 함수들;
  create_empty_Yplane();
  다른 함수들;
  handle_exit();
}
```
- `unsigned char`: 부호 없음 8비트 -> 1바이트 단위
- `*YPlaneArrPtr`: 포인터
- `(unsigned char *)`: 타입 캐스팅 -> malloc은 보통 void*(보통 포인터 타입)를 반환하기 때문.
- `malloc()`: 동적 메모리 할당
- `WIDTH * HEIGHT`: 스트림 크기. 배열의 크기 만큼 만들어야 한다.

단 한번만 실행되면 되기 때문에 main에서 생성, 함수 본문, 파괴 순으로 작성했다.
어플리케이션이 시작 하면 생성, 사용, 종료되면 파괴한다.

### 2. Raw 데이터 추출
```cpp
/*NvBuffer2Raw 함수로 NV버퍼(m_dmabufs)를 raw로 생성*/
NvBuffer2Raw(m_dmabufs, 0, WIDTH, HEIGHT, YPlaneArrPtr);
```
- `m_dmabufs`: 버퍼 이름
- `0`: 몇 번 plane인가? -> 현재 YUV타입이니 보통 0번은 Y plane -> 테스트 결과 0번 plane외엔 QR인식이 안됨 -> Y 추출
- `WIDTH, HEIGHT`: 스트림 크기. 배열의 크기 만큼 만들어야 한다.
- `YPlaneArrPtr`: output

함수 본문에서 매 루프마다 실행한다.
화면이 바뀌면 배열도 바뀌어야 하니..

### 3. Mat 데이터 생성
```cpp
/* yplane 데이터 Mat */
YPlaneArrMat = cv::Mat(HEIGHT, WIDTH, CV_8UC1, YPlaneArrPtr);
```
- `WIDTH, HEIGHT`: 스트림 크기. 배열의 크기 만큼 만들어야 한다.
- `CV_8UC1`: 컬러 타입. 지금은 YUV의 Y하나만 사용하니 C1이다.
- `YPlaneArrPtr`: input

다시 2차원 배열로 바꿔준다. 마찬가지로 본문.

### 4. 이후 OpneCV 사용.
이래저래 잘 쓴다.

## 테스트

- 선명도 측정: [링크](https://kimec995.github.io/posts/OpenCV_Calc_Blur/)