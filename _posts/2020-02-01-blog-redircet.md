---
title: "[Jekyll, GitHub Page] URL 변경 및 리다이렉트하기"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-02-01T18:40:30+09:00
categories:
  - Blog
tags:
  - Jekyll
  - GitHub Page
  - Redirect
  - Permalink
permalink: /blog/redirect/
---
---
제 [블로그](https://blog.uniony.me/){:target="_blank"}는 Jekyll과 GitHub Page 로 구현되어 있습니다. Jekyll의 기본설정에 따라, 포스트의 url이 `/카테고리/타이틀`로 설정 되어있었습니다. 이게 맘에 들지않아 url을 제 임의로 수정하고 기존의 url이 404 not found를 띄우지 않도록 Redirect했습니다.

<!--more-->

## Jekyll 포스트 url 변경하기
Jekyll 페이지의 url을 변경하는 방법은 간단합니다.
```
permalink: /my/url/
```
원하는 포스트에 `permalink` 변수를 추가해주시면 됩니다.

또는, 글로벌 변수를 수정하여 모든 포스트의 url의 형식을 한번에 바꾸는 방법도 있습니다. `_config.yml` 파일에
```
permalink: /:categories/:title/
```
이런 식으로 permalink 변수들을 사용하여 링크를 만들 수 있습니다. 사용할 수 있는 변수들은 지킬 공식 문서에 모두 나와있습니다. ([Permalinks](https://jekyllrb.com/docs/permalinks/){:target="_blank"})

저는 개별 포스트마다 제가 원하는 링크를 설정해주었습니다.

![permalink](/assets/post-images/redirect/permalink.png)

## 기존 링크 Redirect
포스트 url을 바꿔버렸으니, 기존의 url로 접속시 404 Not Found 에러가 발생합니다.

<img src="/assets/post-images/redirect/404.png" alt="404error" style="width:100%; border: 1px solid #24292e; margin-bottom: 10px;"/>

이 블로그는 Google Search Console에 등록되어 있습니다. 아직 유입은 없지만 구글에 등록된 링크를 탔을 때에 404 Not Found 에러를 띄우고 싶지는 않았습니다. 그래서 기존의 링크를 새로운 링크로 Redirect하기로 했습니다.

### 301 Redirect
Redirect를 하는 가장 좋은 방법은 서버에서 301 redirect 응답을 해 주는 것입니다. 그러나 이 블로그는 정적 페이지 호스팅(Static Page Hosting)인 GitHub Page를 이용하고 있기 때문에 서버단에서의 301 리다이렉트 응답을 보낼 수 없습니다.

### [Plugin 사용](https://github.com/jekyll/jekyll-redirect-from){:target="_blank"}
이 플러그인을 사용하면 리다이렉트 링크를 생성해준다고 합니다. 그러나 저는 리다이렉트해야할 링크가 몇개 없었기 때문에 굳이 플러그인을 사용하지 않았습니다.

### Redirected Layout 사용하기
제가 사용한 방법입니다. [이곳](https://superdevresources.com/redirects-jekyll-github-pages/){:target="_blank"}을 참고했습니다.

먼저 redirected.html을 만들어서 _layout 폴더에 넣어줍니다.

```html
<!DOCTYPE html>
<html>
<head>
<link rel="canonical" href="{{ page.redirect_to }}"/>
<meta http-equiv="content-type" content="text/html; charset=utf-8" />
<meta http-equiv="refresh" content="0;url={{ page.redirect_to }}" />
</head>
<body>
    <h1>Redirecting...</h1>
      <a href="{{ page.redirect_to }}">Click here if you are not redirected.<a>
      <script>location='{{ page.redirect_to }}'</script>
</body>
</html>
```
이 레이아웃은 `canonical` 태그를 최신 링크로 바꿔주고 `refresh` 메타 태그를 이용해 새로운 페이지로 redirect 시켜줍니다.

그리고 최상위 폴더에 `redirect`라는 폴더를 만든 뒤에 리다이렉트 링크를 하나씩 만들어줬습니다.
```
---
layout: redirected
sitemap: false
permalink: /coding/visual%20studio%20code/vscode-markdown/
redirect_to:  /vscode/markdown/
---
```
`permalink`에 예전 링크를 넣고 `redirect_to`에 새로운 링크를 넣어줍니다. 그리고 블로그의 `sitemap.xml`에 예전 링크가 들어가지 않도록 `sitemap`은 `false`로 설정해줍니다.

리다이렉트가 필요한 링크마다 리다이렉트 링크를 만들어 주었습니다.

![redirect.md](/assets/post-images/redirect/mds.png)

## 결과
이제 구글서치콘솔에 등록되어있는 포스트 링크로 접속했을 때에 새로운 페이지로 리다이렉트될 것입니다. 또한 시간이 지나면, 구글크롤링봇이 해당 페이지가 리다이렉트된 것을 파악하고 기존 링크로 만들어진 색인을 새로운 링크로 옮길 것입니다.

사실 refresh 메타 태그는 별로 추천되지 않는 방법이라고 합니다. 유저가 원하지 않는 링크로 이동시키고, 시간이 오래걸리기 때문이죠. 그러나 301 Redirect가 불가능할 때에는 최선의 방법이라고 생각되어 이 방법을 사용했습니다. 더 나은 방법을 찾으면 알려드리도록 하겠습니다.

## Reference
* [Permalinks](https://jekyllrb.com/docs/permalinks/){:target="_blank"}
* [How to redirect URLs on Jekyll site hosted on GitHub Pages](https://superdevresources.com/redirects-jekyll-github-pages/){:target="_blank"}
* [What is the best approach for redirection of old pages in Jekyll and GitHub Pages?](https://stackoverflow.com/questions/10178304/what-is-the-best-approach-for-redirection-of-old-pages-in-jekyll-and-github-page){:target="_blank"}
* [Google Warning: Using The Meta Refresh Tag Is Bad Practice](https://www.weboptimizers.com.au/google-warning-meta-refresh/){:target="_blank"}