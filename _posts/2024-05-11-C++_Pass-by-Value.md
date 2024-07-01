---
title: C++_Pass-by-Value
author: Kimec995
date: 2024-05-11 14:00:00 +09:00
categories: [C++, Basic]
tags: [TIL, C++, Basic]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/C++_IMG.png
  alt: C++_Pass-by-Value
---
연관 개념: [Pass-by-Reference(Cpp)](https://kimec995.github.io/posts/C++_Pass-by-Reference/): 수정 가능한 데이터 전달
# Pass-by-Value
> 함수에 데이터를 전달할 때는 값으로 전달(pass-by-value) 됨. 이 **값은 수정불가**.

![image.png](\assets\img\postimg\Cpp_Basic\pass-by-value-img.png)

![[Pasted image 20231126172800.png]]

- 데이터는 **복사되어 전달**된다.

- 전달된 인수는 함수를 통해 변화되지 않음
	- 실수로 값 변경을 방지한다.

- 값을 변화시키는 기능이 필요하거나, 복사 비용이 높을 때를 위한 방법: **포인터, [[reference(참조자)(Cpp)]]**

```c++
#include<iostream>

void param_test(int formal)
{
	std::cout << formal << std::endl;
	formal = 100;
	std::cout << formal << std::endl;
}

int main()
{
	int actual = 50;
	std::cout << actual << std::endl;
	param_test(actual);
	std::cout << actual << std::endl;
	return 0;
}
```

```bash
50    //actual 값
50    //actual 이 들어간 formal
100   //100으로 초기화한 formal
50    //actual 값
```