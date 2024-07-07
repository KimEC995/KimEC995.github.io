---
title: C++_Reference
author: Kimec995
date: 2024-05-12 14:00:00 +09:00
categories: [C++, Basic]
tags: [TIL, C++, Basic]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/C++_IMG.png
  alt: C++_Reference
---
# Reference(참조자)
변수는 할당된 메모리 공간에 붙여진 이름이다. **이 공간에 다른 이름을 붙이는 것이 참조자 이다.** 일종의 별명개념. 
[Pointer-포인터(Cpp)](https://kimec995.github.io/posts/C++_Pointer/)와 달리 별개의 주소값도, 메모리 공간도 차지하지 않는다.
`&`를 사용한다.

```c++
#include <iostream>

int main()
{
	int num1 = 100;
	int &num2 = num1;

	std::cout << num1 << std::endl;
	std::cout << num2 << std::endl;

	std::cout << &num1 << std::endl;
	std::cout << &num2 << std::endl;

	return 0;
}
```

```bash
100
100
00CFFC38
00CFFC38
```

# Pass-by-Reference
자세한 내용: [Pass-by-Reference(Cpp)](https://kimec995.github.io/posts/C++_Pass-by-Reference/)
참조자를 이용해 수정 가능한 값을 함수로 전달할 수 있다.
만약 복사 비용이 높거나 데이터 수정이 필요한 경우 사용한다.

# Swap 예제
Cpp의 swap 함수를 구현할 수 있다. `#include <algorithm>` 중 `std::swap()` 을 이용한다.
	참조자를 사용해 두 값을 교환하자.

```c++
#include <iostream>

void swap(int &a, int&b);

int main()
{
	int x = 10, y = 20;
	std::cout << x << " " << y << std::endl;
	swap(x, y);
	std::cout << x << " " << y << std::endl;

	return 0;
}

void swap(int &a, int &b)
{
	int temp = a;
	a = b;
	b = temp;
}
```

```bash
10 20
20 10
```

만약 참조자를 사용하지 않았다면
```bash
10 20
10 20
```
이었을 것이다.