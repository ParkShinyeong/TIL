node.js를 이용하여 백엔드를 구축한다

## Cors 다시 보기!

[mdn](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)

cors(cross origin resource sharing) - 교차 출처 리소스 공유 (↔ SOP - [동일 출처 정책](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin_policy))

- 추가 HTTP 헤더를 사용하여 한 출처에서 실행 중인 웹 어플리케이션이 다른 출처에서 선택한 리소스에 접근할 수 있는 권한을 부여하도록, 브라우저에 알려주는 체제이다.
- 웹 애플리케이션은 리소스가 자신의 출처(domain, scheme, port)와 다를 때 서버 HTTP 요청을 실행한다.

서로 다른 출처끼리 요청을 주고 받는 건 안되는 것이 기본 값이다. 브라우저에서 애초부터 금지시켰다.

웹 생태계가 다양해지면서 다양한 서비스 간에 보다 자유롭게 데이터가 주고받아질 필요가 생겼다. 그런데 보안 상의 이유로, 이런 사이트 간의 요청을 브라우저가 막고 있는데, 이를 해결하기 위해 **어떤 기준을 충족시키면 리소스 공유가 되도록 만들어진 매커니즘이 cors**이다.

즉, 다른 출처 간에 리소스를 공유할 수 있도록 하는 것이 cors이다.

(출처란? → 보내고 받는 각각의 위치, 즉 웹사이트와 API 주소)

기준? → 요청 받는 서버쪽에서 이걸 허락할 다른 출처들을 미리 명시해준다.

요청에 Origin이라는 header를 붙여준다. 이 헤더에는 요청하는 쪽의 scheme, domain, port가 담긴다.

shceme → 요청할 자원에 접근하는 방법 (프로토콜), http, ftp 등등

#

**Simple request**

- 토큰 등 사용자 식별 정보가 담긴 요청 등
- 보내는 측 요청 옵션에 credentials 항목을 true로 세팅하고, 받는 쪽에서도 보내는 쪽의 출처 - 웹페이지 주소를 정확하게 명시한 다음 Access-Control-Allow-Credentials 항목을 true로 맞춰줘야 한다.
- 다음 요건을 충족한다.
  - `GET`, `HEAD`, `POST` 중 하나의 메소드여야 한다.
  - User Agent가 자동으로 설정한 헤더만 사용 가능하다. 그 외에는 사용이 불가능하다 ..?
  - Content-type은 다음 값만 허용한다.
    - `application/x-www-form-urlencoded`
    - `multipart/form-data`
    - `text/plain`

이 값이 충족되면 preflight request를 트리거하지 않는다.

**Preflight request**

- Put, Delete 등 요청들은 본 요청을 보내기 전에 Preflight 요청을 먼저 보내서 본 요청이 안전한지 확인하고, 여기서 허락이 떨어져야 본 요청을 보낼 수 있다.
  → 서버 데이터에 영향을 줄 수 있는 요청들이므로 요청 자체를 보내기 전에 먼저 허용 여부를 검증한다.
- 다음 요건을 충족한다.
- Accept, Accept-language, Content-language, Content-Type, DPR, Downlink, Save-data,

##

##

## HTTP 트랜잭션 해부

- Node.js HTTP 처리 과정을 이해한다.
- HTTP 요청이 어떻게 동작하는지 안다.

### 서버 생성

- 모든 node 웹 서버 어플리케이션은 웹 서버 객체를 만든다. → `createServer`

```jsx
const http = require("http");
const server = http.createServer((request, response) => {});
//create가 반환한 server 객체는 eventemitter이고 여기서는 server 객체를 생성하고 리스너를 추가하는 축약 문법을 사용
//const server = http.createServer();
//server.on('request', (request, response) => {})
```

이 서버로 오는 HTTP 요청마다 createServer에 전달된 함수가 한 번씩 호출된다.

