# SOP&CORS

# SOP(Same-Origin Policy, 동일 출처 정책)

## 정의

---

- SOP는 웹 보안의 핵심 원칙 중 하나로, 웹 브라우저가 다른 출처(origin)의 리소스와 상호 작용하는 방식을 제한합니다.
- 출처는 프로토콜, 도메인(호스트 이름), 포트의 조합으로 정의됩니다.
- 즉, 동일 출처 정책에 따르면, 한 출처에서 로드된 문서나 스크립트는 다른 출처의 리소스와 상호 작용할 때 제한을 받습니다.
- 이 정책의 주요 목적은 사용자의 중요한 데이터를 보호하고, CSRF 혹은 크로스 사이트 스크립팅(XSS) 공격과 같은 보안 위협으로부터 사용자를 보호하는 것입니다.
- ex) **[https://example.com에서](https://example.com에서) 로드된 웹 페이지는 SOP에 의해 [https://another-site.com](https://another-site.com) 의 리소스를 직접 AJAX 요청으로 불러오거나 읽을 수 없음.**

## Origin 이란 scheme + domain name + port

<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/bf11a920-5c7d-4c81-8979-76ea1a175861" width="500px"/>

HTTP PORT: 80, HTTPS PORT 443

- ✔️ `scheme`, `domain`, `port`가 동일한 경우:
    - `http://example.com**/app1/index.html` vs.`http://example.com**/app2/index.html`
    - `http://example.com:80/app1/index.html` vs `http://example.com**/app2/index.html`
    - `https://example.com:8080**/app1/index.html` vs.`https://example.com:8080/app2/index.html`
- ❌ `scheme`이 다른 경우:
    - `http://example.com`vs `https://example.com`
- ❌ `domain`이 다른 경우:
    - `https://www.example.com` vs `https://hello.example.com`
- ❌ `port`가 다른 경우:
    - `https://example.com:8080` vs `https://example.com`

SOP 정책 허용

- 1script**: 다른 출처(cross-origin)의 스크립트를 문서에 삽입(embed)하는 것은 가능하지만, `fetch API` 등을 이용하여 다른 출처로 요청을 날리는 것은 불가능합니다.
- **css**: `<link>` 혹은 `@import`로 다른 출처의 css를 삽입할 수 있습니다. 이때 올바른 `Content-Type` 헤더가 설정되어야 할 수도 있습니다.
- **iframe**: [X-Frame-Options](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) 응답 헤더가 `DENY` 혹은 `SAMEORIGIN`이 아닌 이상, 일반적으로 다른 출처의 iframe을 삽입하는 것은 가능합니다. 하지만 자바스크립트 등을 이용하여 다른 출처의 iframe에 접근하는 것은 불가능합니다.
- **form**: `<form>` 태그의 `action` 속성값으로 출처가 다른 URL을 사용할 수 있습니다. 즉, 다른 출처로 폼 데이터를 전송하는 것이 가능합니다.
- **image**: 다른 출처의 이미지를 삽입하는 것은 가능합니다. 하지만 자바스크립트 등을 이용하여 다른 출처의 이미지를 읽는 것은 불가능합니다 (e.g. 자바스크립트를 이용하여 다른 출처의 이미지를 `<canvas>`에 삽입하는 경우)
- **multimedia**: 다른 출처의 오디오·비디오를 삽입할 수 있습니다.

# CORS(Cross-Origin Resource Sharing, 교차 출처 리소스 공유)

## 정의

---

- CORS는 SOP의 제한을 안전한 방식으로 완화하기 위한 메커니즘입니다.
- 서버가 특정 출처에서 자신의 리소스에 접근하는 것을 허용하고자 할 때, CORS를 사용할 수 있습니다.
- CORS는 HTTP 헤더를 사용하여 웹 페이지가 다른 출처의 리소스에 접근할 수 있는 권한을 부여받을 수 있도록 합니다.
- ex) 서버가 CORS를 지원하면, 응답에 **`Access-Control-Allow-Origin`**과 같은 특정 CORS 관련 헤더를 포함시킬 수 있습니다. 이 헤더는 어떤 출처(origin)에서 자신의 리소스에 접근할 수 있는지를 브라우저에 알려줍니다. 예를 들어, **`https://example.com`**의 서버가 **`Access-Control-Allow-Origin: https://another-site.com`** 헤더를 포함한 응답을 보내면, **`https://another-site.com`**은 **`https://example.com`**의 리소스에 접근할 수 있게 됩니다.

