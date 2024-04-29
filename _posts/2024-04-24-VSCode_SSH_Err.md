---
title: Error) Visual Studio SSH Remote 업데이트 문제
author: Kimec995
date: 2024-04-24 09:00:00 +09:00
categories: [VSCode, Error]
tags: [Error, Network]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/999_ETC/VSCode.png
  alt: Visual Studio SSH Remote 업데이트 문제
---
블로그 글은 많이 늦었지만..

24.02.03 기준 SSH Remote가 업데이트했었다.

![image.png](\assets\img\postimg\999_ETC\VSCODE_SSH_Err_01.png)

이로 인해 Ubuntu 18.04(사용중인 HOST) 에선 지원이 안된다
![image.png](\assets\img\postimg\999_ETC\VSCODE_SSH_Err_02.png)

Ubuntu 18.04에서 SSH를 계속 사용하기 위해 사용할 수 있는 방법은 두 가지.

# 0. 기본 설정
[VSCode-SSH Docs](https://code.visualstudio.com/docs/remote/ssh)
단, 두 방법 모두 VSCode의 버전이 1.86 이하여야 한다.
![image.png](\assets\img\postimg\999_ETC\VSCODE_SSH_Err_03.png)

확인은 `Help` -> `About` 에서 확인.

또한 Host에서 `.vscode-server` 디렉토리가 이전 버전이거나 업데이트 되었다면 삭제한다.
혹시 `.cache` 디렉토리에 VSCode 서버 관련 파일이 존재한다면 삭제한다.

만약 업데이트 된 후 `.vscode-server`에 접근하면 구성이 아래와 다를 것이다. 뭐.. 바이너리는 다를수도.
```bash
drwxrwxr-x  5 kimec kimec 4096  2월  5 10:49 ./
drwxr-xr-x 25 kimec kimec 4096  2월  5 11:12 ../
-rw-rw-r--  1 kimec kimec 1944  2월  5 10:54 .1a5daa3a0231a0fbba4f14db7ec463cf99d7768e.log
-rw-rw-r--  1 kimec kimec    6  2월  5 10:49 .1a5daa3a0231a0fbba4f14db7ec463cf99d7768e.pid
-rwx------  1 kimec kimec   37  2월  5 10:49 .1a5daa3a0231a0fbba4f14db7ec463cf99d7768e.token*
drwxrwxr-x  3 kimec kimec 4096  2월  5 10:47 bin/
drwx------  6 kimec kimec 4096  2월  5 10:49 data/
drwx------  2 kimec kimec 4096  2월  5 10:49 extensions/
```
## 1. SSH Remote Pre버전
![image.png](\assets\img\postimg\999_ETC\VSCODE_SSH_Err_04.png)

이미지는 버전 낮춘 모습이라 버튼이 활성화 되지 않았는데, `Switch to Release Version` 버튼 대신 PreRelease 관련 버튼이 있다.

이전 버전이면 이름 옆에 초록 상자로 `Pre-Release` 로 표시된다.

단, VSCode 버전이 1.86 이하일 때 가능하다.
만약 자동 업데이트 되었다면 아래 방법을 사용하거나 싹 밀고 다시 설치할 것.
이 부분은 1.86 이상에선 Pre 버전 테스트 안해봐서 잘 모르겠다. 될 수도 있으니 먼저 버전 낮춰 테스트도 괜찮을 듯 하다.

다시 설치해도 `Config` 파일을 날린게 아닌 이상 설정은 남는다.

## 2. VSCode 버전 고정
간단하다.
VSCode 1.86 이하의 exe 파일로 다시 설치한 후 버전을 고정한다.
[VSCode 릴리즈 노트](https://code.visualstudio.com/updates/v1_86)

![image.png](\assets\img\postimg\999_ETC\VSCODE_SSH_Err_05.png)

![image.png](\assets\img\postimg\999_ETC\VSCODE_SSH_Err_06.png)


기존의 VSCode 파일을 싹 날린 후 다시 설치하고.

1.  `Ctrl` + `,` 단축키로 VSCode의 설정 파일에 접근한다.
![image.png](\assets\img\postimg\999_ETC\VSCODE_SSH_Err_07.png)

2. `Update` 를 검색한다.
![image.png](\assets\img\postimg\999_ETC\VSCODE_SSH_Err_08.png)

3. Auto Update 탭을 `None` 한다.
![image.png](\assets\img\postimg\999_ETC\VSCODE_SSH_Err_09.png)

이 부분은 Extensions 업데이트 관련인데, 난 SSH Remote 업데이트로 이 문제가 났으니 그냥 None 했다. 아래의 Mode를 None하면 당연히 Extensions 업데이트로 인한 VSCode 업데이트도 막히겠으나 혹시 모르니 선택함.

4. Mode 탭 또한 `None` 한다.
![image.png](\assets\img\postimg\999_ETC\VSCODE_SSH_Err_10.png)

5. Mode 탭을 수정하면 재부팅 메시지가 나온다. 재부팅.

그 외 자동 업데이트 관련 설정을 꺼놓는다.

참고로 이거 설정하는 사이에 다시 업데이트 되었어서 또 지우고 다시 설치했다... App 내부에서 버전을 낮추는 설정을 찾는게 빠를지도..

6. SSH Remote는 이전 버전으로 받는다.
1.84로 고정한 뒤 Extension을 최신 버전으로 안받아봐서 모르겠다. 
만약 SSH Remote Extension의 더 이후 버전이 나온다면 어떻게 될까. 이후에 다시 테스트 할 것.