# 클라이언트 - 서버 아키텍처

**2-Tier 아키텍처 (클라이언트 - 서버 아키텍처)**

리소스가 존재하는 곳과 리소스를 사용하는 곳을 분리시킨 것

- 리소스가 존재하는 곳: 서버
- 리소스를 사용하는 곳: 클라이언트

클라이언트와 서버는 요청과 응답을 주고받는다. 클라이언트-서버 아키텍처에서는 요청이 선행되고, 그 후에 응답이 온다.

**3-Tier 아키텍처**

기존 2티어 아키텍처에 데이터베이스가 추가된 형태를 3티어 아키텍처라고 한다.

일반적으로 서버는 리소스를 전달해주는 역할만 담당한다.

리소스를 저장하는 공간은 별도로 마련해둔다. → 데이터베이스

- 클라이언트처럼 사용자가 직접 눈으로 보고, UI를 클릭 또는 터치하는 등의 상호작용을 할 수 있는 앱을 개발 → 프론트엔드 개발자
- 사용자 눈에 보이지 않지만, 상품 정보를 API로 노출한다던지, 로그인, 로그아웃, 권한 관리 등 사용자 인증을 주로 다룰 때 / 데이터 베이스 등 시스템 설계를 하는 경우 → 백엔드 개발자

**[클라이언트의 종류]**

플랫폼에 따라 구분.

- 웹사이트 (웹 앱) , 웹 브라우저
- 스마트폰 / 태블릿용 앱
- 데스크탑 앱

# 클라이언트와 서버의 통신

### 프로토콜

클라이언트-서버 간 요청과 응답 방법을 지정한 **통신 규약**

ex) 웹 어플리케이션 아키텍처에서는 클라이언트와 서버가 서로 **HTTP라는 프로토콜**을 이용해 요청과 응답을 주고받는다. HTTP를 이용해 주고받는 메시지를 **HTTP 메시지**라고 한다.

### API (Application Programming Interface)

클라이언트에서 요청으로 하려는데, 정확한 규약에 따라서 요청을 해야한다. 그런데 서버가 어떻게 구성되어 있는지 모르면, 클라이언트는 요청을 제대로 할 수 없다.

→ **서버가 클라이언트에게 리소스를 잘 활용할 수 있도록 인터페이스(Interface)를 제공한 것**: API

보통 인터넷에 있는 데이터를 요청할 때는 HTTP 프로토콜을 사용하여 주소(URL, URI)를 통해 접근할 수 있다.

