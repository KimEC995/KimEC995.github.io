---
title: C++_iostream-INOUT
author: Kimec995
date: 2023-11-03 14:00:00 +09:00
categories: [C++, Basic]
tags: [TIL, C++, Basic]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/C++_IMG.png
  alt: C++_iostream-INOUT
---
## IN / OUT
### cin
scanf. `cin` `>>` `txt`
### cout
printf. `cout` `<<` `txt`
### 사용
줄줄이 소세지 마냥 붙여도된다. `%d` 등 사용X

```c++
#include <iostream>
int main()
{
    int Age;
    float Hig;
    std::cout << "나이(만), 엔터, 키(소수점), 엔터 \n";
    std::cin >> Age >> Hig;
    std::cout << "나이: " << Age << "\n" << "키: " << Hig << std::endl;
  return 0;
}
```

```bash
나이(만), 엔터, 키(소수점), 엔터
28
164.5
나이: 28
키: 164.5
```