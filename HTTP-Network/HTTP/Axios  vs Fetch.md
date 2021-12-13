Axios와 Fetch 는 기본적으로 HTTP 프로토콜을 이용해 웹 어플리케이션이 서버와 소통하기 위한 API이다.

이때까지 Fetch 만 사용해오다가, 세션, 토큰을 배울 때 axios를 접하게 되었다.

Axios와 Fetch의 차이점은 무엇이고, 사용할 때 각각 어떤 장점과 단점을 가지고 있을까?

사실 Fetch와 Axios는 기능적으로는 매우 유사하다.

## Fetch

Fetch는 내장 API이며, fetch() 메소드를 통해 요청을 보낼 수 있다.

- HTTP 파이프라인 일부에 접근하고 조작할 수 있는 자바스크립트 인터페이스를 제공한다.
- promise 기반으로 다루기 쉽다.
- 내장 라이브러리이기 때문에 사용하는 프레임워크가 안정적이지 않을 때 사용하기 좋다. (ex) react-native)
- Axios에 비해 브라우저 호환성이 상대적으로 떨어진다. (internet explore의 일부 버전이 fetch를 지원하지 않는다.)
- Axios에 비해 기능이 부족하다.

```jsx
const url ='http://localhost:3000/login'
const option ={
  method: 'POST',
  header: {
    'Accept': 'application/json',
    'Content-Type': 'application/json'; charset=UTP-8' }.
  body: JSON.stringify({
      email:'syn123@gmail.com',
      password:'psy1234'
  })

fetch(url, ooptions)
  .then(res => console.log(res))
```

### Axios

- 외부 API이며, 모듈을 따로 설치해주어야 사용할 수 있다.
- response timeout 처리 방법이 있다. (fetch에는 없다.)
- promise기반으로 다루기 쉽다.
- 크로스 브라우징에 신경을 많이 써서 브라우저 호환성이 뛰어나다.
- 내장된 XSRF 보호 기능을 제공한다.

```jsx
axios
  .post(
		'http://localhost:3000/login',
		{'syn123@gmail.com', 'psy1234'},
		{ headers: {
				'Content-Type': 'application/json',
				'Accept': 'application/json',
			}
  )

.then((res) => {
  console.log(res)
  })


```

[(참조)](https://www.geeksforgeeks.org/difference-between-fetch-and-axios-js-for-making-http-requests/)