[(스타벅스 API서버가 제공하는 적절한 URL 디자인 예제)](https://en.wikipedia.org/wiki/Query_string)

### HTTP API 디자인 잘 하는 방법

→ Best Pratice.

HTTP 요청에는 메소드라는 것이 존재한다. 메소드를 지정하여 리소스와 관련된 행동들을 지정할 수 있다.

- 리소스를 달라고 요청하거나(GET), 정보를 추가하거나(CREATE), 지워 달라고 요청할 수 있다. (DELETE)
- HTTP 메소드는 CRUD 행동에 따라 목적에 맞게 사용해야 한다.

[HTTP 요청 메서드](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)

[제목 없음](https://www.notion.so/7d9383a65146489fbfd6948603e57475)

---

# URL과 URI

**URL (Uniform Resource Locator)** : 네트워크 상에서 웹 페이지, 이미지, 동영상 등 파일이 위치한 정보

- scheme, hosts, url-path로 구분할 수 있다.
- scheme: 통신방식(프로토콜)
- hosts: 웹 서버의 이름이나 도메인, IP를 이용한 주소
- url-path: 웹 서버에서 지정한 루트 디렉토리부터 웹페이지, 이미지, 동영상 등이 위치한 경로와 파일명
  :scheme: :hosts: :url-path:
  [file://127.0.0.1/Users/username/Desktop/](file://127.0.0.1/Users/username/Desktop/)
  http://www.google.com:80/search

**URI (Uniform Resource Identifier)** : 일반적인 URL의 기본 요소에서 query, bookmark가 포함되는 것이다.

- **query**: 웹 서버에 보내는 추가적인 질문
- ex) [`http://www.google.com:80/search?q=JavaScript`](http://www.google.com:80/search?q=JavaScript)를 브라우저에 검색하면 구글에서 JavaScript를 검색한 결과가 나온다.
- URI는 URL을 포함하는 상위 개념

- `127.0.0.1` 은 로컬 PC를 나타낸다.
- port는 서버로 진입할 수 있는 통로이다.

---

# IP와 port

**IP address(Internet Protocol address, IP 주소)**

- 네트워크에 연결된 특정 pc의 주소를 나타내는 체계
- 인터넷 상에서 사용하는 주소 체계
- IPv4(Internet Protocol version 4) : IP 주소의 4번째 버전

  터미널에서 `nslookup google.com`을 입력하면 IP주소를 확인할 수 있다.

  각 덩어리마다 0부터 255까지 나타낼 수 있고, 2^(32), 약 43억개의 ip주소를 표현할 수 있다.

  이중 용도가 정해진 것이 있다.

  - **localhost, 127.0.0.1** : 현재 사용 중인 로컬 PC를 지칭
  - **0.0.0.0, 255.255.255.255** : **broadcast address**, 로컬 네트워크에 접속된 모든 장치와 소통하는 주소. 서버에서 접근 가능 IP 주소를 broadcast address 로 지정하면, 모든 기기에서 서버에 접근할 수 있다.

현재는 개인 PC 보급과, 각종 서비스를 위한 서버를 생산하면서 IPv4로 할당할 수 있는 pc가 한계를 넘어섰다. → IPv6의 등장, 2^(128) 개의 주소를 표현할 수 있다.

**Port**

- 터미널에서 리액트를 실행하면 나타나는 화면에는, 로컬 PC의 IP 주소인 127.0.0.1 뒤에 :3000과 같은 숫자가 표현된다. 이 숫자는 **IP 주소가 가리키는 PC에 접속할 수 있는 통로(채널)**을 의미한다.
- 이미 사용 중인 포트는 중복해서 사용할 수 없습니다.
- 포트 번호는 0~ 65,535 까지 사용할 수 있다. 그 중에서 0 ~ 1024번까지의 포트 번호는 주요 통신을 위한 규약에 따라 이미 정해져 있다.

  - 22 : SSH
  - 80 : HTTP
  - 443: HTTPS

  이미 정해진 포트번호라도 필요에 따라 자유롭게 사용할 수 있다. 잘 알려진 포트의 경우 URL에 명시하지 않지만, 그 외의 잘 알려지지 않은 포트(`:3000`과 같은 임시 포트는 반드시 포함해야 한다.)

---

# 도메인과 DNS

**Domain name**

- 웹사이트에 접속할 때, IP주소를 대신하여 사용하는 주소가 있다. 도메인 이름을 이용하면, 한눈에 파악하기 힘든 IP주소를 보다 분명하게 나타낼 수 있다.
  `nslookup` 명령으로 google.com의 IP 주소와 도메인 네임을 알 수 있다.
  - IP 주소: 172.217.161.238
  - 도메인 이름: google.com

**DNS**

- 네트워크 상에 존재하는 모든 PC는 IP 주소가 있다. 그러나 모든 IP주소가 도메인 이름을 가지는 것은 아니다.
- 로컬 PC를 나타내는 127.0.0.1은 [localhost](http://localhost) 로 지정할 수 있지만, 그 외에 모든 도메인 이름은 일정 기간 동안 대여하여 사용한다.
- 이렇게 **대여한 도메인 이름과 IP 주소를 매칭**하기 위해 네트워크에서 이것을 위한 서버가 별도로 있다.
  ⇒ DNS (Domain Name System) : 호스트의 **도메인 이름을 IP 주소로 변환**하거나, **반대의 경우**를 수행할 수 있도록 개발된 데이터 베이스 시스템
  - 브라우저 검색창에 [google.com](http://google.com) 입력 → 이 요청은 DNS에서 IP주소(172.217.161.238)를 찾음. → 이 IP 주소에 해당하는 웹 서버로 요청을 전달 → 클라와 서버가 통신할 수 있도록 한다.

---

[크롬 브라우저 에러 읽기 ](https://www.notion.so/4c6e6a48bf1f4195949803ac03b679c8)

전체 에러 메시지 목록 ⇒ 브라우저 검색창에 `chrome://network-error/` 을 입력하여 확인한다.

[문제 해결 도움](https://support.google.com/chrome/answer/95669#zippy=%2Cpage-loading-error-codes-and-issues)

⭐⭐⭐ [chrome network tab 사용법](https://www.youtube.com/watch?v=e1gAyQuIFQo)

---

# HTTP Messages

**HTTP (Hyper text transfer protocol)**

- HTML과 같은 문서를 전송하기 위한 [Application Layer](https://en.wikipedia.org/wiki/Application_layer) 프로토콜이다.
- 웹 브라우저와 웹 서버와 소통을 위해 디자인 되었다.
- 전통적인 클라이언트-서버 모델에서 클라이언트가 HTTP message 양식에 맞춰 **요청**을 보내면, 서버도 양식에 맞춰 **응답**을 한다.
- ⭐ HTTP는 특정 상태를 유지하지 않는다. (**Stateless, 무상태성**)

< Rest API를 이해하는 기본적인 부분 >

→ HTTP의 클라이언트-서버 흐름을 구성하는 구성 요소를 이해한다.

Pull protocol로 이루어진다.

### HTTP 메시지

http 메시지는 **서버와 클라이언트 간에 데이터가 교환되는 방식**이다.

- 메시지 타입: 요청(request). 응답(response)
- 요청(request): 클라이언트가 서버로 전달해서 서버의 액션이 일어나게끔 하는 메시지
- 응답(response): 요청에 대한 서버의 답변

**HTTP request**

- header
  - Request line
    - HTTP verb(요청 메소드) / URI / HTTP version number
  - optional requesst header
    - 요청 헤더에 대한 특정 속성을 설명하는데 사용한다.
    - name: value, name: value
- blank - header와 body를 구분한다.
- body(optional) : 서버에 보내고자 하는 요청에 대한 다른 정보를 추가한다.

**HTTP response**

응답 요청. 요청된 정보를 포함하거나 HTTP 요청 메시지와 유사한 것이 있는 경우 오류 메시지를 포함할 수 있다.

- header

  - status line

    - HTTP version / status code / reason phrase

          현재 프로토콜 버전 / 상태 코드(요청의 결과-200, 302, 404) / 상태 텍스트 ( 상태 코드 설명)

  - optional requesst header
    - 요청을 지정하거나, 요청 헤더에 대한 특정 속성을 설명하는데 사용하는 헤더의 집합
    - name: value, name: value

- blank - header와 body를 구분한다.
- body(optional) : 클라이언트가 웹페이지를 요청한 경우 클라이언트가 요청한 데이터 혹은 문서가 포함되어 있다.

### stateless

- 상태를 가지지 않는다.
- HTTP로 클라이언트와 서버가 통신을 주고 받는 과정에서, HTTP가 클라이언트나 서버의 상태를 확인하지 않는다.
- 필요에 따라 다른 방법(쿠키-세션, API 등)을 통해 상태를 확인할 수 있다.

꼭 확인하기!

[HTTP 요청 메서드](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)

[HTTP 메시지](https://developer.mozilla.org/ko/docs/Web/HTTP/Messages)

[HTTP 상태 코드](https://developer.mozilla.org/ko/docs/Web/HTTP/Status)

Advanced!

- [MDN: MIME Type](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types)
  - `Content-Type`에 대해서 설명합니다.
- [브라우저는 어떻게 동작하는가](https://d2.naver.com/helloworld/59361)

---

# AJAX (Asynchrnous JavaScript And XmlHttpRequest)

- JavaScript DOM과 Fetch, XMLhttpRequest, HTML 등 다양한 기술을 사용하는 웹 개발 기법
- 웹페이지의 필요한 부분에 필요한 데이터만 비동기적으로 받아와 화면에 그려낼 수 있다.
- 유저의 요구에 따라 반응하며 변화하는 부분

  ex)

  - **구글의 검색창** : 검색창에 한글자씩 입력할 때마다 해당 글자로 시작하는 단어를 서버에서 받아와 바로 아래 추천 검색어로 보여준다. 즉 검색창에서 필요한 데이터만 비동기적으로 받아와 렌더링되며, 여기에 AJAX가 사용된다.
  - **원티드**: 채용 공고 목록을 스크롤 하다가, 제일 하단에 도달하면, 새로운 채용 공고를 가져와 렌더링 한다. 이러한 이벤트→ 무한 스크롤 이라 하며, 무한 스크롤이 발생할 때마다, Fetch를 통해 데이터를 가져와 업데이트하고, 렌더링한다.
  - **페이스북 메시지**
  - **네이버 포털사이트 뉴스 탭**

- AJAX의 핵심 기술: JavaScript DOM, Fetch

  - **전통적인 웹 어플리케이션** → form 태그를 이용해 서버에 데이터를 전송 → 서버는 요청에 대한 응답으로 새 웹 페이지를 제공한다. 즉, 클라에서 요청을 보내면 매번 새로운 페이지로 이동한다.
    **[Fetch]**
  - **Fetch**를 이용하면, 페이지를 이동하지 않아도, 서버에서 필요한 데이터를 불러올 수 있다.
    - Fetch는 사용자가 현재 페이지에서 작업을 하는 동안 서버와 통신할 수 있도록 한다.
    - 즉, 브라우저가 서버에 요청을 보내고 응답을 받을 때까지 모든 동작을 멈추는 것이 아니라, 계속해서 새로운 페이지를 사용할 수 있게 하는 것이다.
    - **필요한 데이터**만 불러와 DOM에 적용시켜, 새로운 페이지로 이동하지 않고, 기존 페이지에서 필요한 부분만 변경한다.
  - Fetch 이전에는 XHR(XMLHttpRequest)를 사용했으나, fetch 는 XHR의 단점을 보완한 새로운 Web API이며, XML보다 가볍고, JavaScript와 호환되는 JSON을 사용한다.

  ```jsx
  // Fetch를 사용
  fetch('http://52.78.213.9:3000/messages')
  	.then (function(response) {
  		return response.json();
  	})
  	.then(function (json) {
  		...
  });
  ```

  ```jsx
  // XMLHttpRequest를 사용
  var xhr = new XMLHttpRequest();
  xhr.open("get", "http://52.78.213.9:3000/messages");

  xhr.onreadystatechange = function () {
   if (xhr.readyState !== 4) return;
   // readyState 4: 완료

   if (xhr.status === 200) {
    // status 200: 성공
    console.log(xhr.responseText); // 서버로부터 온 응답
   } else {
    console.log("에러: " + xhr.status); // 요청 도중 에러 발생
   }
  };

  xhr.send(); // 요청 전송
  ```

  - Fetch는 간편함, promise 지원 등의 장점을 가지고 있다.
    이외에도 Axios와 같은 라이브러리도 존재하며, 경우에 따라 적절한 것을 선택하여 사용한다.

### AJAX의 장점

- 서버에서 HTML을 완성하여 보내주지 않아도 웹페이지를 만들 수 있다. 필요한 데이터를 비동기적으로 가져와 브라우저에서 화면의 일부만 업데이트 하여 렌더링 할 수 있다.
- XHR이 표준화 되면서부터 브라우저에 상관 없이AJAX를 사용할 수 있게 되었다
- 유저 중심 어플리케이션 개발 AJAX를 사용하면 필요한 일부분만 렌더링하기 때문에 빠르고 더 많은 상호작용이 가능한 어플리케이션을 만들 수 있다.
- 더 작은 대역폭 대역폭: 네트워크 통신 한 번에 보낼 수 있는 데이터의 크기. 이전에는 서버로부터 완성된 HTML 파일을 받아와야했기 때문에 한번에 보내야 하는 데이터의 크기가 컸다. 그러나 AJAX에서는 필요한 데이터를 텍스트 형태(JSON, XML 등) 보내면 되기 때문에 비교적 데이터의 크기가 작다.

### AJAX의 단점

- Search Engine Optimization(SEO)에 불리
  AJAX 방식의 웹 어플리케이션은 한 번 받은 HTML을 렌더링 한 후, 서버에서 비동기적으로 필요한 데이터를 가져와 그려낸다. 따라서, 처음 받는 HTML 파일에는 데이터를 채우기 위한 틀만 작성되어 있는 경우가 많다.
  검색 사이트에서는 전세계 사이트를 돌아다니며 각 사이트의 모든 정보를 긁어와, 사용자에게 검색 결과로 보여준다. 그런데 AJAX 방식의 웹 어플리케이션의 HTML 파일은 뼈대만 있고 데이터는 없기 때문에 사이트의 정보를 긁어가기 어렵다.
- 뒤로가기 버튼 문제
  일반적으로 사용자는 뒤로가기 버튼을 누르면 이전 상태로 돌아갈 거라고 생각하지만, AJAX에서는 이전 상태를 기억하지 않기 때문에 사용자가 의도한 대로 동작하지 않는다. 따라서 뒤로가기 등의 기능을 구현하기 위해서는 별도로 History API를 사용해야한다.
  [지메일이 핫메일을 이긴 진짜 이유 (Ajax가 가져온 유저 인터페이스의 혁신)](https://sungmooncho.com/2012/12/04/gmail-and-ajax/)

---

# SSR vs CSR

### SSR (server side rendering)

- 웹페이지를 서버에서 렌더링한다.
- 브라우저가 서버url로 get 요청을 보내면 서버는 정해진 웹 페이지 파일을 브라우저로 전송한다.
- 서버의 웹페이지가 브라우저에 도착하면 완전히 렌더링 된다.
- 서버에서 웹 페에지를 브라우저로 보내기 전에, 서버에서 완전히 렌더링한다.
- 브라우저가 다른 경로로 이동할 때마다 서버는 이 작업을 다시 수행한다.

### CSR (client side rendering)

- 웹페이지를 클라이언트에서 렌더링 한다.
- 브라우저가 서버로 요청을 보내면, 웹 페이지의 골격이 될 단일 페이지를 클라이언트에 보낸다.
- 이 때 서버는 웹페이지와 JavaScript를 같이 보내며, 클라이언트가 웹페이지를 받으면, 함께 전달된 JavaScript 파일이 브라우저에서 웹페이지를 완전히 렌더링 된 페이지로 바꾼다.
- 웹페이지를 렌더링하는데 필요한 데이터는 API를 이용한다.
- 브라우저가 다른 경로로 이동할 때마다, 브라우저가 요청한 경로에 따라 페이지를 다시 렌더링 한다.

둘의 주요 차이점은 **페이지가 렌더링 되는 위치**이다.

**USE SSR**

- [SEO(search engine optimization)](https://en.wikipedia.org/wiki/Search_engine_optimization) 가 우선 순위인 경우
- 웹페이지 첫 화면 렌더링이 빠르게 필요한 경우 (ssr은 단일 파일 용량이 작다. )
- 웹페이지가 사용자와 상호작용이 적은 경우

**USE CSR**

- SEO가 우선 순위가 아닌 경우
- 사이트에 풍부한 상호 작용이 있는 경우. csr은 빠른 라우팅으로 강력한 사용자 경험을 제공한다.
- 웹 어플리케이션을 제공하는 경우 더 나은 사용자 경험(빠른 동적 렌더링)을 제공할 수 있다.

---

# Brouser Secury Model

### CORS (cross-origin resource sharing)

- 추가 HTTP 헤더를 사용하여 한 출처에서 실행 중인 웹 애플리케이션이 다른 출처이 선택한 자원에 접근할 수 있도록 권한을 부여하도록 브라우저에 알려주는 체제.
- 즉 **브라우저가 자발적으로 웹 어플리케이션을 사용하는 유저들을 보호하기 위한 정책**이다.

- Fetch API나 XMLHttpRequest 요청이 cors를 사용한다.

1. Simple requests
   - 일부 요청은 CORS preflight를 트리거 하지 않는다.
   - 다음 요건을 충족한다.
     - `GET`, `HEAD`, `POST` 중 하나의 메소드여야 한다.
     - User Agent가 자동으로 설정한 헤더만 사용 가능하다. 그 외에는 사용이 불가능하다 ..?
     - Content-type은 다음 값만 허용한다.
       - `application/x-www-form-urlencoded`
       - `multipart/form-data`
       - `text/plain`
2. Preflighted requests
   - 다음 요건을 충족한다.
   - Accept, Accept-language, Content-language, Content-Type, DPR, Downlink, Save-data,
3. Requests with credentials

### XSS

### CSRF
