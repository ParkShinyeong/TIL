express, express-session, sequelize 등을 이용해 간단한 로그인, 로그아웃 기능을 구현해보았다.

## 해야 할 것

## SERVER SIDE

- express-session 라이브러리를 이용해 **쿠키 설정**을 해준다.
  - domain: localhost
  - path: ‘/’
  - sameSite: none
  - HttpOnly: true
  - Secure: true
- **CORS 설정을 해준다.**

### 로그인

- userId와 password를 요청 받아 데이터베이스에 존재하는지 확인
- 결과가 존재하는 경우, 세션 객체에 userId를 저장한다.
- 결과가 존재하지 않으면 상태코드 400과 ‘not authorized’ 메시지를 내보낸다.

### 로그아웃

- 세션 아이디를 통해 고유한 세션 객체에 접근한다.
- 로그인 시 세션 객체에 저장한 값이 존재할 경우, 이미 로그인 한 상태로 판단한다.
- 세션 객체에 담긴 값의 존재 여부에 따라 응답을 구현한다.

### 사용자 정보

- 세션 객체에 담긴 값을 확인하고 값이 없으면, 상태코드 400, ‘not authorized’’ 메시지를 내보낸다.
- 세션 객체에 담긴 userId 값이 데이터베이스에 존재하면 상태코드 200, ‘OK’ 메세지와, 사용자 데이터를 내보낸다.

## CLIENT SIDE

### 로그인

- 서버에 로그인 요청을 보낸다. (POST, ’/users/login’)
- 로그인 요청에 성공하면, 로그인 상태를 바꾸어주고, 사용자 정보를 서버에 요청한다. (GET, ‘/users/userinfo’)
- 가져온 사용자 정보로 상태를 바꾸어준다.

### 마이페이지

- 서버에 로그아웃 요청을 보낸다. (POST, ‘/users/logout’)
- 로그아웃에 성공하면 로그인 상태를 바꾸어준다.

---

## SERVER SIDE

### index.js

세션의 쿠키 옵션을 설정해준다.

```jsx
app.use(
  session({
    secret: '@codestates',
    resave: false,
    saveUninitialized: true,
    **cookie: {
      domain: 'localhost',
      path: '/',
      maxAge: 24 * 6 * 60 * 10000,
      sameSite: 'none',
      httpOnly: true,
      secure: true,
    }**,
  })
);
```

CORS 설정을 해준다.

```jsx
app.use(cors({
  origin: 'https://localhost:3000',
  methods: ['GET', 'POST', 'OPTIONS'],
  **credentials: true,**
}));
```

이렇게 cors 설정을 해주는 이유는 **같은 origin에서 통신을 하는 경우, cookie가 알아서 request header에 들어가므로** 우리가 굳이 신경쓰지 않아도 된다.

그러나 현재 서버는 ‘https://localhost:4000/’이며 클라는 ‘https://localhost:3000’에서 실행되므로, 같은 origin이라고 볼 수 없다.

이렇게 **origin이 다른 http 통신에서 request header에 쿠키가 자동으로 들어가지 않는다**. 따로 설정을 해서 쿠키를 넘겨주어야 하므로, 다음 두 가지 작업을 해주어야 한다.

- 프론트 요청 헤더에 `withCredentials: true` 를 설정
- 서버의 응답헤더에 `Access-Control-Allow-Credentials: true` 를 설정

따라서 위의 cors 설정에 credentials: true로 설정해주었다.

### Login

```jsx
const { Users } = require('../../models');

module.exports = {
  post: async (req, res) => {
		// 데이터베이스에서 userId와 password와 일치하는 값을 찾는다.
    const **userInfo** = await Users.findOne({
      where: { userId: req.body.userId, password: req.body.password },
    });

		// 일치하는 값이 없으면 사용자 정보가 없으므로 상태코드 400을 리턴
    if (!userInfo) {
      res.status(400).send({ data: null, message: 'not authorized' });
    }
		//
		else {
      **req.session.save(function () {
        req.session.userId = userInfo.userId;
        res.json({ data: userInfo, message: 'ok' });
      });**
    }
  }
}
```

### Logout

```jsx
module.exports = {
 post: (req, res) => {
  // 요청받은 세션에 userId가 존재하는지 확인한다.
  if (!req.session.userId) {
   res.status(400).send({ data: null, message: "not authorized" });
  } else {
   // userId가 존재하면 세션을 없앤다.
   req.session.destroy();
   res.json({ data: null, message: "ok" });
  }
 },
};
```

### UserInfo

```jsx
const { Users } = require("../../models");

module.exports = {
 get: async (req, res) => {
  // 세션에 userId가 없다면 상태코드 400을 내보낸다.
  if (!req.session.userId) {
   res.status(400).send({ data: null, message: "not authorized" });
  }
  // 세션의 userId와 일치하는 데이터를 찾아서 응답으로 전달한다.
  else {
   const result = await Users.findOne({
    where: { userId: req.session.userId },
   }).catch((err) => res.json(err));
   res.status(200).json({ data: result, message: "ok" });
  }
 },
};
```
