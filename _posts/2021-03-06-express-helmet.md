---
title: "Express 웹사이트 보안 강화하기 [Helmet, CSP]"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-03-06T13:56:30+09:00
categories:
  - Node.js
tags:
  - Node.js
  - Express
  - Helmet
  - CSP
permalink: /nodejs/helmet/
---
---
Express로 만들어진 웹 사이트의 보안을 강화하기 위해 [Helmet](https://helmetjs.github.io/){:target="_blank"} 미들웨어를 사용합니다.
<!--more-->

{% include ad-contents.html %}

## 설치 및 사용법
* [Helmet](https://helmetjs.github.io/){:target="_blank"}

* [Production Best Practices: Security](https://expressjs.com/en/advanced/best-practice-security.html){:target="_blank"}

```
$ npm install helmet --save
```

```javascript
// ...

const helmet = require('helmet')

// 모든 기능 사용
app.use(helmet());

// or, 특정 기능 사용
app.use(helmet.contentSecurityPolicy());
app.use(helmet.dnsPrefetchControl());
app.use(helmet.expectCt());
app.use(helmet.frameguard());
app.use(helmet.hidePoweredBy());
app.use(helmet.hsts());
app.use(helmet.ieNoOpen());
app.use(helmet.noSniff());
app.use(helmet.permittedCrossDomainPolicies());
app.use(helmet.referrerPolicy());
app.use(helmet.xssFilter());

// ...
```

{% include ad-contents.html %}

## Content Security Policy (CSP)
CSP를 사용하면 웹사이트의 http응답에 CSP 헤더가 추가됩니다. CSP 헤더가 존재할 경우, 브라우저는 CSP 헤더에 언급되지 않은 리소스들을 로드하지 않습니다. Helmet의 CSP 기본설정은 'self' 즉, 자신의 웹사이트에 존재하는 리소스들만을 허용합니다.

![Broken LoLog.me](/assets/post-images/lolog-helmet/broken.png)
(CSP로 인해 리소스를 가져오지 못한 웹페이지 [LoLog.me](https://lolog.me/){:target="_blank"})

그래서 CDN 등의 외부 사이트의 소스를 이용할 경우, 또는 다른 웹사이트에서 이미지 로드하는 경우, 심지어 인라인 스크립트로 자바스크립트 코드를 작성한 경우 모두 에러를 발생시킵니다. 이를 해결하기 위해서는 웹 페이지가 사용할 신뢰할 수 있는 리소스들의 도메인을 CSP 헤더에 추가해야합니다.

```javascript
const cspOptions = {
  directives: {
    // 기본 옵션을 가져옵니다.
    ...helmet.contentSecurityPolicy.getDefaultDirectives(),
    
    // 구글 API 도메인과 인라인된 스크립트를 허용합니다.
    "script-src": ["'self'", "*.googleapis.com", "'unsafe-inline'"],

    // 리그오브레전드 사이트의 이미지 소스를 허용합니다.
    "img-src": ["'self'", "data:", "*.leagueoflegends.com"],
  }
}

// Helmet의 모든 기능 사용. (contentSecurityPolicy에는 custom option 적용)
app.use(helmet({
  contentSecurityPolicy: cspOptions,
}));
```

{% include ad-contents.html %}

## 'unsafe-inline' 대체하기
CSP 헤더에 'unsafe-inline'을 추가하면, 인라인된 스크립트의 실행이 허용됩니다. 그러나 이는 해커가 웹사이트의 inline 스크립트를 주입하여 보안상의 위협을 가할 수 있게 합니다. 이를 해결하는 방법은 세가지가 있습니다.

### 1. inline script 없애기
기존에 작성했던 인라인 스크립트를 개별 파일로 옮기고, js 파일 자체를 script 태그의 src 속성으로 추가하면 됩니다. onclick 속성과 같이, 태그 내부에서 자바스크립트를 실행하는 속성들은 js 파일에서 addEventListener() 함수로 대체할 수 있습니다.

```html
<button id="btn" onclick="doSomething()">
```
버튼 태그의 onclick 속성을
```javascript
document.getElementById("btn").addEventListener('click', doSomething);

// jQuery 이용시
$('#btn').click(dosomething);
```
 addEventListener() 함수를 호출함으로써 대체할 수 있습니다. (또는 jQuery 이용)

### 2. nonce 속성 이용
웹페이지를 로드할 때마다 무작위로 생성된 nonce 속성을 인라인 스크립트에 부여하고, 이를 CSP 헤더에 추가합니다. 응답(response) 마다 다른 nonce 값이 설정되므로, 제 3자가 인라인 스크립트를 주입하기 어려워집니다.

Express에서 nonce 생성 및 사용 예제
```javascript
// nonce 생성
app.use((req, res, next) => {
  res.locals.cspNonce = crypto.randomBytes(16).toString("hex");
  next();
});

// helmet 설정 예
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", (req, res) => `'nonce-${res.locals.cspNonce}'`]
    },
  })
);
```

HTML에서 script 태그에 nonce 속성 추가
```html
<script nonce="{cspNonce Value}">
  alert('Hi');
</script>
```
### 3. hash 사용
각각의 인라인 스크립트의 hash 값을 CSP에 추가합니다. 'inline-unsafe' 속성을 제거한 채로 브라우저에서 웹페이지를 로드할 경우 에러 메시지가 뜨는데, 메시지 내부에 해당 인라인 스크립트의 해시값을 얻을 수 있습니다.

![Error massages](/assets/post-images/lolog-helmet/error.png)
빨간 박스 안의 값이 해시값입니다. inline script 마다 고유한 해시값을 갖고 있습니다. CSP의 scriptSrc 옵션에 해당한는 해시값을 추가하면 됩니다. (여러개의 inline script가 있을 경우에는 각각의 해시값을 모두 넣어야합니다.)
```javascript
// helmet 설정 예
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", , "'sha256-8tCjiEiH4STP6Nn4TLcbKGQVABm858n6iZwulDeFJFA='"]
    },
  })
);
```

hash를 사용할 경우에는 각각의 inline script 마다 해시값을 추가해야하고, inline script가 매번 바뀔 경우 해시값도 매번 바뀌게 된다는 단점이 있습니다. 웹사이트에 inline script가 매우 적고, 항상 동일할 경우에만 사용하는 것이 좋겟습니다.

{% include ad-contents.html %}

## CSP에 Google Gtag 추가 (Google Tag Manager)
Google Gtag를 손쉽게 관리하기 위해서, 기존 Gtag 코드를 Google Tag Manage로 대체했습니다.
* [태그 관리자 설정 및 설치하기](https://support.google.com/tagmanager/answer/6103696?hl=ko){:target="_blank"}

태그 관리자 코드에 nonce를 추가하고 몇몇 도메인을 허용해주면, CSP와 함께 구글 태그 관리자를 정상적으로 사용할 수 있습니다.
* [Using Google Tag Manager with a Content Security Policy](https://developers.google.com/tag-manager/web/csp){:target="_blank"}

nonce를 추가한 Google Tag Manager 구성 태그
```html
<!-- Google Tag Manager -->
<script nonce='{SERVER-GENERATED-NONCE}'>(function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
'https://www.googletagmanager.com/gtm.js?id='+i+dl;var n=d.querySelector('[nonce]');
n&&j.setAttribute('nonce',n.nonce||n.getAttribute('nonce'));f.parentNode.insertBefore(j,f);
})(window,document,'script','dataLayer','GTM-{YOUR-CONTAINER-ID}');</script>
<!-- End Google Tag Manager -->
```

추가해야할 CSP 옵션 (Google Tag Manager + Google Analytics)
```javascript
// nonce 생성
app.use((req, res, next) => {
  res.locals.cspNonce = crypto.randomBytes(16).toString("hex");
  next();
});

// contentSecurityPolicy 옵션
const cspOptions = {
  directives: {
    ...helmet.contentSecurityPolicy.getDefaultDirectives(),
    "script-src": ["'self'", "https://www.googletagmanager.com", "https://www.google-analytics.com",
      "https://ssl.google-analytics.com", "https://tagmanager.google.com", "*.gstatic.com", (req, res) => `'nonce-${res.locals.cspNonce}'`],
    "img-src": ["'self'", "www.googletagmanager.com", "https://www.google-analytics.com" ,
      "https://*.gstatic.com", "https://www.gstatic.com", "data:"],
   
    "connect-src": ["'self'", "https://www.google-analytics.com"]
  }
}

// contentSecurityPolicy 옵션과 함께 Helmet 기능 전체 활성화
app.use(helmet({
  contentSecurityPolicy: cspOptions,
}));
```

{% include ad-contents.html %}

## Reference
* [Helmet](https://helmetjs.github.io/){:target="_blank"}

* [Production Best Practices: Security](https://expressjs.com/en/advanced/best-practice-security.html){:target="_blank"}

* [Content Security Policy (CSP)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP){:target="_blank"}

* [CSP: script-src](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src){:target="_blank"}

* [Content Security Policy](https://developers.google.com/web/fundamentals/security/csp){:target="_blank"}

* [태그 관리자 설정 및 설치하기](https://support.google.com/tagmanager/answer/6103696?hl=ko){:target="_blank"}

* [Using Google Tag Manager with a Content Security Policy](https://developers.google.com/tag-manager/web/csp){:target="_blank"}