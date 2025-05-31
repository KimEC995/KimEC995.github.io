---
title: 네트워크) HSRP - 게이트웨이 이중화
tags:
  - Network
  - CISCO
---
연관: [[게이트웨이]], [[네트워크 고가용성 - HA|네트워크 고가용성]]

여기선 [[CISCO]]의 게이트웨이 이중화 기술 - HSRP에 관한 내용만 적는다.

# HSRP
> Hot Standby Router Protocol

- **프로토콜**: [[IP - Internet Protocol|IP]]
- **관련 계층**: [[OSI 7 Layer & TCP,IP 4 Layer#네트워크 계층|네트워크 계층(L3)]]

일반적인 네트워크에선 [[게이트웨이]]를 하나로 설정한다.

![[HSRP(1).png]]

위 이미지와 같이 하나의 L2 스위치 아래에 (이미지에선 2개지만) 2,000개의 PC가 연결되어 있다고 가정하자. 그러던 중 천재지변으로 인해 우리 동네 [[게이트웨이]](를 수행하는 [[Router|라우터]])가 빠샤 되어버리면? 그 사이 인터넷도 사용할 수 없고, 그 손실이 얼마나 크겠는가?

그래서 [[네트워크 고가용성 - HA|네트워크 고가용성]]을 위해 게이트웨이를 이중화 하기도 한다.

![[HSRP(2).png]]

이렇게 라우터를 두 개나 구비해서 물리적으로 연결한 후 스위치에 통하는 게이트웨이를 모두 오른쪽에만 설정하면 그건 게이트웨이 이중화가 아니다. 그냥 라우터가 하나 노는거지. 게이트웨이 이중화를 하기 위해 물리적인 연결 뿐만 아니라 **논리적인 연결**도 필요하다. 이 때 HSRP를 통해 논리적 게이트웨이를 생성해 두 개의 라우터에서 이 주소에 접근할 수 있다.

---
## HSRP의 용어
> HSRP Terms

- **활성 라우터**(Active Router)
	- 주로 게이트웨이를 수행하는 라우터
	- 가상 IP 주소를 소유하고, 트래픽을 실제로 전달한다.
	- 대기 라우터에게 주기적으로 `Hello` [[패킷]]을 보내 자신의 상태를 알린다.

- **대기 라우터**(Standby Router)
	- 활성 라우터의 백업. 
	- 주기적으로 `Hello` 패킷을 받아 활성 라우터의 상태를 체크한다.
	- 활성 라우터가 다운되었을 때 자신이 게이트웨이를 수행한다.

- **HSRP 그룹**(Group)
	- 라우터들의 여러 인터페이스를 묶기 위해 사용하는 번호.
	- 같은 그룹 내에서 활성과 대기가 나뉜다.

- **가상 IP 주소**(Virtual IP Address)
	- 논리적 주소.
	- 같은 그룹 내의 라우터들은 이 가상 주소를 이용해 같은 게이트웨이 역할을 담당한다.

- **우선 순위**(Priority)
	- 활성과 대기 상태를 구분하는 순위. 
	- 높을 수록 활성 라우터에 가깝다.
	- Priority를 직접 부여하거나, [[IP - Internet Protocol#IP의 주소|IP 주소]]가 높은 라우터가 활성 라우터로 설정된다.

- **트래킹**(Tracking)
	- 라우터의 상태를 모니터링하여 상태가 변화했을 때 우선 순위를 변동한다.

- **선출**(Preemption)
	- 활성 <-> 대기 상태를 강제로 탈취한다.

#### HSRP의 상태
> HSRP State

- **Initial State**
	- 시작 상태. 아직 동작하지 않는다.
	- 설정 변경, 인터페이스 생성 / 부활시 이 상태로 돌입한다.

- **Listen State**
	- 가상 IP 주소가 결정된 상태. 
	- 하지만 활성 / 대기 라우터는 선출되지 않았다.
	- 이 때 `Hello` 패킷을 송수신 하며 라우터 선출에 돌입한다.

- **Speak State**
	- 주기적으로 `Hello` 패킷을 전송하는 상태.

- **Active State**
	- 활성 라우터로 선출 됨

- **Standby State**
	- 대기 라우터로 대기 중

---
## HSRP 우선 순위 정책 - 경계값 기피
HSRP는 **우선 순위**에 따라 `활성 라우터(Active)`와 `대기 라우터(Standby)`로 구분한다. 우선 순위가 가장 높은 라우터가 Active 상태로, 그 외 라우터들은 Standby 상태로 대기한다.

이 때 우선 순위는

```
Router(config-if)#standby 1 priority ?
	<0-255> Priority value
```

기본 값은 `100` / 부여 가능한 번호는 0 부터 255 까지이다.

그렇다면 활성 라우터는 110으로 설정하면 될까? 그건 아니다. 만약 활성 라우터가 다운되고 대기 라우터가 Active로 변경되면 

![[HSRP(3).png]]

대기하던 라우터는 `Tracking`을 통해 활성화 된다. 이 때 기존의 라우터 `priority`값은

```
// Active 라우터의 값 변화 - 다운 전
Router(config)#do sh standby
...
	State is Active
	...
	Priority 101 (configured 101)
...

// Active 라우터의 값 변화 - 다운 중
%HSRP-6-STATECHANGE: FastEthernet0/0 Grp 1 state Standby -> Active

Router(config)#do sh standby
...
	State is Standby
	...
	Priority 91 (configured 101) <------- 주목!
...

// Active 라우터의 값 변화 - 복구 후
Router(config)#do sh standby
...
	State is Active
	...
	Priority 101 (configured 101)
...
```

으로 다운되면 16번 줄 처럼 `-10`하게 된다. 

만약 ACT 라우터를 `110`으로 설정한 뒤 다운되어 `100`으로 바뀌게 되면 다른 라우터들과 동일한 `100`(Default) 값을 가지게 된다.

**따라서 큰 이유가 없다면 `110` 등 경계값을 가지지 않도록 설정한다.**

---
# HSRP 기본 구현

![[HSRP(4).png]]

간단하게 게이트웨이 이중화를 구현했다.

- 사전 작업
	- 환경: [[00_CISCO Packet Tracer|CISCO Packet Tracer v 8.2.2]]
	- 전체 라우팅 프로토콜: [[EIGRP]]

- HSRP 정책
	- 가상 게이트웨이 IP: `192.168.10.100`
	- 활성 라우터 -> 오른쪽 -> ACT 라우터로 표기
	- 대기 라우터 -> 왼쪽 -> STN 라우터로 표기

> [!note] HSRP 인터페이스
> 참고로 HSRP는 모두 게이트웨이를 수행하는 인터페이스, 즉 이미지에선 `Fa0/0`에서 수행한다.

## 라우터 설정(공통)
ACT / STN 라우터 모두 동일한 설정을 한다. 이후 이 둘은 우선순위를 이용해 구분한다.

#### IP 부여
가장 먼저 가상 게이트웨이 IP를 부여한다.

```
standby [HSRP 그룹 번호] [가상 게이트웨이 IP]
```

```
Router(config-if)#standby 1 ip 192.168.10.100
```

#### 선점 선언
그 다음에는 [[00_프로세스 스케줄링 알고리즘#선점형 방식|선점형 스케줄링]] 설정을 위해 `preempt` 선언한다. 

선점형 선언을 하는 이유는 만약 ACT 라우터가 다운될 경우 STN 라우터가 게이트웨이 역할을 수행하는데, 이후 ACT 라우터가 복구되면 다시 ACT 라우터가 **게이트웨이 역할을 탈환**하기 위해 필요하다.

```
standby [HSRP 그룹 번호] preempt
```

```
Router(config-if)#standby 1 preempt
```

#### 트랙 설정
라우터의 게이트웨이를 수행하는 인터페이스가 다운 되는건 HSRP가 인식하지만, 그 외 인터페이스의 상태는 알 수 없다. 

위의 이미지같은 경우 활성/대기 라우터로 들어오는 **외부 라우터가 다운되어도 HSRP는 알 수 없으며** 이를 알리기 위해 트랙을 설정한다.

```
standby [HSRP 그룹 번호] track [인식 할 인터페이스]
```

```
Router(config-if)#stan 1 tra s0/0/0
```

#### PC의 게이트웨이 설정
IP는 뭐 정적을 하던 [[DHCP]]를 하던 상관 없지만, PC들의 게이트웨이는 위에서 설정한 가상 IP 주소를 부여한다.

![[HSRP(5).png]]

#### HSRP 상태 확인
```
show standby
```

```
Router(config-if)#do sh stan

FastEthernet0/0 - Group 1
  State is Active
    5 state changes, last state change 00:00:18
  Virtual IP address is 192.168.10.100
  Active virtual MAC address is 0000.0C07.AC01
    Local virtual MAC address is 0000.0C07.AC01 (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 1.591 secs
  Preemption enabled
  Active router is local
  Standby router is 192.168.10.1
  Priority 101 (configured 101)
    Track interface Serial0/0/0 state Up decrement 10
  Group name is hsrp-Fa0/0-1 (default)
```

---
## 활성 / 대기 라우터 분류
이후 HSRP 정책에 따라 우선 순위가 부여된다.

우선 순위를 직접 기입하지 않으면, 라우터들 중 IP 주소가 가장 높은 라우터가 활성 라우터로 선출된다. 정책 실현을 위해 직접 기입하자.

#### 우선 순위 부여
일반적으로 ACT 라우터는 더 높은 번호로, STN 라우터는 기본값을 활용한다.

```
standby [HSRP 그룹 번호] priority [우선순위 번호]
```

```
Router(config-if)#st 1 pri 101
```

---
# 응용 - HSRP의 그룹 분류

아래의 이미지와 같이 두 회사가 있고, 하나의 엄청 비싼 **라우터를 공용으로 사용하되 게이트웨이를 나누고 싶다.**

![[HSRP(6).png]]

이 때 그룹을 이용해 게이트웨이를 분리할 수 있다.

- 전체 라우팅 프로토콜: [[EIGRP]]
	- 단, 왼쪽과 오른쪽은 다른 회사이기 때문에 EIGRP의 그룹을 달리한다.

- HSRP 정책
	- 가상 게이트웨이 IP: `192.168.20.100`
	- 활성 라우터 -> 왼쪽 -> ACT 라우터로 표기
	- 대기 라우터 -> 오른쪽 -> STN 라우터로 표기

---
## HSRP 적용(그룹 변경)
나머지는 상동. 번호만 다르기 때문에 빠르게 간다.

```
standby [HSRP 그룹 번호] 명령어들
```

```
Router(config-if)#sta 2 ip 192.168.20.100

Router(config-if)#sta 2 pre

Router(config-if)#sta 2 pri 101

Router(config-if)#sta 2 tra s0/0/1
```

#### 확인(가운데 라우터)
가운데 라우터는 그룹 1과 그룹 2가 모두 모여있어 하나의 라우터에서 확인이 가능하다.

```
Router(config-if)#do sh stan

FastEthernet0/0 - Group 1
  State is Active
    5 state changes, last state change 00:00:18
  Virtual IP address is 192.168.10.100
  Active virtual MAC address is 0000.0C07.AC01
    Local virtual MAC address is 0000.0C07.AC01 (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 1.767 secs
  Preemption enabled
  Active router is local
  Standby router is 192.168.10.1
  Priority 101 (configured 101)
    Track interface Serial0/0/0 state Up decrement 10
  Group name is hsrp-Fa0/0-1 (default)

FastEthernet0/1 - Group 2
  State is Active
    6 state changes, last state change 00:44:51
  Virtual IP address is 192.168.20.100
  Active virtual MAC address is 0000.0C07.AC02
    Local virtual MAC address is 0000.0C07.AC02 (v1 default)
  Hello time 3 sec, hold time 10 sec
    Next hello sent in 1.902 secs
  Preemption enabled
  Active router is local
  Standby router is 192.168.20.2
  Priority 101 (configured 101)
    Track interface Serial0/0/1 state Up decrement 10
  Group name is hsrp-Fa0/1-2 (default)
```