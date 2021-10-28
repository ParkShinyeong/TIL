## CDD (Component-Driven-Development)

- **레고처럼 조립해나갈 수 있는 부품 단위로 UI 컴포넌트를 만들어 나가는 개발.**
  - 컴포넌트 생성 - 컴포넌트 결합 - 페이지 조립
- **재사용 할 수 있는 UI 컴포넌트의 필요성**
  - 재사용 할 수 있는 UI 컴포넌트를 미리 디자인하고 개발하면 여러 프로젝트, 혹은 여러 팀 간의 같은 UI 컴포넌트를 공유할 수 있다.
- 예시 사이트
  - [BBC](https://5d28eb5ee163f6002046d6fb-sbbmllgpjg.chromatic.com/?path=/story/components-brand--without-brand-link)
  - [UN](https://uikit.wfp.org/docs/index.html?path=/docs/getting-started-intro--page)

##

### StoryBook

**Component Explorer ( 컴포넌트 탐색기)**

- CDD를 지원하는 도구
- 많은 UI 개발도구가 있으며, 이 중 하나가 **[StoryBook](https://storybook.js.org/)!**

[장점]

- 각각의 컴포넌트를 따로 볼 수 있게 구성해주어 **한번에 하나의 컴포넌트에서 작업**할 수 있다.
  복잡한 개발 스택을 시작하거나, 특정 데이터를 데이터 베이스로 강제 이동하거나, 애플리케이션을 탐색할 필요 없이 전체 UI를 한눈에 보고 개발할 수 있다.
- **재사용성을 확대**하기 위해 **컴포넌트를 문서화**
- **자동으로 컴포넌트를 시각화하여 시뮬레이션** 할 수 있는 다양한 테스트 상태를 확인할 수 있음
  - 버그를 사전에 방지할 수 있다.
- **테스트 및 개발 속도를 향상**시킨다.
- 애플리케이션도 **의존성**을 걱정하지 않고 빌드할 수 있다.

[기능]

- UI 컴포넌트들을 카탈로그화 한다.
- 컴포넌트의 변화를 story로 저장한다.
- 핫 모듈 재 로딩과 같은 개발 툴 경험 제공하기
- 리액트를 포함한 다양한 뷰 레이어 지원하기

[[설치 및 세팅 방법](https://storybook.js.org/tutorials/intro-to-storybook/react/en/get-started/)]

터미널에 다음과 같이 입력한다.

```jsx
# Clone the template
npx degit chromaui/intro-storybook-react-template taskbox

cd taskbox

# Install dependencies
yarn
```

- 스토리북이 실행되기 위해 필요한 코드 형식 ([예시](https://codesandbox.io/s/urclass-5bvx0?from-embed))

```jsx
import React from "react";
import { Button } from "@storybook/react/demo";

// UI 목업의 이름은 Button, Button 컴포넌트를 나타낸다.
export default {
 title: "Button",
 component: Button,
};

//Button 컴포넌트를 custom한 컴포넌트 -> primary, secondary
// => 이들을 export하면 storybook에서 UI 목업을 확인할 수 있다.
export const Primary = () => <Button>Hello Button</Button>;

export const Secondary = () => <Button>Bye Button</Button>;
```

##

---

##

## CSS in JS 방법론

아래 페이지에 CSS의 구조화와 CSS 방법론의 특징에 대해 서술해놓았다.

[CSS 구조화](https://www.notion.so/CSS-ecf5af1fdbc444c191801ee4f258890a)

[CSS 방법론 특징 장단점 ](https://www.notion.so/410afc7b6ea440b2af2af993b6681113)

##

---

##

## Styled-Component

[styled-Component](https://styled-components.com/)

컴포넌트 기반 CSS 작성에 적합한 CSS-in-JS

- **React 컴포넌트 기반 개발 환경**에서 **스타일링을 위한 CSS 성능 향상**을 위해 탄생
- **스타일 속성이 추가된 React 컴포넌트**를 만들 수 있다.

```jsx
// 다른 웹페이지로 이동하는 버튼을 만든다.
// Button을 만들고, tag 속성을 정의하고 (여기서 a), 백틱 안에 CSS 문법 작성

const Button = styled.a`
 display: inline-block;
 border-radius: 3px;
 padding: 0.5rem 0;
 margin: 0.5rem 1rem;
 width: 11rem;
`;
```

##

### **Styled Component의 특징**

- **Automatic critical CSS**
  화면에 어떤 컴포넌트가 렌더링 되었는지 추적해서 해당하는 **컴포넌트에 대한 스타일을 자동으로 삽입**한다. 따라서 사용자가 어플리케이션을 사용할 때 최소한의 코드만으로 화면이 띄워지도록 할 수 있다.
- **No class name bugs**
  Styled Component 는 **스스로 유니크한 `className` 을 생성**한다. 이는 `className` 의 중복이나 오타로 인한 버그를 줄여준다.
- **Easier deletion of CSS**
  Styled Component 는 모든 스타일 속성이 특정 컴포넌트와 연결되어 있기 때문에 만약 **컴포넌트를 더 이상 사용하지 않아 삭제할 경우 이에 대한 스타일 속성도 함께 삭제**된다
- **Simple dynamic styling**
  `className`을 일일이 수동으로 관리할 필요 없이 React 의 props 나 전역 속성을 기반으로 컴포넌트에 스타일 속성을 부여하기 때문에 간단하고 직관적이다.
- **Painless maintenance**
  컴포넌트에 스타일을 상속하는 속성을 찾아 다른 CSS 파일들을 검색하지 않아도 되기 때문에 코드의 크기가 커지더라도 유지보수가 어렵지 않다.
- **Automatic vendor prefixing**
  그저 개별 컴포넌트마다 기존의 CSS 를 이용하여 스타일 속성을 정의하면 된다. 이외의 것들은 Styled Component 가 알아서 처리해 줍니다.

##

**Styled-Component 설치**

```jsx
//with npm
npm install --save styled-components

//with yarn
yarn add styled-conponents
```

##

**Styled Component 에서는 package.json에 다음 코드를 추가하도록 권장한다.**

```jsx
// 여러 버전의 Styled Component가 설치되어 발생하는 문제를 줄여준다.
{
  "resolutions": {
    "styled-components": "^5"
  }
}
```

##

### 사용법

`styled.컴포넌트```

```jsx
import styled from "styled-components";

// <h1> 태그를 렌더링 할 title component를 만듭니다.
const Title = styled.h1`
 font-size: 1.5em;
 text-align: center;
 color: palevioletred;
`;

// <section> 태그를 렌더링 할 Wrapper component를 만듭니다.
const Wrapper = styled.section`
 padding: 4em;
 background: papayawhip;
`;

export default function App() {
 // 일반적으로 컴포넌트를 사용하는 것처럼 Title과 Wrapper를 사용하시면 됩니다!
 return (
  <Wrapper>
   <Title>Hello World!</Title>
  </Wrapper>
 );
}
```

##

같은 스타일 속성을 지닌 여러 개의 컴포넌트들 중에서 약간의 변화를 주고 싶은 경우 ⇒ styled(컴포넌트)

```jsx
// 기존의 Button 컴포넌트에 Orange 컴포넌트만을 위한 새로운 속성 추가
const Orange = styled(Button)`
 color: orange;
 border-color: orange;
`;
```

##

함수 안에서 props를 사용할 수도 있다.

```jsx
// Button component
// primary라는 props의 전달 여부에 따라 다른 스타일을 지정할 수 있다.
...
  background: ${(props) => (props.primary ? "palevioletred" : "white")};
  color: ${(props) => (props.primary ? "white" : "palevioletred")};
...

// App component
...
  <Button>Normal</Button>
  <Button primary>Primary</Button> // prop 전달
...
```

##

컴포넌트에 props로 스타일 속성이 전달되면, 해당 컴포넌트는 **props로 전달된 속성을 우선 적용**한다.

전달되는 속성이 없으면, 기본으로 설정된 속성을 적용한다.

```jsx
import styled from "styled-components";

const Input = styled.input`
  color: ${(props) => **props.inputColor** || 'red'};
`;

export default function App() {
 return (
    <div>
      {/* Input 컴포넌트에 지정된 inputColor(red)가 적용됨.  */}
      <Input defaultValue="김코딩" type="text" />

      {/*Input 컴포넌트는 props로 전달된 커스텀 inputColor(blue)가 적용됨. */}
      <Input defaultValue="박해커" type="text" **inputColor="blue**" />
    </div>
  )
}
```

##
u
---

##

## DOM reference를 잘 활용할 수 있는 useRef

React로 모든 개발 요구 사항을 충족할 수 없다.

다음과 같이 DOM 엘리먼트의 주소값을 활용해야 하는 경우 그렇다

- focus
- text selection
- media playback
- 애니메이션 적용
- d3.js, greensock 등 DOM 기반 라이브러리 활용

React는 이런 예외적인 상황에서 **useRef**로 **DOM 노드, 엘리먼트, 그리고 리액트 컴포넌트 주소값**을 참조할 수 있다.

```jsx
const 주소값을-담는-그릇 = useRef(참조자료형)
return (
  <div>
    <input ref={주소값을-담는-그릇} type='text' />
  </div>
);
```

- 주소값은 컴포넌트가 re-render 되더라도 바뀌지 않는다.

[https://codesandbox.io/s/icy-shape-2svpm?file=/src/App.js](https://codesandbox.io/s/icy-shape-2svpm?file=/src/App.js)

[https://codesandbox.io/s/gallant-mcclintock-hi0fh](https://codesandbox.io/s/gallant-mcclintock-hi0fh)

---

self advanced

[UI를 위한 패턴](https://brunch.co.kr/@blckschrl/69)
