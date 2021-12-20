**MERN stack** ⇒ 자바스크립트에서 인기있는 프레임 워크 (MongoDB, Express, React, Node.js)

이 중 express는 node.js환경에서 웹서버, API서버를 제작하기 위해 사용되는 인기있는 프레임 워크이다.

#

### Express 장점

##

1. **라우팅(Routing) : 메소드와 URL로 분기점을 만드는 것**

- Express는 프레임워크 자체에 **라우터 기능**을 제공한다.
- http모듈을 사용한 것보다 훨씬 직관적이다. → (라우팅 할 때 if문을 쓰지 않아도 된다.)

```jsx
const router = express.Router()

// http 모듈 => if(req.method === 'GET' && req.url === '/lower') { ... }
router.**get('/lower'**, (req, res) => {
  res.send(data)
})

//http 모듈 => if(req.method === 'POST' && req.url === '/lower') { ... }
router.**post('/lower'**, (req, res) => {
  //do something
})
```

##

2. **middleware 을 이용해 body를 받기 수월하다.**

- http 모듈에서 HTTP body (payload)를 받을 때에는 **Buffer을 조합**해서 다소 복잡한 방식으로 body를 얻을 수 있다.

```jsx
let body = [];
request
 .on("data", (chunk) => {
  body.push(chunk);
 })
 .on("end", () => {
  //body 변수에 문자열 형태로 payload가 담겨져 있다.
  body = Buffer.concat(body).toString();
 });
```

- **body-parser 미들웨어**를 사용하면 이 과정을 간단하게 처리할 수 있다

```jsx
const bodyParser = require("body-parser");
const jsonParser = bodyParser.json();

app.post("/api/users", jsonParser, function (req, res) {
 //req.body에 JSON의 형태로 payload가 담겨 있다.
 console.log(req.body);
});
```

##

3. CORS 를 더 쉽게 사용할 수 있다.

- 순수 node.js 코드에 CORS 헤더를 사용하려면, 응답 객체의 `writeHead` 메소드를 사용해야 한다.
- 메소드를 이용해도 `Access-Control-Allow-*` 헤더를 매번 정의해야 한다.
- `OPTIONS` 메소드에 대한 라우팅도 따로 구현해야 한다.

```jsx
const defaultCorsHeader = {
 "Access-Control-Allow-Origin": "*",
 "Access-Control-Allow-Methods": "GET, POST, PUT, DELETE, OPTIONS",
 "Access-Control-Allow-Headers": "Content-Type, Accept",
 "Access-Control-Max-Age": 10,
};

// 생략
if (req.method === "OPTIONS") {
 res.writeHead(201, defaultCorsHeader);
 res.end();
}
```

- cors 미들웨어를 사용하면 이 과정을 간단하게 처리할 수 있다.

```jsx
const cors = require("cors");

app.use(cors()); // 모든 요청에 대해 CORS 허용
```

- 특정 요청에만 cors 모듈을 적용할 수 있다.

```jsx
const cors = require("cors");

// 생략
// 특정 요청에 대해 CORS 허용
app.get("/products/:id", cors(), function (req, res, next) {
 res.json({ msg: "This is CORS-enabled for a Single Route" });
});
```