- HTTP 요청이 서버에 오면 node가 트랜잭션을 다루려고 request, response 객체를 전달하여 요청 핸들러 함수를 호출한다.

**트랜잭션(Transaction)**은 데이터베이스의 상태를 변환시키는 하나의 논리적 기능을 수행하기 위한 작업의 단위 또는 한꺼번에 모두 수행되어야 할 일련의 연산

- 요청을 실제로 처리하려면 listen 메서드가 server 객체에서 호출되어야 한다 .

##

##

## 요청 객체(request)

### 메서드, URL, 헤더

- 요청을 처리할 때, 먼저 메서드와 URL을 확인한 후 이와 관련된 적절한 작업을 실행하려고 한다.
- Node가 request 객체에 유용한 프로퍼티를 넣어두었으므로, 쉽게 작업할 수 있다.

```jsx
const { method, url, headers } = request;
const userAgent = headers["user-agent"];
```

- 메서드: 일반적인 HTTP 메서드 / 동사
- url: 전체 URL에서 서버, 프로토콜, 퐅를 제외한 것 (세번째 슬래시 이후 나머지 전부)
- header: 모든 헤더는 소문자로 표현

### 요청 바디

- POST, PUT 요청을 받을 때 애플리케이션 **요청 바디**는 중요하다.
- 핸들러에 전달된 request 객체는 `[ReadableStream](https://nodejs.org/api/stream.html#stream_class_stream_readable)` 인터페이스를 구현하고 있다.
- 이 스트림에 이벤트 리스너를 등록하거나, 다른 스트림에 파이프로 연결할 수 있다.
- 스트림의 'data' 와 'end' 이벤트에 이벤트 리스너를 등록해서 데이터를 받을 수 있다.

- 각 'data' 이벤트에서 발생시킨 청크는 **Buffer**이다. 이 청크는 **문자열 데이터**이며, 이 데이터를 배열에 수집한 다음, 'end' 이벤트에서 이어 붙인 다음, 문자열로 만든다.

```jsx
let body = [];
request
 .on("data", (chunk) => {
  // 배열에 수집
  body.push(chunk);
 })
 .on("end", () => {
  // end 이번트에서 이어 붙이고, 문자열로 만듦
  body = Buffer.concat(body).toString();
 });

// 이 코드가 장황하게 느껴질 수도 있는데, npm에 concat-stream이나 body같은 모듈로 이 로직을 감출 수 ㅇ
```

