---
title: push_back(VectorCpp)
author: Kimec995
date: 2024-04-30 08:00:00 +09:00
categories: [C++, Basic]
tags: [TIL, C++, Basic]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/C++_IMG.png
  alt: push_back(VectorCpp)
---

상위항목:[Vector(Cpp)](https://kimec995.github.io/posts/C++_Vector/)

![image.png](\assets\img\postimg\Cpp_Basic\pushback_IMG.png)

공백 벡터에서 시작해 배열이 하나씩 추가될 때 마다 벡터의 크기를 확대한다.

**장점**
- Dynamic Array 이용이 편하다.

**단점**
- 비효율적이다.
	- 벡터의 크기가 증가한 만큼 새로운 메모리 영역을 확보해야한다.
	- 예시: 가족이 1명 늘어날 때 마다 더 큰 집으로 이사간다.
	- 보완점: 어느정도 크기가 예상되면 고정된 크기를 사용하자.