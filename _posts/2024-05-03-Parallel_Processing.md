---
title: 병렬 처리(Parallel Processing)
author: Kimec995
date: 2024-05-03 19:00:00 +09:00
categories: [CUDA]
tags: [Jetson, Developer_Kit, CUDA]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/JetsonNano_Logo.png
  alt: 병렬 처리(Parallel Processing)
---

GPU 프로그래밍이 더 높은 성능을 보이는 이유는 CPU에 비해 많은 수의 연산코어(Porcessing Core)를 지닌 대규모 병렬처리 장치이기 때문이다. 따라서 GPU 프로그래밍은 기본적으로 병렬처리를 사용한다.

## 병렬처리의 개념
![image.png](\assets\img\postimg\Jetson\Parallel_Processing_Exp_Img.png)

여러 작업이 동시에 진행되도록 하는 것. **하나의 큰 작업을 여러 개의 작은 작업으로 나누고 동시에 처리하여 전체 실행 속도를 향상**시킨다.
일반적으로 여러 개의 프로세서 또는 스레드를 사용해 작업을 동시에 처리함.

## 병렬처리의 필요성
- **사용자 요구** : 더 빠르고 정확한 작업
	과거엔 단일코어 CPU의 성능을 높여 사용자 요구를 충족했으나, 그 한계에 봉착함. 

- **컴퓨터 아키텍처 발전 경향** : 무식하게 트랜지스터를 늘려(집적도를 높여) 효과를 보기엔 한계가 있다.
	따라서 하나의 연산 코어에 대한 집적도를 높여 하나의 칩에 많은 연산코어를 담는 전략을 취하게됨.