❓ **Chunk [(관련 링크)](https://www.geeksforgeeks.org/what-is-chunk-in-node-js/)**

데이터는 stream 형식으로, 각각의 요청으로 인해 서버에서 클라이언트로 전송된다. stream 은 chunk를 포함한다. **Chunk는 클라이언트에서 서버로 보내지는 데이터를 조각낸 것**으로 stream의 Buffer을 만들기 위해 모든 청크 concept을 교환한 다음 Buffer가 전체 데이터로 변환된다.

**request 객체**는 chunk의 데이터를 핸들링하기 위해 사용된다.

```jsx
request.on("eventName", callback);
```

### 오류

- request 스트림의 오류가 발생하면 스트림에서 'error' 이벤트가 발생하면서 오류를 전달한다.
- 이베느에 리스너가 등록되어 있지 않다면 Node.js 프로그램을 종료시킬수도 있는 오류를 던질 것이다.
- 그러므로 단순히 오류를 로깅만 하더라도 요청 스트림에 'error' 리스너를 추가해야 한다!
- (하지만 HTTP 오류 응답을 내보내는 것이 좋을 것이다!)

```jsx
request.on("error", (err) => {
 console.error(err.stack);
});
```

### 요청 최종

서버를 생성하고 요청의 메서드, URL , 헤더, 바디를 가져왔다.

```jsx
const http = require("http");

http
 .createServer((request, response) => {
  const { headers, method, url } = request;
  let body = [];
  request
   .on("error", (err) => {
    console.err(err);
   })
   .on("data", (chunk) => {
    body.push(chunk);
   })
   .on("end", () => {
    body = Buffer.concat(body).toString();
   });
 })
 .listen(8080); // 이 서버를 활성화하고 8080 포트로 받는다.
```

예제를 실행하면 요청을 받을 수 있지만, 요청에 응답하지는 않는다. 사실 웹 브라우저에서 이 예제에 접근하면 클라이언트에 돌려보내는 것이 없으므로 요청이 타임아웃에 걸릴 것이다.

##

##

## 응답 객체 (response)

- 응답 객체는 ServerResponse의 인스턴스이면서 `[WritableStream](https://nodejs.org/api/stream.html#stream_class_stream_writable)`이다.

### HTTP 상태코드

- 따로 설정하지 않으면 HTTP 상태코드는 항상 200이다.
- 상태 코드를 변경하려면 `statusCode` 프로퍼티를 설정한다.

```jsx
response.statusCode = 404; //클라이언트에게 리소스를 찾을 수 없다.
```

### 응답 헤더 설정

- 암묵적인 헤더

- setHeader 메서드로 헤더를 설정한다.
- 응답 헤더를 설정할 때는 대소문자는 중요하지 않는다.
- 헤더를 여러 번 설정하면 마지막에 설정한 값을 보낸다.

```jsx
resposne.setHeader("Content-Type", "application/json");
response.setHeader("X-Powered-By", "bacon");
```

**명시적인 헤더 데이터 전송**

- 지금까지 설명한 헤더와 상태코드를 설정하는 메서드는 **암묵적인 헤더** 를 사용하고 있다고 가정한다.
- 이는 바디 데이터를 보내기 전 적절한 순간에 헤더를 보내는 일을 노드에 의존하고 있다
- 원한다면 명시적으로 응답 스트림에 헤더를 작성할 수 있다. → `writeHead`메서드를 이용
- 스트림에 상태 코드와 헤더를 작성한다.

```jsx
response.writeHead(200, {
 "Content-Type": "application/json",
 "X-Powered-By": "bacon",
});
```

암묵적이든, 명시적이든 일단 헤더를 설정하고 나면 응답 데이터를 전송할 준비가 된 것이다

### 응답 바디 전송

- response 객체는 WritableStream 이므로 클라이언트로 보내는 응답 바디는 일반적인 스트림 메서드를 사용해서 작성한다.

```jsx
response.write("<html>");
response.write("<body>");
response.write("<h1>Hello, world!</h1>");
response.write("</body>");
response.write("</html>");

response.end();

// ==> 더 간단하게 작성할 수도 있다.
response.end("<html><body><h1>Hello, world!</h1></body></html>");
```

- 바디에 데이터 청므를 작성하기 전에 상태 코드와 헤더를 설정해야 한다. HTTP 응답에서 바디 전에 헤더가 있으므로 이는 이치에 맞다.

### 오류

- response 스트림도 request 스트림 오류와 마찬가지로 'error'이벤트를 발생시킬 수 있고, 때로는 이 오류를 처리한다.

```jsx
const http = require("http");

http
 .createServer((request, response) => {
  //요청
  const { headers, method, url } = request;
  //요청 바디
  let body = [];
  request
   .on("error", (err) => {
    console.error(err);
   })
   .on("data", (chunk) => {
    body.push(chunk);
   })
   .on("end", () => {
    body = Buffer.concat(body).toString();

    //응답
    response.on("error", (err) => {
     console.error(err);
    });
    //응답 헤더
    response.statusCode = 200;
    response.setHeader("Content-Type", "application/json");
    // 주의: 위 두 줄은 다음 한 줄로 대체할 수도 있습니다.
    // response.writeHead(200, {'Content-Type': 'application/json'})
    //응답 바디
    const responseBody = { headers, method, url, body };

    response.write(JSON.stringify(responseBody));
    response.end();
    // 주의: 위 두 줄은 다음 한 줄로 대체할 수도 있습니다.
    // response.end(JSON.stringify(responseBody))
   });
 })
 .listen(8080);
```

##

## 예제

- 요청 메서드가 POST이고, URL이 `/echo` 인 경우
- 이 조건이 아닌 경우 404를 응답한다.

```jsx
const http = require("http");

http
 .createServer((request, response) => {
  if (request.method === "POST" && request.url === "/echo") {
   let body = [];
   request
    .on("data", (chunk) => {
     body.push(chunk);
    })
    .on("end", () => {
     body = Buffer.concat(body).toString();
     response.end(body);
    });
  } else {
   response.statusCode = 404;
   response.end();
  }
 })
 .listen(8080);
```

→ 이 방법으로 URL을 검사함으로써 '라우팅'을 하고 있다.

- switch 문으로 간단히 라우팅을 할 수 도 있고, [express](https://www.npmjs.com/package/express) 같은 프레임워크로 복잡한 라우팅을 할 수도 있다.
- 라우팅 처리를 하는 무언가를 찾고 있다면 [router](https://www.npmjs.com/package/router)을 사용해보자!

request 객체는 ReadableStream이고, response 객체는 WritableStream 이다.

= 데이터를 한 스트림에서 다른 스트림으로 직접 연결하는 pipe를 사용할 수 있다!

```jsx
const http = require("http");
http
 .createServer((request, response) => {
  if (request.method === "POST" && request.url === "/echo") {
   request.pipe(response);
  } else {
   response.statusCode = 404;
   response.end();
  }
 })
 .listen(8080);
```

이것이 스트림이다!!! (먼소리야;;)

요청 스트림에서 오류 처리하기

- 오류를 `stderr`에 로깅하고, 400 상태코드를 내보낸다.

```jsx
const http = require("http");
http
 .createServer((request, response) => {
  request.on("error", (err) => {
   console.error(err);
   response.statusCode = 400;
   response.end();
  });
  response.on("errpr", (err) => {
   console.error(err);
  });

  if (request.method === "POST" && request.url === "/echo") {
   request.pipe(response);
  } else {
   response.statusCode = 404;
   response.end();
  }
 })
 .listen(8080);
```

---

##

[localhost](https://ko.wikipedia.org/wiki/Localhost): 컴퓨터 네트워크에서 사용하는 '루프백 호스트명'으로, **자신의 컴퓨터**나 컴퓨터에 대한 IP 연결 또는 호출을 설정하는데 사용된다. localhost 자체도 도메인 이름이 될 수 있으며, 자체 IP 주소가 있다.

IPv4에서 IP 주소: 127.0.0.1

IPv6 : ::1

HTTP method

**OPTIONS**

- 목표 리소스와의 통신 옵션을 설명하기 위해 사용. 클라이언트는 OPTIONS 메소드의 URL을 특정지을 수 있다. aterisk(\*)을 통해 서버 전체를 선택할 수 있다.
- **CORS**에서 OPTIONS 메소드를 통해 **preflight 요청**, 즉 사전 요청을 보내, **서버에 해당 parameter을 포함한 요청을 보내도 되는지**에 대한 응답을 줄 수 있게 한다. ([mdn](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/OPTIONS#preflighted_requests_in_cors))
- Access-Control-Request-Method
  - 헤더는 프리플라이트 요청의 일부분으로 서버에게 실제 요청이 전달될 때 POST 요청 메소드로 전달될 것임을 명시한다.
- Access-Control-Request-Headers
  - 헤더는 서버에게 실제 요청이 전달될 때 `X-PINGOTHER`와 `Content-Type` custom header와 함께 전달될 것임을 명시한다.
  서버는 이러한 요구사항들에 맞춰 요청을 수락할 것인지 정할 수 있다.
