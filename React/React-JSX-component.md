# React

### **Achievement Goals**

- React의 3가지 특징에 대해서 이해하고, 설명할 수 있다.
- JSX가 왜 명시적인지 이해하고, 바르게 작성할 수 있다.
- React 컴포넌트(React Component)의 필요성에 대해서 이해하고, 설명할 수 있다.
- `create-react-app` 으로 간단한 개발용 React 앱을 실행할 수 있다.

### React란?

프론트엔드 개발을 위한 **JavaScript 오픈소스 라이브러리** 

리액트는 다음 세가지 특징으로 인해 프론트엔드 개발을 더 손쉽게 할 수 있다. 

 

### [react 특징](https://reactjs.org/)

1. **선언형(Declarative)** 

리액트는 한 페이지를 보여주기 위해 HTML/CSS/Js로 나누어서 적기 보다, 하나의 파일에 명시적으로 작성할 수 있게 JSX를 활용한 선언형 프로그래밍을 지향한다. 

1. **컴포넌트 기반 (Component-Based)** 

리액트는 **하나의 기능을 구현하기 위해 여러 종류의 코드를 묶어둔 컴포넌트 기반**으로 개발합니다. (이를 느슨한 결합이라고 한다) 

- **컴포넌트를 분리하면 서로 독립적이어서  재사용 가능하므로, 기능 자체에 집중하여 개발**할 수 있다.
- 유지 보수가 편하고 컴포넌트 간 의존성이 줄어들고 독립적이어서 유닛 테스트를 하기에도 편하다.

1. **범용성(Learn Once, Write Anywhere)** 

리액트는 자바스크립트 라이브러리라고 불린다. 

- 기존에 Js 프레임워크로 개발된 프로젝트에 어디에든 유연하게 적용될 수 있다.
- Facebook에서 관리되어서 안정적이고, 2020년 가장 유명한 프론트엔드 기술로 사용된다.
- 리액트의 형제 격인 리액트 네이티브를 사용하면 모바일개발도 할 수 있다.

## JSX(JavaScript XML)

[XML](https://www.notion.so/XML-a2ce6094593c4db99ae7b68aa4fc87f9)

 문자열도 아니고, HTML도 아니다. **React에서 UI를 구성할 때 사용하는 문법**으로 **JavaScript를 확장한 문법**이다. 이를 통해 우리는 React 엘리먼트를 만들 수 있다. 

**[Babel]**

그러나 **브라우저가 바로 실행할 수 있는 JavaScript 코드가 아니다**. 따라서 브라우저가 이해할 수 있는 JavaScript 코드로 변환을 해주어야 한다. 이 때 **'Babel'**을 이용한다. **Babel은 JSX를 브라우저가 이해할 수 있는 JavaScript로 컴파일**한다. 컴파일 후, JavaScript를 브라우저가 읽고 화면에 렌더링할 수 있다. 

[DOM vs React DOM]

DOM → HTML, CSS, JavaScript

React DOM → CSS, JSX 

React에서는 CSS, JSX 파일만을 가지고 웹 어플리케이션을 개발할 수 있다. 컴포넌트 하나를 구현하기 위해 필요한 파일이 줄었고, 한눈에 컴포넌트를 확인할 수 있다. JSX를 사용하면 **JavaScript만으로 마크업(markup)형태의 코드를 작성하여 DOM에 배치할 수 있게 되는 것**이다. JSX은 HTML처럼 생겼지만, HTML이 아니기 때문에 앞서 언급했던 “Babel”을 이용한 컴파일 과정이 필요하다.

DOM → JavaScript와 함께 사용하려면 HTML과 JavaScript를 연결해서 사용해야한다. 

React → JSX를 이용해 DOM보다 명시적으로 코드를 작성할 수 있다. JavaScript 문법과 HTML 문법을 동시에 이용해 기능과 구조를 한눈에 확인할 수 있다. 

이렇게 구조와 동작에 관한 코드를 한 뭉치로 적은 코드셋을 **컴포넌트**라고 한다. 

JSX 없이도 React 요소를 만들 수 있다. 다만 코드가 복잡하고, 가독성이 떨어진다. JSX를 사용해 코드를 더 쉽게 이해할 수 있다. 

### JSX 활용

**1) 하나의 엘리먼트 안에 모든 엘리먼트가 포함되어야 한다.** 

