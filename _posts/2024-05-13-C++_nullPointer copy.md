---
title: C++_null Pointer
author: Kimec995
date: 2024-05-13 14:00:00 +09:00
categories: [C++, Basic]
tags: [TIL, C++, Basic]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/C++_IMG.png
  alt: C++_null Pointer
---
> 임의 메모리 주소(쓰레기값)을 가리키는 상태가 아닌, **아무것도 가리키지 않는 상태**를 의미한다.

포인터 자료형의 기본값. 기존의
```c++
int* int_ptr = 0;    // int 타입 변수인지 Ptr변수인지 `*`을 봐야함

int* int_ptr = nullptr;    //Ptr 변수구나~
```
보다 가독성이 좋다.(직관적이다)
### 예시
```c++
int* int_ptr = nullptr;
```