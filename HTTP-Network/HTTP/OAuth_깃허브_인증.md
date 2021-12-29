### 깃허브 OAuth 인증 방식 구현하기

[참고 사이트]

[깃허브에 내 application 등록하기](https://www.oauth.com/oauth2-servers/accessing-data/create-an-application/)

[깃허브 앱에서 사용자 식별 및 권한 부여](https://docs.github.com/en/developers/apps/building-github-apps/identifying-and-authorizing-users-for-github-apps#2-users-are-redirected-back-to-your-site-by-github) (authorization Code, access token 요청할 때)

`[location.assign()](https://developer.mozilla.org/en-US/docs/Web/API/Location/assign)`

**`octokit`** - [(참고1)](https://github.com/octokit/core.js#readme) / [(참고2)](https://docs.github.com/en/rest/reference/users#get-the-authenticated-user) 인증된 사용자 정보를 가져오는 모듈 ..?

## 깃허브에 내 application 등록하기

[(참고 사이트)](https://www.oauth.com/oauth2-servers/accessing-data/create-an-application/)

참고 사이트를 활용해서, Oauth 앱을 등록한다. (깃허브-셋팅- Developer settings-OAuth apps)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/508d7ae0-4708-4697-90d9-ef42dbcf6103/Untitled.png)

- **application name** - 내가 만든 어플리케이션 이름을 지정해준다.
- **client_secret**
  생성할 때 한번만 볼 수 있으므로 주의하자! (client_secret은 항상 비밀로 지켜져야 한다. → .env 파일에 저장 )
- **hompage url**
  **깃허브가 사용자 인증을 마친 후 리디렉트 할 url이다**. - Oauth 매커니즘이 인증 과정을 끝낸 후 리디렉션을 통해 다시 내 앱으로 이동하는 원리이다.

## 환경 설정

Github에서 제공하는 client_id와 client_secret을 .env 파일에 넣어준다.

```jsx
GITHUB_CLIENT_ID=
GITHUB_CLIENT_SECRET=
```

## 서버 설정

### app.js

```jsx
const express = require("express");
const app = express();
const cors = require("cors");
const PORT = process.env.PORT || 3001;

const handleCallback = require("./controller/callback");
const handleImages = require("./controller/images");

app.use(express.json());

app.use(cors({ origin: true }));

app.post("/callback", handleCallback);

app.get("/images", handleImages);

app.listen(PORT, () => {
 console.log(`listening on port ${PORT}`);
});

module.exports = app;
```

express를 사용해서 작성하였다.

‘/callback’으로 post 요청이 오면, handleCallback 컨트롤러와 연결하고, ‘/images’로 get 요청이 오면, handleImages 컨트롤러와 연결한다.

### **/Callback 컨트롤러**

/callback으로 post 요청이 오면, 요청에 담긴 authorizationCode를 바탕으로 access token을 받아올 수 있도록 해준다.

[access token을 요청할 때 참고 사이트](https://docs.github.com/en/developers/apps/building-github-apps/identifying-and-authorizing-users-for-github-apps#2-users-are-redirected-back-to-your-site-by-github)

- callback.js

```jsx
require("dotenv").config();

const clientID = process.env.GITHUB_CLIENT_ID;
const clientSecret = process.env.GITHUB_CLIENT_SECRET;
const axios = require("axios");
module.exports = (req, res) => {
 // 클라이언트에서 req의 body에 authorization Code가 들어온다.
 const authorization = req.body.authorizationCode;
 // authorizationCode를 이용해 깃허브에 access token을 요청한다.
 // parameter로 client_id, client_secret, code(authorization code)를 넣어준다.
 axios
  .post(
   "https://github.com/login/oauth/access_token",
   {
    client_id: clientID,
    client_secret: clientSecret,
    code: authorization,
   },
   {
    headers: { accept: "application/json" },
   }
  )

  .then((result) => {
   // access token을 받아오면, 토큰을 응답으로 보내준다.
   res.status(200).send({ accessToken: result.data.access_token });
  });
};
```

### **/images 컨트롤러**

받아온 access token을 확인 후, local에 저장된 resource images를 클라이언트로 보내주는 라우터이다.

- 로컬 리소스

```jsx
module.exports = [
 {
  file: "codestates.png",
  blob:
   "data:image/png;base64,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",
 },
 {
  file: "heart.png",
  blob:
   "data:image/png;base64,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",
 },
 {
  file: "github.png",
  blob:
   "data:image/png;base64,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",
 },
];
```

로컬에 다음과 같이 이미지 파일이 저장되어 있다. 이를 클라이언트로 보내준다.

- **images.js**
  accessToken은 클라이언트에서 header의 authorization에 담겨서 온다. accessToken이 존재하면 이미지 파일을 응답으로 보내주도록 하자

```jsx
const images = require("../resources/resources");

module.exports = (req, res) => {
 const accessToken = req.headers.authorization;
 // accessToken이 없을 때
 if (!accessToken) {
  return res.status(403).send({ message: "no permission to access resources" });
 }
 // accessToken이 있을 때
 res.status(200).send({ images: images });
};
```

## 클라이언트

### Login Component

- 깃허브의 사용자 인증 링크로 이동해 **Authorization 코드를 받아온다.**

```jsx
import React, { Component } from 'react';

class Login extends Component {
  constructor(props) {
    super(props)
    this.socialLoginHandler = this.socialLoginHandler.bind(this)
    // 깃허브에 authorization Code를 요청하는 url이다. client_id 파라미터를 가지고 있어야 한다.
		**this.GITHUB_LOGIN_URL = `https://github.com/login/oauth/authorize?client_id={ 내 앱 클라이언트 id }`**
  }

	// 버튼을 클릭하면 this.socialLoginHandler가 실행된다.
  socialLoginHandler() {
		// location.assign(url)메소드는 전달인자로 들어온 url을 로드 및 표시한다.
		//여기서는 깃허브에 권한을 요청하는 url로 연결해주었다.
    **window.location.assign(this.GITHUB_LOGIN_URL)**
  }

  render() {
    return (
      <div className='loginContainer'>
        OAuth 2.0으로 소셜 로그인을 구현해보세요.
        <img id="logo" alt="logo" src="https://image.flaticon.com/icons/png/512/25/25231.png" />
        <button
					// 버튼을 클릭하면 this.socialLoginHandler가 실행된다.
          **onClick={this.socialLoginHandler}**
          className='socialloginBtn'
        >
          Github으로 로그인
          </button>
      </div>
    );
  }
}

export default Login;
```

`[location.assign()](https://developer.mozilla.org/en-US/docs/Web/API/Location/assign)`

### App Component

- **authorization code를 받아왔다면, 해당 코드를 server에 전달**해 access token으로 받아올 수 있다.
- 받아온 access token으로 app의 state에 저장한 후, mypage 컴포넌트에 props로 받아 활용한다.

```jsx
import React, { Component } from 'react';
import { BrowserRouter as Router } from 'react-router-dom';
import Login from './components/Login';
import Mypage from './components/Mypage';
import axios from 'axios';

class App extends Component {
  constructor() {
    super();
    this.state = {
			// 로그인 상태와, accessToken을 state에 저장한다.
      isLogin: false,
      accessToken: ''
    };
    this.getAccessToken = this.getAccessToken.bind(this);
  }

	// 서버에 /callback으로 post 요청을 보내 access Token을 받아오는 메소드이다.
  async getAccessToken(authorizationCode) {
		**const result = await axios
      .post('http://localhost:3001/callback', {
        authorizationCode
      })**

    // access Token을 받아오면 state 상태의 값을 바꾸어준다.
		// 로그인 성공 => true, accessToken => 빈 문자열에서 받아온 accessToken값
    **this.setState({
      isLogin: true,
      accessToken: result.data.accessToken
    })**
  }


// authorization server로 부터 클라이언트로 리디렉션 되었을 때, authorization code를 받아온다.
  componentDidMount() {
    const url = new URL(window.location.href)
		// authorization Code는 http://localhost:3000/?code=5e52fb85d6a1ed46a51f 에서 code 파라미터에 해당한다.
    const authorizationCode = url.searchParams.get('code')
    if (authorizationCode) {
			// 받아온 authorizationCode를 getAccessToken 메소드에 넣어준다.
      this.getAccessToken(authorizationCode)
    }
  }

  render() {
    const { isLogin, accessToken } = this.state;
    return (
      <Router>
        <div className='App'>
					{// 로그인 상태에 따라 mypage 컴포넌트를 띄우거나, login 컴포넌트를 띄운다.}
          {isLogin ? (
							// 받아온 accessToken은 Mypage에 props로 전달한다.
            <Mypage accessToken={accessToken} />
          ) : (
              <Login />
            )}
        </div>
      </Router>
    );
  }
}

export default App;
```

`[url.searchParams.get()`](https://developer.mozilla.org/ko/docs/Web/API/URL/searchParams)

url 중 get의 인자에 들어간 쿼리 매개변수에 접근할 수 있다.

예를 들면

`http://example.com/**?name=shinyeong&age=23**`

라는 url 페이지가 있을 때 name과 age 매개변수를 다음과 같이 가져올 수 있다.

```jsx
// http://example.com/?name=shinyeong&age=23
let params = new URL(document.location).searchParams;
let name = params.get("name"); // shinyeong
let age = parseInt(params.get("age")); //23
```

약간 헷갈리는데

login 창에서 accesstoken 을 요청하고 ⇒ 인증 후 ⇒ http://localhost:3000으로 다시 리디렉션 (app.js) ⇒ 리디렉션 된 url의 parameter에 authorization Code 존재 ⇒ authorization Code로 access token 발급 요청

을 하는 것이다.

그래서 accesstoken 요청은 login 컴포넌트에서 하고, 받아온 authorization code로 accesstoken을 요청하는 것은 app.js에서 하는 것이다.

### Mypage Component

받아온 accesstoken으로 리소스에 대한 API 요청을 할 수 있다.

- 먼저 github api 요청에 access token을 함께 보내 유저 정보를 받아온다.
- local server에 저장된 이미지를 받아온다. (’GET’, ‘/images’)

받아온 리소스들로 Mypage를 완성한다.

```jsx
import React, { Component } from "react";
import axios from 'axios';
const { Octokit } = require("@octokit/core");

class Mypage extends Component {

  constructor(props) {
    super(props);
		// state에 이미지 정보와, 사용자의 이름, login id, 레포지토리 주소, public repository 개수를 담는다.
    **this.state = {
      images: [],
      name: '',
      login: '',
      html_url: '',
      public_repos: 0,
    }**
  }

	// Github API를 통해 사용자 정보를 받아온다.
	// 여기서 octokit이라는 모듈을 사용해주었다.
  async getGitHubUserInfo() {
    const **octokit** = new Octokit({ auth: this.props.accessToken });
    const result = **await octokit.request('GET /user');**
    const {name, login, html_url, public_repos } = result.data;
		받아온 사용자 정보로 state를 바꾸어준다.
    this.setState({
      name,
      login,
      html_url,
      public_repos
    })

  }

	// /images로 get 요청을 보내서 로컬의 이미지 정보를 불러온다.
  async getImages() {
    const result = await axios.get("http://localhost:3001/images", {
			// 헤더에 access Token을 담아서 보내준다. (인증된 사용자임을 알려준다.)
      headers: {
        authorization: `token ${this.props.accessToken}`,
      }
    })
		// 받아온 이미지 정보로 state를 바꾸어준다.
    this.setState({
      images: result.data.images
    })
  }

  componentDidMount() {
    this.getGitHubUserInfo()
    this.getImages()
  }

  render() {
    const { accessToken } = this.props
		// 만약 accessToken이 없는 경우 로그인에 성공하지 못한 상태이므로, 로그인이 필요하다는 문구를 띄워준다.
    if (!accessToken) {
      return <div>로그인이 필요합니다</div>
    }

    return (
      <div>
        <div className='mypageContainer'>
          <h3>Mypage</h3>
          <hr />

          <div>안녕하세요. <span className="name" id="name">{**this.state.name**}</span>님! GitHub 로그인이 완료되었습니다.</div>
          <div>
            <div className="item">
              나의 로그인 아이디:
              <span id="login">{**this.state.login**}</span>
            </div>
            <div className="item">
              나의 GitHub 주소:
              <span id="html_url">{**this.state.html_url**}</span>
            </div>
            <div className="item">
              나의 public 레포지토리 개수:
              <span id="public_repos">{**this.state.public_repos**}</span>개
            </div>
          </div>

          <div id="images">
            **{this.state.images.map((image, idx) => {
              return <img src={image.blob} key={idx} />
            })}**
          </div>
        </div>
      </div >
    );
  }
}

export default Mypage;
```

**`octokit` - [(참고1)](https://github.com/octokit/core.js#readme) [(참고 2)](https://docs.github.com/en/rest/reference/users#get-the-authenticated-user)**

```jsx
$ npm install @octokit/core
```

```jsx
const { Octokit } = require("@octokit/core");
const octokit = new Octokit({ auth: `personal-access-token123` });
const response = await octokit.request("GET /orgs/{org}/repos", {
 org: "octokit",
 type: "private",
});
```
