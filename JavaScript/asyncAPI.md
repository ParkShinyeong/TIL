###타이머 관련 API

- setTimeout(callback, millisecond)
  - 인자: 실행할 콜백 함수, 콜백 함수 실행 전 기다려야 할 시간 (밀리초)
  - return value: 임의의 타이머 id
  ```jsx
  setTimeout(function () {
   console.log("2초 후 실행");
  }, 2000);
  // 123
  ```
- setInterval(callback, millisecond)

  - 일정 시간을 가지고 함수를 반복적으로 실행
  - 인자: 실행할 콜백 함수, 반복적인 함수를 실행하기 위한 시간 간격 (밀리초)
  - return value: 임의의 타이머 id

  ```jsx
  setInterval(function () {
   console.log("1초마다 실행");
  }, 1000);
  // 345
  ```

- clearInterval(timerId)
  - 반복 실행 중인 타이머 종료
  - 인자: 타이머 아이디
  - return value: 없음
  ```jsx
  const timer = setInterval(function () {
   console.log("1초마다 실행");
  }, 1000);
  clearInterval(timer);
  // 더 이상 반복 실행되지 않음
  ```
- clearTimeout(timerId) → `setTimeout`에 대응한다.

---

#

### Node.js

로컬 환경에서 자바스크립트를 실행할 수 있는 자바스크립트 런타임이다. 브라우저에서 불가능한 몇 가지 일이 가능하다.

**Node.js 내장 모듈 사용법**

자바스크립트 코드 가장 상단에 `require` 구문을 사용하여 다른 파일을 불러온다.

```jsx
const fs = require("fs"); // 파일 시스템 모듈 불러오기
const dns = require("dns"); // DNS 모듈을 불러오기
```

**3rd-party 모듈 사용법**

**3rd-party 모듈**은 해당 프로그래밍 언어에서 공식적으로 제공하는 **빌트인 모듈(built-in module)**이 아닌 모든 외부 모듈을 일컫는다.

ex) Node.js 에서 underscore은 공식 문서에 없는 모듈이다. → 써드 파티 모듈

```jsx
//터미널에서 다음과 같이 입력하여 underscore 모듈을 설치한다.
npm install underscore
```

```jsx
//마찬가지로 자바스크립트 코드 가장 상단에 require 구문을 사용하여 불러온다.
const_ = require("underscore");
```

#

### fetch API 를 이용한 네트워크 요청

비동기 요청의 가장 대표적인 사례 → 네트워크 요청!

네트워크를 통해 이루어지는 요청은 형태가 다양한데, 그 중 URL로 요청하는 경우가 가장 흔하다.

ex) 네이버 같은 포털 사이트에서 최신 뉴스, 날씨와 같은 시시각각 변하는 정보와 고정적인 정보들이 있다.

변하는 정보는 동적으로 데이터를 받아서 해당 정보만 업데이트한다. 여기서 요청 API를 사용한다.

이 중 대표적인 fetch API를 알아보자

fetch API 사용법 ([mdn](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch))

```jsx
let url = "http://shinyeong/shinyeong_page/weriskfehifwlekf?tabID=최신뉴스";

fetch(url)
  .then((response) => response.json()) // 자체적으로 json() 메소드가 있어, 응답을 JSON 형태로 변환시켜서 다음 Promise로 전달
  .then((json) => console.log(json) // 콘솔에 json을 출력
  .catch((error) => console.log(error)); // 에러가 발생한 경우, 에러를 띄움
```