CORS를 통해 웹 애플리케이션에서 다양한 출처의 리소스를 안전하게 사용할 수 있게 해주면서도, 개발자가 보안을 유지할 수 있도록 설계되었습니다. 이를 통해 API 사용, 외부 리소스 로딩, 웹 폰트 등 다양한 웹 기술에서 교차 출처 접근이 필요한 경우 보안과 유연성을 동시에 제공

## CORS 동작

1. 기본적으로 웹 애플리케이션이 출처가 다른 곳에 요청할 때 요청 헤더에 `Origin` 이라는 필드를 함께 보냅니다:

`Origin: https://www.google.com`

1. 서버가 응답할 때 `Access-Control-Allow-Origin` 이라는 응답 헤더 필드에 허용하고자 하는 출처를 명시합니다. 예를 들어, 출처가 `https://my-server.com`인 서버에서 `https://www.google.com` 로 부터 오는 리소스 요청을 허용하고자 한다면, 응답 헤더에 `Access-Control-Allow-Origin: https://www.google.com` 을 설정하여 응답하게 됩니다. 그리고 응답을 받은 브라우저는 자신이 보냈던 요청의 `Origin`과 서버 응답의 `Access-Control-Allow-Origin` 값이 일치하는지를 살펴보고, 일치하면 출처가 다른 요청일지라도 유효한 요청으로 보고 받아온 리소스를 사용할 수 있게 합니다.

<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/01b01fe2-433c-4f60-b7ac-828e2bbb6931" width="500px"/>

