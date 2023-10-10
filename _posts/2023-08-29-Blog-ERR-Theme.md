---
title: GitBlog Error - Local vs Web
author: Kimec995
date: 2023-08-28 11:05:00 +09:00
categories: [GitBlog, Error]
tags: [GitBlog, Error, Chirpy, Jekyll]
pin: true
math: true
mermaid: true
image: 
  path: /assets/img/postimg/jekyll.png
  alt: GitBlog Error - Local vs Web
---

8월 24일 이래저래 다사다난한 에러를 뚫고 드디어 깃허브블로그를 개설했었다!!
라는 희망이 피던 찰나 또 다른 에러들을 만났다...

이 포스트는 다른 사람들이 만약 나와 같은 문제를 겪는다면 구글링을 할 수고를 줄여주고 싶어 작성한다.

본 블로그는 Chirpy Jekyll Theme 을 사용한 블로그다.
- Ruby : 3.2.2
- Jekyll : 4.3.2
- Bundle : 2.4.19

이번에 겪은 문제.
1. 포스팅 업로드 문제
2. Light / Dark Theme Switcher 동작 안함(같은원인)
3. Post 내부 Contents MD 적용 안됨 - 간이목차 연동안됨(같은원인)

## 1. 포스팅 업로드 문제

`_posts` 폴더 내부에 `xx.md` 파일을 작성해도 업로드가 안된다. 

### 시도했던 방법은(실패)

- `categories` 를 패스 형식으로 작성한다.
    - 왜냐하면 포스트가 업로드 될 때 `categories` 에 링크가 생성되는데, 그 위치를 못 잡는다 생각했다.

<br>

- 해당 포스트의 이미지를 전부 삭제한다.
    - 이미 이전 포스트(테스트 포스트) 에서 이미지 관련 문제를 해결했었기에 이 부분은 한번 시도나 해보자 라는 마음이었다.

<br>

- `_posts` dir 가 어디서 호출이 되는지, 어떤 방식으로 포스팅이 되는지 확인한다.
    - `.\_includes\related-posts.html` 파일 경로 문제라 생각했었다. 내부를 확인 해 보니 `.\_layouts\post.html`  호출해 작업하더라. 훑어보니 겹치는 명령어나 큰 문제가 없다고 판단해(애초에 테스트 포스트는 업로드 됐었고...) 다른 방법을 찾아보았다.

### 해결 방법(성공)
- `YYYY-MM-DD-Title.md` 의 날짜가 업로드 날짜와 다르다 <----내 경우 이문제였다. 어이없게도 오늘 날짜를 헷갈렸다.
- `_config.yml`에 `futrue: true`를 추가해본다.
- 그 외 [스택오버플로우](https://stackoverflow.com/questions/16990138/jekyll-not-generating-posts)

솔직히 말이 안되긴 했다.\
포스팅이 안될거면 처음부터, 혹은 로컬에서부터 안됐어야 했는데 오늘꺼만 딱 안되어 당황했다.\
앞으로는 날짜 확인을 잘하자...

## 2. Light / Dark Theme Switcher 동작 안함

로컬에서는 `Light / Dark Theme Switcher` 가 작동했는데 웹에서는 작동을 안했다.

### 시도했던 방법은(실패)

- `_config` 파일의 theme_mode:  [light|dark] 를 `dark` 만 수행
    - 이유: switcher 코드자체에는 문제가 없었을 것. 로컬에선 잘 됐으니까. 아마 웹에서 theme 설정 기본값이 코드 내부에 충돌해 이런 문제가 생겼다고 판단.

<br>

- 로컬과 웹 업로드 된 코드 비교 - `Switcher, Theme` 단어가 들어가는 코드 전부
    - 업로드 과정 중 코드 문장이던 url이던 손상을 입었을 것이라 판단. => 전부 멀쩡했다.

<br>

- 웹에서 동작한 HTML 과 로컬에서 동작하는 HTML 코드 비교
    - 똑같았다.

### 해결 방법(성공)
문제를 찾던 중 갑자기 머릿속에 한가지 생각이 떠올랐다.  

GithubBlog를 만들 때 `Chirpy` 테마 내부에 `assets/js/dist` 가 없었어서 마찬가지로 웹 동작이 안됐었다.

그래서 로컬에서 따로 파일을 다운받아 생성했는데, 이게 같이 커밋됐었나?

라는 생각이 들어 `.gitignore` 파일을 열어보았다.


```python
# Misc
assets/js/dist
```

아!

주석처리 하고 커밋했더니 작동한다!!

만약 비슷한 문제를 겪는 사람이라면

1. 위 방법처럼 `.gitignore` 주석처리

2. `Chirpy-demo` [깃허브링크](https://github.com/cotes2020/chirpy-demo/tree/main/assets/js/dist) 에서 해당 파일 세팅하기

를 추천한다!

## 3. Post 내부 Contents MD 적용 안됨 - 간이목차 연동안됨

마찬가지로 로컬에선 동작했지만, 웹에서는 동작을 안했다.

[2번 문서](#2-light--dark-theme-switcher-동작-안함) 과 같은 방법으로 해결되었다.

## 마무리

이번 포스트에선 깃블로그 개설 후 겪은 문제들을 모아보았다.

조만간은 깃블로그 개설 중 겪은 문제들을 올려볼까 한다.
