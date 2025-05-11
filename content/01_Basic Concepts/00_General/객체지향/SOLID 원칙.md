---
title: 객체지향) SOLID 원칙
tags:
  - OOP
---
# SOLID 원칙
> 클린 코드로 유명한 로버트 마틴 형님이 5개 원칙을 정리함.

좋은 객체 지향 설계의 5가지 원칙

- *S*RP: 단일 책임의 원칙(Single Responsibility Principle)
- *O*CP: 개방-폐쇠의 원칙(Open/Closed Principle)
- *L*SP: 리스코프 치환 원칙(Liskov Substitution Principle)
- *I*SP: 인터페이스 분리의 원칙(Interface Segregation Principle)
- *D*IP: 의존관계 역전의 원칙(Dependency Inversion Principle)

---
## SRP: 단일 책임의 원칙
> Single Responsibility Principle

#### 하나의 클래스는 하나의 책임만 지닌다.
- 하나의 책임은 모호하다
	- 클 수도, 작을 수도 있다.
	- 문맥과 상황에 따라 다르다.

- *중요한 기준은 변경*
	- 변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것
	- ex) UI 변경, 객체의 생성과 사용을 분리

---
## OCP: 개방-폐쇠의 원칙
> Open/Closed Principle

SOLID에서 1번 중요
#### 소프트웨어 요소는 확장에는 열려있으나, 변경에는 닫혀있어야 함
- 뭔말이야
	- 확장을 하려면 당연히 기존 코드를 변경해야 하는거 아님?
- [[다형성(Polymorphism)|다형성]]을 활용하자
- 인터페이스를 구현한 새로운 클래스를 하나 만들어서 새 기능을 구현
- *역할과 구현의 분리*

#### 예제
```java
// 예시 - 클라이언트가 구현 클래스를 직접 선택함

// 객체 구현 - MemberRepository를 사용한 서비스
public class MemberService{
	private MemberRepository memberRepository = new MemberRepository();
}

// 이후 객체 변경 - JdbcMemberRepository로 변경
public class MemberService{
	//private MemberRepository memberRepository = new MemberRepository();
	private MemberRepository memberRepository = new JdbcMemberRepository();
}
```

- MemberService 클라이언트가 구현 클래스를 직접 선택함
	- 기존: `private MemberRepository memberRepository = new MemberRepository();`
	- 변경: `private MemberRepository memberRepository = new JdbcMemberRepository();`
- *구현 객체를 변경하려면 클라이언트 코드를 변경해야한다..!*
	- 다형성을 이용했으나, OCP원칙을 못 지킴
- 해결방법
	- 객체를 생성하고, 연관관계를 맺어주는 *별도의 조립, 설정자가 필요*하다.
	- 엥 다형성만으로는 해결 불가하네네

---
## LSP: 리스코프 치환 원칙
> Liskov Substitution Principle

#### 프로그램 기능의 정확성을 지킴 / 부모 클래스 대체 가능
- 프로그램의 객체는 프로그램의 정확성을 깨지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 함
	- ex)
		- 자동차 인터페이스의 엑셀을 앞으로 가라는 기능
		- 뒤로 가면 LSP 위반
		- 느리더라도 앞으로 가야 함

- [[다형성(Polymorphism)|다형성]]에서의 원칙을 지키려면 LSP가 필요하다.
	- 하위 클래스는 인터페이스 규칙을 다 지켜야 함
	- 다형성 지원하기 위한 원칙
	- 인터페이스를 구현한 구현체를 믿고 사용
- *단순히 컴파일 성공의 의미가 아님*

---
## ISP: 인터페이스 분리의 원칙
> Interface Segregation Principle

#### 여러 개의 전용 인터페이스가 하나의 범용 인터페이스보다 낫다
- 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다
	- ex)
		- 자동차 인터페이스 -> 운전 인터페이스, 정비 인터페이스로 분리
		- 정비 인터페이스가 변해도 운전자 클라이언트에 영향을 주지 않음
- *인터페이스가 명확해지고, 대체 가능성이 높아진다*

---
## DIP: 의존관계 역전의 원칙
> [[Dependency]] Inversion Principle

SOLID에서 2번 중요
#### 추상화에 의존해야지, 구체화에 의존하면 안된다 / 변화가 없는 것에 의존
- [[00. 의존성 주입(Dependency Injection)]] 에 해당한다
- 구현 클래스에 의존하지 말고, 인터페이스에 의존해라.
	- 구현 클래스는 몰라도 된다..!
- 앞의 OCP에서 *역할에 의존*해야 한다는 것과 동일하다.
- 객체 사상도 Client가 인터페이스에 의존해야 *유연하게 구현체를 변경*할 수 있다.

#### 예제
위의 OCP 예제 그대로
```java
// 예시 - 클라이언트가 구현 클래스를 직접 선택함

// 객체 구현 - MemberRepository를 사용한 서비스
public class MemberService{
	private MemberRepository memberRepository = new MemberRepository();
}

// 이후 객체 변경 - JdbcMemberRepository로 변경
public class MemberService{
	//private MemberRepository memberRepository = new MemberRepository();
	private MemberRepository memberRepository = new JdbcMemberRepository();
}
```

- `MemberService`는 인터페이스에 의존하지만, 동시에 구현 클래스에도 의존함
	- `10, 11`번 줄: 직접 선택하니까
- 이는 *DIP를 위반함*

- 이를 해결하기 위해서
	- `MemberService`는 `MemberRepository`에만 의존해야 함
	- 마찬가지로 `누군가`가 *별도의 조립, 설정자가  필요*  함

---
# 결론: 다형성만으로는 SOLID원칙을 모두 지킬 수 없다

- OCP, DIP원칙을 보면 다형성 만으로는 이 두 원칙에 위배됨
- 이를 해결할 *별도의 조립, 설정자가 필요*해짐 -> [[00. 의존성 주입(Dependency Injection)|의존성 주입]]