이때 SOP도 그렇고 CORS도 마찬가지로, 이들은 **브라우저의 정책**이라는 점입니다. 즉, 다른 출처의 리소스를 사용하는 것을 제한하는 것은 서버가 아니라 브라우저라는 점, 다시 말해 만약 서버에 CORS 정책을 위반한 요청을 날렸을 때 이를 차단하는 것은 서버가 아니라 브라우저라는 점입니다. 서버는 정상적으로 요청을 받아 응답하지만, 이후 응답을 받은 브라우저에서 이를 분석하여 CORS 정책을 위반한 경우라면 이를 차단하는 것입니다. 이와 같은 이유로, 브라우저에선 CORS 정책으로 인해 차단되는 요청도 [Postman](https://www.postman.com/)과 같이 브라우저가 아닌 환경(혹은 서버 끼리 통신)에선 통신이 원활하게 이뤄질 수 있습니다.

<img src="https://github.com/wjdansrl7/2024_CS_STUDY/assets/48114924/9e6ad3aa-3410-4fc7-9dd4-86f8af541caa" width="500px"/>


CORS 설정과 관련된 헤더들

# **요청 헤더**

### **Origin**

`Origin` 헤더는 출처가 다른 요청 혹은 preflight 요청의 출처를 나타냅니다. 값이 `null` 일 수도 있으며, 출처가 다른 요청의 경우 항상 `Origin` 헤더가 전송됩니다.

### **Access-Control-Request-Method**

preflight 요청을 보낼 때, 서버에게 실제 요청의 HTTP 메서드 정보를 알려주는 역할을 합니다.

### **Access-Control-Request-Headers**

preflight 요청을 보낼 때, 서버에게 실제 요청의 HTTP 헤더 정보를 알려주는 역할을 합니다.

# **응답 헤더**

### **Access-Control-Allow-Origin**

리소스에 접근할 수 있는 출처를 명시합니다. 오직 하나의 출처를 명시하거나, 와일드카드(`*`)를 사용할 수 있습니다. 단, 인증 정보(credential)를 사용하는 경우 와일드카드를 사용할 수 없고, *반드시* 하나의 출처를 명시해야만 합니다. 인증 정보를 사용하는 요청에 와일드카드를 사용하게 되면 에러가 발생합니다.

이때, 헤더 값으로 와일드카드가 아니라 하나의 출처를 명시하는 경우, 출처를 `Vary` 헤더에도 명시하여 클라이언트에게 출처에 따라 응답이 달라질 수 있음을 명시해야 합니다.

### **Access-Control-Expose-Headers**

기본적으론 브라우저의 스크립트에 노출되지는 않지만, 브라우저 스크립트에서 접근할 수 있도록 허용하는 헤더를 지정합니다. 기본으로 노출되는 헤더들은 아래와 같습니다

- `Cache-Control`
- `Content-Language`
- `Content-Length`
- `Content-Type`
- `Expires`
- `Last-Modified`
- `Pragma`

### **Access-Control-Max-Age**

preflight 요청이 얼마 동안 캐시 될지를 나타낼 때 사용됩니다. 이 헤더 값의 단위는 `초` 입니다.

파이어폭스의 경우 최대값은 `86,400초`(24시간), 크롬(Chromium)의 경우 버전 76 이전에는 최대 `600초`(10분), 버전 76 부터는 최대 `7,200초`(2시간)로 제한하고 있습니다.

### **Access-Control-Allow-Credentials**

요청의 인증 모드(e.g. fetch API의 `credentials`)가 `include`인 경우, 응답을 브라우저의 스크립트에서 접근할 수 있도록 허용할지를 나타낼 때 사용됩니다. 즉, `credentials`가 `include`인 요청의 경우 `Access-Control-Allow-Credentials` 헤더 값이 `true` 이어야만 스크립트에서 응답에 접근할 수 있습니다. 이때 인증 정보(credential)에는 HTTP 쿠키, 인증 헤더, TLS 클라이언트 인증서 등이 포함됩니다.

### **Access-Control-Allow-Methods**

preflight 요청에 대한 응답에 사용되는 헤더로, 이후 본 요청에서 어떤 HTTP 메서드를 사용할 수 있는지를 나타낼 때 사용됩니다. 하나 이상의 HTTP 메서드를 `,` 로 구분하여 명시할 수 있으며, 와일드카드 또한 사용 가능하지만 인증 정보를 사용하는 요청의 경우, “모든 HTTP 메서드 허용”으로 해석되는 것이 아니라 문자 그대로 ”’*’ 메서드 허용”으로 해석됩니다. 따라서 인증 정보를 사용하는 경우에는 와일드카드 대신 HTTP 메서드를 구체적으로 명시하는 것이 바람직합니다.

### **Access-Control-Allow-Headers**

`Access-Control-Request-Headers` 헤더를 포함하는 preflight 요청에 대한 응답에 사용되는 헤더로, 이후 본 요청에서 어떤 HTTP 헤더를 사용할 수 있는지를 나타낼 때 사용됩니다. `Access-Control-Request-Headers` 요청 헤더가 존재하는 경우 반드시 응답 헤더에 `Access-Control-Allow-Headers` 를 포함해야 합니다.

`Access-Control-Allow-Methods`와 유사하게, 하나 이상의 HTTP 헤더를 `,` 로 구분하여 명시할 수 있고, 와일드카드 또한 사용 가능하지만 인증 정보를 사용하는 경우에는 “모든 HTTP 헤더 허용”이 아니라 ”’*’ 헤더 허용”으로 해석됩니다.

CORS의 상세한 동작 흐름

# **1. 간단한 요청인경우 (Simple Request)**

아래의 조건을 *모두* 만족하는 HTTP 요청은 **간단한 요청(simple request)**로 간주됩니다:

- HTTP 메서드가 아래 세 가지 중 하나인 경우:
    - `GET`
    - `HEAD`
    - `POST`
- 브라우저에 의해 자동으로 설정되는 헤더(e.g. `Connection`, `User-Agent`)등을 제외하고, 요청 헤더가 아래 목록에 나와있는 것만 설정된 경우:
    - `Accept`
    - `Accept-Language`
    - `Content-Language`
    - `Content-Type`의 경우, 헤더값이 아래 셋 중 하나이어야 함:
        - `application/x-www-form-urlencoded`
        - `multipart/form-data`
        - `text/plain`

위 조건을 만족하는 요청은 별다른 절차 없이 다른 출처에 HTTP 요청을 보낼 수 있습니다. 이 경우, 일단 요청을 보내고 응답을 받은 뒤 해당 요청이 CORS를 만족하는지를 체크합니다.

# **2. 복잡한 요청인경우 (Complex Request)**

간단한 요청 이외의 요청을 **복잡한 요청(complex request)**라고 하는데, 이 경우 본 요청을 보내기 전에 [preflight 요청](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#preflighted_requests)을 보내서 다른 출처에 해당 요청을 보낼 수 있는지 점검한 뒤, 보낼 수 있다고 확인을 받으면 그제야 본 요청을 서버에 보냅니다.

preflight 요청은 `OPTION` HTTP 메서드를 사용하며, 일반적으로 아래와 같이 생겼습니다:

`OPTIONS /data HTTP/1.1
Origin: https://example.com
Access-Control-Request-Method: DELETE`

이를 받은 서버에선 아래와 같이 허용하는 출처와 허용하는 메서드, preflight 요청을 얼마 동안 캐시 할 것인지 등에 관한 정보를 응답합니다:

`HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: GET, DELETE, HEAD, OPTIONS
Access-Control-Max-Age: 86400`

이에 대한 흐름을 그림으로 나타내면 아래와 같습니다:

![https://cdn.jsdelivr.net/gh/jaehyeon48/jaehyeon48.github.io@master/assets/images/web/sop-and-cors/preflight_request.png](https://cdn.jsdelivr.net/gh/jaehyeon48/jaehyeon48.github.io@master/assets/images/web/sop-and-cors/preflight_request.png)

preflight 요청 흐름.

이때, preflight 요청의 성공/실패 여부, 즉 preflight 요청에 대한 응답이 성공(200 OK)인지, 혹은 실패(400번대 코드)인지의 여부와 CORS 위반 에러는 별 상관이 없다는 점에 주의하세요. 브라우저가 CORS 정책 위반 여부를 판단하는 시점이 preflight 응답을 받은 이후일 뿐만 아니라, `Origin` 값과 `Access-Control-Allow-Origin`의 값이 같냐 다르냐를 토대로 CORS 정책 위반 여부를 판별하는 것이지 preflight 요청 자체의 성공/실패 여부를 토대로 판별하는 것이 아닙니다.

이렇게 preflight 요청에 담은 `Origin` 헤더 값과 preflight 응답으로 부터 받아온 `Access-Control-Allow-Origin` 헤더 값을 비교하여, CORS 정책을 위반했다면 이후 실제 요청은 전송되지 않습니다.

# **3. Credential을 포함한 요청일경우**

마지막은 credential, 즉 HTTP 쿠키와 같이 인증 정보를 포함하는 요청을 보내는 경우입니다. 기본적으로 `fetch` 등의 요청 API는 다른 출처에 요청하는 경우엔 쿠키 정보나 인증과 관련된 헤더를 요청에 포함하지 않습니다. 만약 요청에 포함하고자 한다면 `credentials` 옵션을 사용하면 되는데, 이 옵션에는 3개의 값이 존재합니다:

- `same-origin`: 기본값으로, 같은 출처 간 요청에만 인증 정보를 포함합니다.
- `include`: 다른 출처에 요청하는 경우에도 항상 인증 정보를 포함하도록 합니다.
- `omit`: 인증 정보를 절대 포함하지 않도록 합니다.

이때, 다른 출처에 인증 정보를 포함하여 요청을 보내는 경우, 서버 측에선 반드시 `Access-Control-Allow-Credentials` 응답 헤더 값을 `true`로 설정해야 하고, `Access-Control-Allow-Origin` 헤더 값에 `*` (모든 출처를 허용) 대신 반드시 하나의 출처를 명시해야 합니다.

브라우저는 **cross-origin 요청**을 전송하기 전에 **OPTIONS** 메소드로 **preflight**를 전송한다.

**Response**로 **Access-Control-Allow-Origin**과 **Access-Control-Allow-Methods**가 넘어오는데 이는 서버에서 **어떤 origin**과 **어떤 method**를 허용하는지 브라우저에게 알려주는 역할을 한다.

브라우저가 결과를 성공적으로 확인하고 나면 **cross-origin** 요청을 보내서 그 이후 과정을 진행한다

## Preflight 관련 설정

> 그럼 cross-origin 요청을 보낼 때마다 매번 preflight 요청을 보내나요?
> 

결론부터 말하자면 매번 보내지 않는다.

서버 설정을 통해서 **preflight** 결과의 캐시를 일정 기간 동안 저장시킬 수가 있다.

이 캐시 정보가 살아있는 시간 동안은 **cross-origin 요청**에 대해서 **preflight**를 생략하고 바로 요청 전송이 가능하다.

참고자료

[https://jaehyeon48.github.io/web/sop-and-cors/](https://jaehyeon48.github.io/web/sop-and-cors/)

[https://velog.io/@jesop/SOP와-CORS](https://velog.io/@jesop/SOP%EC%99%80-CORS)
