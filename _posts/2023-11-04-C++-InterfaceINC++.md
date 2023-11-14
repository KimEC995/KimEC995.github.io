---
title: Interface IN C++
author: Kimec995
date: 2023-11-04 14:00:00 +09:00
categories: [C++, Basic]
tags: [TIL, C++, Basic]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/C++_IMG.png
  alt: Interface IN C++
---
> 참고: C++의 Interface 기능은 원래 공식적으론 존재하지 않았으나 C++20이후 개념(concept)이라는 새로운 기능이 추가되며 좀 더 명확하지만 제한적으로 이용할 수 있다. 따라서 이전버전의 라이브러리나 언어를 사용한다면 제약이 생긴다.

[Document](https://learn.microsoft.com/en-us/cpp/cpp/interface?view=msvc-170)

상위항목: interface, 순수 가상 함수, 다형성, 다중 상속
# Interface In C++
>C++의 인터페이스는 추상 클래스로 구현한다. 따라서 순수 가상 함수가 기본 클래스이다. 또한 다중 상속을 지원한다. 

## 특징
- 구현이 없다.
- 다중 상속을 지원한다.
- 다형성 인터페이스로 가상 소멸자와 순수 가상 함수 만 포함된다.

## 예시
```C++
__interface IMyInterface { HRESULT CommitX(); HRESULT get_X(BSTR* pbstrName); };
```

> 이 코드 조각은 C++/CLI에서 사용되는 인터페이스를 나타내고 있다. C++/CLI는 C++ 언어에 .NET 플랫폼을 지원하기 위한 확장이며, .NET의 COM 기술과 상호 작용하기 위해 사용될 수 있다.
> 
> 여기서 `__interface`는 C++/CLI에서 인터페이스를 정의하기 위한 키워드이다. `IMyInterface`는 .NET 인터페이스로, 두 개의 멤버 함수(`CommitX`와 `get_X`)를 가지고 있다.
> 
> `HRESULT CommitX();`: 이 함수는 COM 기술에서 사용되는 반환 형식인 `HRESULT`를 반환하며, `CommitX`라는 이름의 메서드이다. 이 메서드는 어떠한 인자도 받지 않는다.
> 
> `HRESULT get_X(BSTR* pbstrName);`: 또 다른 `HRESULT`를 반환하는 메서드로, `get_X`라는 이름의 메서드. 이 메서드는 `BSTR*` 형식의 포인터를 인자로 받는다. `BSTR`은 COM에서 사용되는 문자열 형식이다.
> 
> C++/CLI에서는 .NET과의 상호 작용을 위해 `HRESULT`와 같은 COM 형식을 사용할 수 있다. 이러한 인터페이스를 구현하는 클래스는 이러한 메서드들을 구현해야 하며, 해당 클래스가 이 인터페이스를 구현하고 있다는 것을 나타내기 위해 `gcnew` 등의 키워드를 사용하여 .NET 객체를 생성할 수 있다.