---
title: C++_Pass-by-Reference
author: Kimec995
date: 2024-05-10 14:00:00 +09:00
categories: [C++, Basic]
tags: [TIL, C++, Basic]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/C++_IMG.png
  alt: C++_Pass-by-Reference
---
# Pass-by-Reference
함수 내에서 참조자로 전달할 경우, 주소를 이용해 전달하기 때문에 원래 변수의 값을 수정할 수 있다.

- 함수 내에서 값의 변환이 필요할 때 사용한다.
- 값의 변환을 위해 매개변수의 주소값이 필요함
- 배열이 아닌 경우도 Cpp에선 참조자를 통해 가능하다.

## 예시
```c++
#include <iostream>

void scale_number(int& num);

int main()
{
	int num = 1000;
	scale_number(num);
	std::cout << num << std::endl;

	return 0;
}

void scale_number(int& num)
{
	if (num > 100)
	{
		num = 100;
	}
}
```

```bash
100
```
원래 Cpp의 변수는 [[Pass-by-Value(Cpp)]] 특성을 가져 원래 변수를 수정할 수 없지만 참조자를 사용하면 원래 변수를 수정할 수 있다.