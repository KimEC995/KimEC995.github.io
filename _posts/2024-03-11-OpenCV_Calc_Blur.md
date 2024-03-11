---
title: OpenCV를 이용한 카메라 화면 선명도 측정 비교
author: Kimec995
date: 2024-03-11 09:00:00 +09:00
categories: [OpenCV]
tags: [OpenCV, Linux, Jetson]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/OpenCV_Logo.png
  alt: OpenCV 선명도 측정 비교
---
[지난 포스트(링크)](https://kimec995.github.io/posts/Nvbuffer_To_Mat/) 에서 Nvbuffer를 OpenCV cv::Mat 데이터로 변환했다. 
변환은 되었는지, OpenCV의 선명도 측정 함수들 중 어떤게 가장 내 시스템에 적적한지 실험 해보자.

## 환경
- 언어: Cpp
- OS: Ubuntu 18.04
- OpenCV: 4.1.1
- L4T 32.2.3: Nvidia Xavier P2972
- e-CAM20_CUXVR 4개

# calcBlurriness

## 동작 테스트
[OpneCV calcBlurriness 링크](https://docs.opencv.org/4.1.1/d5/d50/group__videostab.html#ga527fd10de0ee99ed7585d4a7dc23c470) 의 `cv::videostab::calcBlurriness()` 를 이용한다.

우선 Ypalne만 추출한 값이 유의미한 값인지 간단하게 테스트 해보자.

```c++
/*calcBlurriness*/
float blurriness = cv::videostab::calcBlurriness(yMat);

/*출력값 확인*/
std::cout << "이미지 흐림 정도: " << blurriness << std::endl;
```
- `yMat`: [지난 포스트](https://kimec995.github.io/posts/Nvbuffer_To_Mat/)에서 생성 한 Ypalne만 추출한 Mat데이터.


출력
```bash
이미지 흐림 정도: 0.0035414
이미지 흐림 정도: 0.0036218
...
```

`0.003` - `0.0008` 사이를 움직인다. 배율 등은 이후에 조정 하고, 카메라 초점에 따라 값이 유의미하게 변화 하는 것을 확인했다.

## 선명도 측정 테스트

### 실험1. 카메라 4개를 동시에 측정하기
별 생각 없이 4개 동시에 측정 해 봤다.

![image.png](\assets\img\postimg\CV_Calc_Blur\OpenCV_Calc_Blur_001.png)

이미지에도 적혀 있지만 그래프가 들쭉 날쭉 하는건 그때 그때 손으로 돌려본거라 큰 분포 확인 용이었다.

제일 왼쪽 카메라는 전체적으로 분포가 낮고 중간 오른쪽 카메라는 분포가 높은 편 인건가? 왼쪽 카메라 값이 신경 쓰여 두 개씩 비교했다.

### 실험2. 카메라 2개 비교하기(1 vs 2)
![image.png](\assets\img\postimg\CV_Calc_Blur\OpenCV_Calc_Blur_002.png)

실험2 부터 실험 방법을 고정했다.

최대한 비슷한 시간동안 초점을 맞추고 풀었더니 그래프가 이쁘게 나온다.
2번 카메라 복구가 오래 걸린 이유는 화면이 작아서 잘 안보였다..

### 실험3. 카메라 2개 비교하기(1 vs 3)
![image.png](\assets\img\postimg\CV_Calc_Blur\OpenCV_Calc_Blur_003.png)

첫 번째 실험에서 3번 카메라의 값이 1번에 비해 지나치게 높아 걱정했으나 실험 방법을 바꾸니 안정된 모습을 보인다.
마찬가지로 카메라 3번 또한 화면이 작아 1번에 비해 결과값이 높다.

## 실험 결론.
- 두 카메라의 초점 차이를 측정하는 방법은 의미있다
- 값이 0에 가까울 수록(작을수록) 초점이 맞다.
- 직접적인 비교(ex. 1 <= 2) 는 안정된 영역에선 큰 의미가 없다.
- 아마 1번 카메라의 분포가 낮은 이유는 큰 화면이라 손으로 초점맞추기 편한 탓도 있을 것이다.
- 따라서 초점에 관한 정확한 데이터는 다음 작업 이후로 미루고 우선 동작 한다는 사실과 이후 활용을 고려할 것.

# Laplacian
[OpneCV Laplacian 링크](https://docs.opencv.org/4.1.1/d4/d86/group__imgproc__filter.html#gad78703e4c8fe703d479c1860d76429e6) 의 `cv::Laplacian()` 을 이용한다.

```c++
#include <opencv2/imgproc.hpp>

/*Laplacian*/
cv::Mat laplacian;
cv::Laplacian(yMat, laplacian, CV_64F);
cv::Scalar mean, stddev;
cv::meanStdDev(laplacian, mean, stddev);
double blurriness = stddev.val[0] * stddev.val[0];

/*출력값 확인*/
 std::cout << "이미지 흐림 정도: " << blurriness << std::endl;
```

## 선명도 측정 테스트

### 실험1. 카메라 4개를 동시에 측정하기
마찬가지로 카메라 4개를 동시에 출력했다.

![image.png](\assets\img\postimg\CV_Calc_Blur\OpenCV_Calc_Blur_004.png)

한 개씩 출력할 땐 응답률이 좋았다.

### 실험2. 카메라 2개 비교하기(1 vs 2)
![image.png](\assets\img\postimg\CV_Calc_Blur\OpenCV_Calc_Blur_005.png)

두 개씩 확인하면 응답률이 매우 떨어지고 결과 값도 안 이쁘다.

## 실험 결론
`cv::Laplacian()`을 이 환경에서 쓰긴 좀... -> 중단

---

# 최종 결론

1. `Nvbuffer2Raw()`는 잘 동작 한다.
2. OpenCV에는 다른 선명도 측정 함수들도 있지만, 시간 부족과 이미 적당한 걸 찾아서 `cv::videostab::calcBlurriness()`로 결정.