여러 엘리먼트를 작성하려면  opening tag와 closing tag로 감싸주어야 한다. 

```jsx
<div> 
  <h1>Hello</h1>
</div>
<div>
  <h2>World</h2>
</div>
// --> X
```

```jsx
<div>{//opening tag}
  <div>
    <h1>Hello</h1>
  </div>
  <div>
    <h2>World</h2>
  </div>
</div> //closing tag
// ---> O
```

**2) 엘리먼트 클래스 사용 시 `className`으로 표기한다 .**

```jsx
<div class = 'greeting'>Hello!</div>  // --> X
<dic className='greeting'>Hello!</div>  // ---> O
```

React에서 CSS class 속성을 지정하려면 'className'으로 표기해주어야 한다. class로 작성하면 React에서 이를 html 클래스 속성 대신 자바스크립트 클래스로 받아들인다. 

**3) JavaScript 표현식 사용 시 중괄호 {} 이용**

```jsx
const name = 'Josh' 

return (
  <div>
    Hello, **{name}**
  </div>
  ); 
```

JSX에서 JavaScript를 쓰고자 한다면, 꼭 중괄호를 이용해야 한다. 중괄호를 이용하지 않으면 일반 텍스트로 인식한다. JSX는 JavaScript의 확장된 문법이므로, JS의 기본 문법을 활용할 수 있다. 

**4)** **사용자 컴포넌트는 대문자로 시작( PascalCase)** 

```jsx
function hello() {
  return <div>Hello!</div>; 
} 

function HelloWorld() {
  return <hello />; 
}  // --> X
```

```jsx
function **Hello()** {
  return <div>Hello!</div>; 
} 

function **HelloWorld()** {
  return **<**Hello **/>;** 
} // --> O
```

**5) 조건부 렌더링에는 삼항 연산자를 사용한다.** 

```jsx
<div>
  {
    (1+1 === 2) ? (<p>정답</p>) : (<p>탈락</p>)
  }
</div> 
```

조건부 삼항 연산자: `조건문 ? (true일 때 실행) : (false일 때 실행)` 

보통 if 명령문의 단축 형태로 쓰인다. 

**6) 여러 개의 HTML 엘리먼트를 표시할 때는 'map'함수를 사용한다.** 

```jsx
const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'}, 
  {id: 2, title: 'installation', content: 'You can install react from npm'}, 
];

function Blog() {
  const content = posts**.map**((post) => 
    <div **key={post.id}**>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
); 

return (
  <div>    
    {content}
  </div>
); 

```

<aside>
💡 map 함수를 사용할 때는 반드시  **'key'" JSX 속성**을 넣어야 한다.

</aside>

'key' JSX 속성을 넣지 않으면 리스트의 각 항목에 key를 넣어야 한다는 경고가 뜬다. 

**[하나의 엘리먼트를 작성할 때]**

단일 요소이므로 ()를 생략할 수 있고, 별도의 감싸는 태그가 존재하지 않아도 된다. 

```jsx
function App {
  return <h1>Hello, world</h1>
}
```

map을 이용한 반복 

