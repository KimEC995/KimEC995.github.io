---
title: NVCC Compile WorkFlow
author: Kimec995
date: 2024-05-04 19:00:00 +09:00
categories: [CUDA]
tags: [Jetson, Developer_Kit, CUDA]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/JetsonNano_Logo.png
  alt: NVCC Compile WorkFlow
---

CUDA는 `.nvcc` 로 컴파일된다. [CUDA 프로그램의 구조와 흐름](https://kimec995.github.io/posts/CUDA_00_03/) 에 따르면 `.c`, `.cpp`와 `.cu` 가 동시에 컴파일 되는 모습으로 보이는데 사실일까.

# 컴파일 전체 플로우

**요약**
- NVCC는 `.c`, `.cpp`, `.cu` 컴파일 가능하다.
- Host(`.c`, `.cpp`) 와 Device(`.cu`) 코드를 분리해 컴파일한다.

- Device -> assembly form(PTX code) -> binary form(cubin object)
- Host -> CUDA C runtime -> + Device에서 온거 -> 컴파일
- 위의 Device 흐름을  just-in-time compilation(JIT컴파일) 이라고 부른다.

- 응용프로그램은 생성된 Host 코드와 연결되거나 또는 무시될수 있다.
- 연결되었을 경우 CUDA driver API를 이용하여 Device에 PTX code/cubin object 를 로드하고 실행시킨다.

## Nvidia 공식 소개
[NVCC소개](https://docs.nvidia.com/cuda/cuda-compiler-driver-nvcc/index.html)

![image.png](\assets\img\postimg\Jetson\Compile_01.png)

공식 소개페이지에서 설명하는 모습은 이렇다. 정리하면

1. 입력 프로그램은 Device 코드의 컴파일을 위해 전처리되고 CUDA 바이너리( `cubin`) 및/또는 **PTX 중간 코드로 컴파일되어 fatbinary에 배치**된다(초록상자)

2. 입력 프로그램은 Host 컴파일을 위해 다시 한 번 전처리되고 합성되어 fatbinary를 포함하고 **CUDA 특정 C++ 확장을 표준 C++ 구성으로 변환**한다.(검정점선)

3. C++ 의 Host 컴파일러는 내장된 fatbinary가 포함된 합성된 Host 코드를 Host 개체로 컴파일한다.(회색상자)

솔직히 무슨소린지 모르겠으나 공식홈페이지라 이 내용을 지울수도 없고.. 더 자세한 설명을 찾아보았다.

## 비공식소개
[링크](https://www.mdpi.com/2076-3417/11/20/9434)

![image.png](\assets\img\postimg\Jetson\Compile_02.png)

좌) 플로우    /    우) 런타임 시 CUDA 프로그램의 실행 및 메모리 전송그림

1. 단일 소스 프로그램은 먼저 Host(CPU)와 Device(GPU) 코드로 **구분**된다.

2. Host 코드는 표준 C/C++ 코드이고 Device 코드는 CUDA 키워드로 지정된 커널이라는 데이터 병렬 함수로 작성된다.

3. 일반적인 C/C++ 컴파일러는 Host 코드를 컴파일하고, Device 코드는 먼저 CUDA의 명령어 세트 아키텍처인 PTX(Parallel Thread Execution) 중간 코드로 변환된다.

4. 이후 Device 코드는 바이너리로 컴파일된다.

이걸 좀 더 단순화 하면 이런 모습이 된다.

![image.png](\assets\img\postimg\Jetson\Compile_03.png)

요약하면 프로그램은 Host와 함께 시작되고 Host가 커널을 시작하면 Device의 수천 개의 스레드에 의해 실행한다.