2021/10/21

**리액트 간단 복습!**

- [React로 생각하기](https://ko.reactjs.org/docs/thinking-in-react.html)
- 단일 책임 원칙에 따른 구분
  하나의 컴포넌트는 한가지 일만 한다.
- 단방향 데이터 흐름(one-way data flow) [(React 공식문서)](https://ko.reactjs.org/docs/state-and-lifecycle.html#the-data-flows-down)
  데이터는 위에서 아래로 흐른다. (데이터의 흐름은 하향식)

역방향 데이터 흐름?

→ 하위 컴포넌트의 어떤 이벤트로 인해 상위 컴포넌트의 상태가 바뀌는 것은 마치 역방향 데이터 흐름같이 이상하게 들릴 수 있다.

**상태 끌어올리기**

단방향 데이터 흐름 원칙에 부합하는 해결 방법으로, 상위 컴포넌트의 '상태를 변경하는 함수를 props를 통해 하위 컴포넌트로 전달하고, 이 함수를 하위 컴포넌트가 실행한다.

```jsx
import React, { useState } from "react";

//부모 컴포넌트
export default function ParentComponent() {
  //상태변경
  const [value, setValue] = useState("날 바꿔줘!");
  const handleChangeValue = (newvalue) => {
    setValue(newvalue);
  };

  return (
    <div>
      <div>값은 {value} 입니다</div>
      {/*props로 상태변경함수를 전달해준다.*/}
      **<ChildComponent handleButtonClick={handleChangeValue} />**
    </div>
  );
}

//ChildComponent-> 고차함수가 인자로 받은 함수를 실행하듯,
//props로 전달받은 함수를 컴포넌트 내에서 실행할 수 있도록 한다.
// 버튼이 클릭될 때 실행되길 원하므로, 해당 부분에 콜백함수를 실행한다.
function ChildComponent(**{ handleButtonClick }**) {
  const handleClick = () => {
    // 이 버튼을 눌러서 부모의 상태를 바꿀 순 없을까?
    **handleButtonClick("값은 이렇게 전달하는거란다?");**
  };

  return <button onClick={handleClick}>값 변경</button>;
}
```

#

#

---

#

#

### Side Effect (부수 효과)

- 함수 내에서 어떤 구현이 함수 외부에 영향을 끼치는 경우, 해당함수는 Side Effect가 있다고 이야기 한다.

```jsx
let foo = "hello";
function bar() {
 foo = "world";
}

bar(); // bar은 Side Effect를 발생시킨다!
```

### Pure Function (순수함수)

- 오직 함수의 입력만이 함수의 결과에 영향을 주는 함수
- 입력으로 전달된 값을 수정하지 않고, 전달 인자가 주어진 경우 항상 똑같은 값이 리턴된다.
- 네트워크 요청과 같은 side effect가 없고, 함수의 입력이 아닌 다른 값이 함수의 결과에 영향을 미치지 않는다.

```jsx
function upper(str) {
 return str.toUpperCase(); // toUpperCase는 원본을 수정하지 않는다. (Immutable)
}

upper("hello"); //'HELLO'
```

Math.random()은 순수 함수가 아닙니다. 왜일까요?

결과가 제공된 인자에 의존하지 않고, 순수함수는 어떠한 인자에 대한 결과값이 항상 일정한데 비해, math.random()은 일정하지 않은 결과를 내놓기 때문에 순수함수라고 볼 수 있다

어떤 함수가 fetch API를 이용해 AJAX요청을 한다. 이 함수는 순수함수가 아니다 왜일까?

#

### **React의 함수 컴포넌트**

- React의 함수 컴포넌트는 props가 입력으로, JSX Element가 출력으로 나간다.
- 여기에는 어떤 side Effect도 없으며, 순수 함수로 작동한다.
- 그러나 React 애플리케이션을 작성할 때 아래와 같이 side effect가 발생할 수 있다.
  **React에서의 Side Effect**

  - AJAX 요청
  - 타이머 사용 (setTimeout), (타이머 API)
  - **데이터 가져오기** (fetch API, localStorage)
    React는 side effect를 다루기 위한 hook → **Effect Hook**을 제공한다.

  #

## Effect Hook

- React 컴포넌트 내에서 Side effects를 실행할 수 있게 하는 것이 Hook이다.

**`useEffect(함수)`**

첫번째 인자 → 함수

- 해당 함수 내에서 side effect를 실행하면 된다.
- 이 함수는 다음과 같은 조건에서 실행된다.
  - 컴포넌트가 생성될 때
  - 컴포넌트에 새로운 props가 전달될 때
  - 컴포넌트의 상태(state)가 바뀔 때 렌더링된다.

주의 사항

- 컴포넌트의 최상위에서만 Hook을 호출한다.
- React 함수 내에서 Hook을 호출한다.

[공식문서](https://ko.reactjs.org/docs/hooks-rules.html#only-call-hooks-at-the-top-level) 꼭 참고해보자!

#

#

### 조건부 effect 발생 (dependency array)

**`useEffect(함수, 배열)`**

두 번째 인자 → 배열

- 이 배열은 조건을 담고 있으며, 이 조건은boolean 형태의 표현식이 아닌, **어떤 값의 변경이 일어날 때**를 의미한다. → **어떤 값의 목록**이 들어간다. (종속성 배열)

```jsx
export default function App() {
  const [proverbs, setProverbs] = useState([]);
  const [**filter**, setFilter] = useState("");
  const [**count**, setCount] = useState(0);

  useEffect(() => {
    console.log("언제 effect 함수가 불릴까요?");
    const result = getProverbs(filter);
    setProverbs(result);
  }, **[filter, count]**);

  const handleChange = (e) => {
    setFilter(e.target.value);
  };

  const handleCounterClick = () => {
    setCount(count + 1);
  };
}
```

두 번째 인자인 배열에 filter와 count를 넣으면, filter와 count의 값이 변할 때마다 useEffect의 첫번째 인자가 실행된다! 만약 배열이 [filter] 로 되어있다면, filter 값이 바뀌면 첫번째 인자가 실행되지만, count 값이 바뀔 때는 실행되지 않는다.

- 이러한 배열을 종속성 배열이라고 하며!, 배열 내의 종속성 값이 변할 때, 첫번째 인자의 함수가 실행된다.

##

### 단 한번만 실행되는 effect 함수

아래와 같이 종속성 목록에 아무런 종속성이 없을 때와, 두 번째 인자를 아예 안 넘기는 것은 어떻게 다를까?

**1. 빈 배열 넣기 `useEffect(함수, [])`** - 종속성에 아무런 종속성이 없을 때

- 컴포넌트가 처음 생성될 때만 effect 함수가 실행된다.
- 예를 들면, 처음 한번만 외부 API를 통해 리소스를 받아오고, 더 이상 API호출이 필요하지 않을 때에 사용할 수 있다.

**2. 아무것도 넣지 않기 (기본 형태)** `**useEffect(함수)**`

- 컴포넌트가 처음 생성되거나, props가 업데이트되거나, 상태(state)가 업데이트 될 때마다 effect 함수가 실행된다.

#

#

---

#

#

## Data Fetching

1. **컴포넌트 내에서 필터링**

   전체 목록 데이터를 불러오고, 목록을 검색어로 filter 하는 방법

   처음 단 한번, 외부 API로부터 데이터를 불러오고 filter 함수를 이용한다.

   [예시)](https://codesandbox.io/s/filter-by-client-vyzdc?from-embed)

2. **컴포넌트 외부에서 필터링**

   컴포넌트 외부로 API 요청을 할 때, 필터링 한 결과를 받아오는 방법 (서버에서 매번 검색어와 함께 요청하는 경우)

   검색어가 바뀔 때마다, 외부 API를 호출한다.

   [예시)](https://codesandbox.io/s/useeffect-2-oute9?from-embed) (- LocalStorage API 사용 - 서버 요청으로 대체 가능 )

**두 방법의 차이점**

**컴포넌트 내부에서 처리**

HTTP 요청을 줄일 수 있다.

브라우저(클라이언트) 메모리 상에 많은 데이터를 갖게 되므로, 클라이언트 부담이 증가

#

#

---

#

#

## AJAX 요청

fetch API를 이용한 AJAX 요청!

```jsx
useEffect(() => {
 fetch(`http://서버주소/proverbs?q=${filter}`)
  .then((resp) => resp.json())
  .then((result) => {
   setProverbs(result);
  });
}, [filter]);
```

##

### AJAX 요청이 느릴 경우 - Loading indicator

- 외부 API 접속이 느릴 경우를 고려해 로딩 화면 (loading indicator)을 구현하는 것은 필수이다!

#### loading indicator 구현

```jsx
const [isLoading, setIsLoading] = useState(true);

// LoadingIndicator 컴포넌트를 별도로 구현한다. (생략)
return {isLoading ? <LoadingIndicator /> : <div> 로딩 완료 화면 </div> }
```

```jsx
useEffect(() => {
  **setIsLoading(true);** // fetch 요청 전에 로딩 화면이 뜬다.
  fetch(`http://서버주소/proverbs?q=${filter}`)
    .then(resp => resp.json())
    .then(result => {
      setProverbs(result); // 데이터를 가져온 후
      **setIsLoading(false);** // 로딩 화면 종료
    });
}, [filter]);
```

#

#### **클라이언트가 서버에 요청을 덜 보내는 방법**

- [Throttle a series of fetch requests in JavaScript](https://dev.to/edefritz/throttle-a-series-of-fetch-requests-in-javascript-ka9)
- [throttle과 debounce](https://webclub.tistory.com/607)

#### **서버가 클라이언트에게 돌려줄 응답을 캐싱하는 방법**

HTTP 및 서버 구현에 대한 이해가 필요합니다.

- [HTTP caching](https://developer.mozilla.org/ko/docs/Web/HTTP/Caching)
