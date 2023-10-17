---
title: Ubuntu update와 upgrade의 차이
author: Kimec995
date: 2023-10-17 17:00:00 +09:00
categories: [Ubuntu, Concept]
tags: [Concept, Ubuntu, Linux]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/Ubuntu/Ubuntu_Logo.png
  alt: Ubuntu update와 upgrade의 차이
---

Ubuntu 사용 중

```bash
sudo apt-get update
```

와

```bash
sudo apt-get upgrade
```

를 사용했다. 이 둘은 무슨 차이일까?

## apt-get 명령어

패키지 관리 명령어.

apt(Advanced Package Tool) 는 리눅스 파생 배포판들의 패키지를 설치/업데이트/삭제 등 명령한다. 즉 소프트웨어 관리 명령어.

뒤에 여러가지 명령어가 붙는다.

## update

원문

**`update`**

`update is used to resynchronize the package index files from their sources. The
indexes of available packages are fetched from the location(s) specified in
/etc/apt/sources.list. For example, when using a Debian archive, this command
retrieves and scans the Packages.gz files, so that information about new and updated
packages is available. An update should always be performed before an upgrade or
dist-upgrade. Please be aware that the overall progress meter will be incorrect as the
size of the package files cannot be known in advance.`

패키지의 리스트를 소스와 동기화 한다. 소스의 리스트는 항상 최신화 되어있으며 위치는 /etc/apt/sources.list. 이다.

**upgrade** 이전에 명령해야 해당 명령이 최신 버전을 받을 수 있다.

install 하지는 않는다.

## upgrade

원문

**`upgrade`**

`upgrade is used to install the newest versions of all packages currently installed on
the system from the sources enumerated in /etc/apt/sources.list. Packages currently
installed with new versions available are retrieved and upgraded; under no
circumstances are currently installed packages removed, or packages not already
installed retrieved and installed. New versions of currently installed packages that
cannot be upgraded without changing the install status of another package will be left
at their current version. An update must be performed first so that apt-get knows that
new versions of packages are available.`

이전에 설치한 프로그램들을 리스트와 대조해 최신 버전으로 install 한다.

---

예를 들어 리눅스에서 파이썬 버전 100이 나오고, 내 파이썬이 1인 경우 최신버전을 불러오려면

1. 업데이트

2. 업그레이드

해야 올바르게 설치가 가능하다는 것이다.


## 출처
[Ubuntu documentation](https://manpages.ubuntu.com/manpages/xenial/man8/apt-get.8.html)