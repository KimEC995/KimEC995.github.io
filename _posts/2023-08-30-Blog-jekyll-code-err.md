---
title: GitBlog Error - Local vs Web
author: Kimec995
date: 2023-08-30 10:00:00 +09:00
categories: [GitBlog, Error]
tags: [GitBlog, Error, Jekyll, WebFrameWork, HTML]
pin: true
math: true
mermaid: true
---

# GitBlog Error - Jekyll 코드 블록 수행 문제

어김없이 블로그 에러가 나왔다.

오늘의 에러는

## 에러

**Markdown 내부에 Django등 템플릿 엔진의 동적 HTML 제어문**

```html
{\%\ load static(.css 파일명) \%\}
```
**등을 입력하면 Jekyll 에서 문자열이 아닌 코드로 인식해 랜더 하려는 에러** 이다.

에러 코드는 하단과 같다.

```html
{% raw %}
{% load static(.css 파일명) %}

<!-- Default css Link -->
<link rel="stylesheet" type="text/css" href="{% static 'base.css %}">
{% endraw %}
```

```python

  Liquid Exception: Liquid syntax error (line 149): Unknown tag 'load' in /_posts/2023-08-29-WFW-HTML_CSS.md
                    ------------------------------------------------
      Jekyll 4.3.2   Please append `--trace` to the `serve` command 
                     for any additional information or backtrace. 
                    ------------------------------------------------
```

문제는 Jekyll 에서 왜인진 몰라도 해당 코드를 주석처리를 해도 코드로 인식한다는 것이었다.

시도한 방법은 다음과 같다.

1. 해당 코드를 주석 처리(html 주석 `<!-- -->`)

2. 해당 코드를 강조처리 ([``], [****] 등등)

3. 코드 블럭의 종류를 `html` -> `python`

4. 수많은 구글링..

## 해결

[깃허브 지킬이슈 링크](https://github.com/jekyll/jekyll/issues/4567)

마찬가지로 템플릿 엔진의 동적 html 제어문으로 해당 코드를 감싸 텍스트로 바꾸는 것 같다.

```html
{\%\ raw \%\}
{% raw %}
{% load static(.css 파일명) %}

<!-- Default css Link -->
<link rel="stylesheet" type="text/css" href="{% static 'base.css %}">
{% endraw %}
{\%\ endraw \%\}
```

`%`만 입력하면 명령어가 되기 때문에 앞뒤 `\`붙임


## 아쉬운점

1. 왜 Jekyll 에서 주석처리를 해도 그것을 무시하지 않고 렌더하려 했는지는 알지 못했다.이 부분은 마크 해 두고 다시 찾아봐야겠다.

2. 왜 Markdown 강조인 [****] 는 렌더 했으면서 템플릿엔진 강조[raw, endraw] 는 넘어갔는지는 모르겠다.
