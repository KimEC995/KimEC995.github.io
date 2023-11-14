---
title: C++_Namespace
author: Kimec995
date: 2023-10-21 14:00:00 +09:00
categories: [C++, Basic]
tags: [TIL, C++, Basic]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/C++_IMG.png
  alt: C++_Namespace
---

코드 그룹화.

**`::`** (연산자) Scope resolution Operator
``` c++
네임::함수명
```
### 1.
```c++
namespace A
{
    void function()
    {
        int function1 = 1;
        std::cout << function1;
    }
}
namespace B
{
    void function()
    {
        int function2 = 2;
        std::cout << function2;
    }
}
int main()
{
  A::function();
  B::function();
  return 0;
}
```

```bash
12
```
### 2.
```c++
namespace A
{
    void function()
    {
        int function1 = 1;
        std::cout << function1;
    }
}
namespace B
{
    void function()
    {
        int function2 = 2;
        std::cout << function1;
    }
}
using namespace A;
int main()
{
  function();
  return 0;
}
```

```bash
1
```
## 참고

[The Cherno : C++](https://www.youtube.com/playlist?list=PLlrATfBNZ98dudnM48yfGUldqGD0S4FFb)