```jsx
const posts = [
  { id: 1, title: "Hello World", content: "Welcome to learning React!" },
  { id: 2, title: "Installation", content: "You can install React via npm." },
  { id: 3, title: "Practice", content: "Practice React via npm run start" }
];

export default function Blog() {
  const blogs = posts.map((post) => {
    <div key={post.id}>
      <div>
        <h3>{post.title}</h3>
        <p>{post.content}</p>
      </div>
      <div>
        {/* TODO : 배열 메소드 map을 이용하여 포스트를 화면에 보여주세요. */}
      </div>
    </div>
  });
  return <div className="post-wrapper">{blogs}</div>;
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/68da1112-3c35-450c-84b6-98fde93c3103/Untitled.png)

---

### 컴포넌트

- 하나의 기능 구현을 위한 여러 종류의 코드 묶음
- UI를 구성하는 필수 요소

리액트 어플리케이션은 **컴포넌트들의 관계를 트리 구조로 형상화하여 표현**할 수 있다. (계층적 구조) 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/02b157d0-5b32-47ef-bec2-08900928066f/Untitled.png)

컴포넌트들은 애플리케이션 내부적으로 근원(root)이 되는 역할을 한다. 최상위 컴포넌트는 근원의 역할을 하며, 다른 자식 컴포넌트를 가질 수 있다. (흔히 React에서 볼 수 있는 <App>이 대표적이다. ) 

각각의 컴포넌트는 독립적인 기능을 가지고 있으면서, UI의 한 부분을 담당하고 있다. 이러한 컴포넌트를 한데 모아 복잡한 UI를 구성할 수도 있고, 더 나아가 최종적으로 복잡한 어플리케이션도 만들 수 있다. 

### 컴포넌트 기반 개발의 장점

- 변경하려는 요소가 있으면, UI에 맞추어 컴포넌트의 위치만 수정해주면 된다.
    
    html, css, js, DOM을 이용하면  html에서 구조의 위치를 바꾼 뒤 css를 통해 디자인을 수정해주어야 하고, 변경된 구조와 스타일에 맞추어 js가 dom을 조작하게끔 수정해주어야 한다. 
    
- 컴포넌트는 재사용이 가능하며 효율적인 개발이 가능하다.
- 마크업, 디자인, 로직을 긴밀하게 연결하여 개발할 수 있다.
- 기술적인 특징이 아닌, 실제 사용자가 사용하는 기능을 기준으로 코드를 모아 개발한다.

---

### Create React App

리액트 SPA를 쉽고 빠르게 개발할 수 있도록 만들어진 툴 체인 (Babel, post CSS, React, jest..?, module bundler, Web pack ..등 웹개발에 필요한 프로그램의 작동 원리를 다 이해하기 어려우므로, 이 **복잡한 개발 세팅을 대신 해주는 것이 Create React App 툴 체인**이다. ) 

`npx create-react-app <파일이름>`: 새로운 리액트 프로젝트 시작, 간단한 개발용 React 앱을 실행

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c0b58e9d-c951-4095-8e50-d7b6bb2fd88f/Untitled.png)

위와 같은 파일이 생성된다. node_modules에 필요한 모듈이 담겨져 있다. 

`index.js` → 우리가 App에서 개발한 부분을 브라우저 상에서 보여준다. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9cd534f2-e1df-477f-b192-f3f1f337ef07/Untitled.png)

```jsx
import React from 'react'; 
import ReactDOM from 'react-dom';
import './index.css';
import App from './App';

ReactDOM.render(
    <App />, //우리가 개발한 부분 
  document.getElementById('root') 
  //개발한 <App />을 html dom 에 접근해서 html element에 넣어주겠다. 
); 

/*이러한 기능을 ReactDOM.render라는 부분이 하고 있다. 
첫번째 인자-> App react element
두번째 부분 -> 원하는 html element위치를 넣으면 잘 작동한다.*/
```

랜덤으로 박명수 어록을 보여주는 프로그램을 만들어보자 

```jsx
import './App.css';

//박명수 명언이 담긴 배열을 만든다. 
let parkProverb = ["참을 인 세 번이면 호구", "고생 끝에 골병 난다", "가는 말이 고우면 얕본다", "어려운 길은 길이 아니다", "성공은 1% 재능과 99%의 빽", "지금 공부 안 하면 더울 때 더운 데서 일하고 추울 때 추운 데서 일한다.", "늦었다고 생각할 때가 늦었다", "“내 너 그럴 줄 알았다” 알았으면 제발 미리 말 해줘라", "즐길 수 없으면 피하라"] 

//배열 안의 요소가 랜덤으로 나오는 컴포넌트. (Math.random() 메소드를 사용 
const getRandomIndex = (length) => {
  return parseInt(Math.random() * length);
}

//
function App() {
  return (
    <div className="App">
      <header>
        <p>
          새로고침하면 새로운 박명수 어록을 볼 수 있어요! 
        </p>
        <a href = "https://www.youtube.com/watch?v=LBctll6gh7Y" >유튜브에서 영상으로 보기</a>
        <div>
          {parkProverb[getRandomIndex(parkProverb.length)]}
          </div>
      </header>
    </div>
  );
}

export default App; //App 컴포넌트를 export 해주어야 index.js에서 import 해올 수 있다. 
```

`npm run start`: 실제 React Web App을 개발 모드로 브라우저에서 실행시켜볼 수 있다.  → 프로그램을 실행시켜 보세요!
