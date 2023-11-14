---
title: operator[]:C++
author: Kimec995
date: 2023-11-04 14:00:00 +09:00
categories: [C++, Basic]
tags: [TIL, C++, Basic]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/C++_IMG.png
  alt: operator[]:C++
---
> 연산자 오버로딩 함수. 배열과 같은 데이터 구조에 대한 인덱스 접근을 제공. 클래스나 사용자 정의 자료형에서 배열과 유사한 인터페이스를 제공할 때 유용함.

```c++
#include <iostream>
#include <cassert>

class MyClass {
private:
    int data[5];  // 예시로 정수형 배열을 멤버로 사용

public:
    // 배열에 대한 인덱스 접근을 위한 operator[] 함수
    int& operator[](int index) {
        assert(index >= 0 && index < 5);  // 인덱스 범위 검사
        return data[index];
    }
};

int main() {
    MyClass obj;

    // operator[]를 사용하여 배열에 값 할당
    for (int i = 0; i < 5; ++i) {
        obj[i] = i * 2;
    }

    // operator[]를 사용하여 배열 값 출력
    for (int i = 0; i < 5; ++i) {
        std::cout << obj[i] << " ";
    }

    return 0;
}
```

```bash
0 2 4 6 8 
```

위의 예제에서 `MyClass` 클래스는 정수형 배열 `data`를 멤버로 갖고 있다. `operator[]` 함수는 배열과 같이 인덱스를 받아 해당 인덱스의 요소에 접근하도록 정의되어 있다. 주의할 점은 `operator[]` 함수가 참조(`int&`)를 반환하고 있으므로, 배열 요소에 값을 할당할 수 있다.

이러한 연산자 오버로딩을 사용하면 사용자 정의 클래스를 배열처럼 다룰 수 있어 편리하게 데이터에 접근할 수 있다.