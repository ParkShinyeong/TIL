- Redux 상태 관리 라이브러리를 통해 컴포넌트와 상태를 분리하는 패턴을 익힌다.
- Redux (혹은 Flux Pattern)에서 사용하는 Action, Reducer 그리고 Store의 의미와 특징을 이해한다.
- Redux의 3가지 원칙이 무엇이며, 주요 개념과 어떻게 연결되는지 이해한다.
- Presentation 컴포넌트와 Container 컴포넌트의 개념을 이해할 수 있다.
- Redux hooks (useSelector, useDispatch)를 사용해 store를 업데이트 할 수 있다.

# Redux

Redux는 자바 스크립트 앱에서 예측 가능한 상태 관리를 해주는 컨테이너이다.

> Redux is one of the libraries that helps you implement sophisticated state management in your application. It goes beyond the local state (e.g. React's local state) of a component. It is one of the solutions you would take in a larger application in order to tame the state. A React application is a perfect fit for Redux, yet other libraries and frameworks highly adopted its concepts as well.

React에서는 컴포넌트 내에서 state를 관리해주는데, 형제 컴포넌트 간에 데이터를 주고 받을 때 부모 컴포넌트를 통해서 주고받는다. 그런데 자식 컴포넌트가 많아지면 상태 관리가 매우 복잡해질 것이다.

⇒ 복잡성을 줄이기 위해 상태 관리 라이브러리인 Redux를 사용한다!

### Redux의 세가지 원칙

- Single source of truth (신뢰할 수 있는 하나의 출처 - store와 연결 )
- State is read-only ( action이라는 객체를 통해 state를 변경할 수 있다.)
- Changes are made with pure function ( reducer와 연결된다. )

### Redux의 장점

1. 상태를 예측가능하게 만들어준다.
2. 유지 보수에 용이하다. (복잡한 컴포넌트에서 prop 내려받는 형식으로 작성하며 버그가 나타났을 때, props로 전달해준 모든 컴포넌트를 수정해야 하므로 복잡해진다. )
3. 디버깅에 유리하다. (action과 state lof 기록 시) → 브라우저에서 redux devtool을 설치할 수 있다.

#

---

#

## Redux 기본 개념

##

### Store

상태(state)가 관리되는 하나의 공간. 컴포넌트가 state 정보가 필요할 때 store에 접근해서 정보를 가져간다.

- 여러 개의 store은 없으며, 여러 개의 reducer을 사용한다.

```jsx
// store > store.js 파일에서 createStore 메소드로 rootReducer을 연결해주고 있다.
const store = createStore(rootReducer);
```

실제 소스 코드에서는 미들웨어와 Redux devtools 지원을 위해 두 번째 인자에 추가적인 내용이 들어가기도 한다.

##

### Action

어떤 액션을 취할 것인지 정의해 놓은 자바 스크립트 객체

store에게 애플리케이션의 데이터를 운반해주는 역할을 한다.

- type과 부가적인 payload를 갖는다. (type은 필수적으로 지정! payload는 필수적이지 않다)
- type은 action type을 언급하는 방식으로 작성한다.
- type은 문자열 리터럴이지만, payload는 문자열, 객체 등 아무것이나 될 수 있다.

```jsx
// example - 주문서
{
  type: 'ORDER',
  drink: {
    menu: 'Latte',
    size: 'Grande',
    iced: false
  }
}
```

##

### Dispatch

Action을 전달하는 메소드. dispatch의 전달 인자로 Action 객체가 전달된다.

- Redux store의 상태를 바꾸기 위해 action을 보낼 수 있다.
- dispatch로 action을 전달하면, 그것은 Redux의 모든 reducer로 보내진다.

##

### Reducer

현재 상태(state)와 Action을 이용해 새로운 상태(state)를 만들어내는 pure function이다.

```jsx
//보통 이런 형식으로 구성된다. switch문을 통해 코드를 작성했지만, if문으로 작성해도 무방하다.
const itemReducer = (state = initialState, action) => {
 switch (action.type) {
  case ADD_TO_CART:
   return Object.assign({}, state, {
    cartItems: [...state.cartItems, action.payload],
   });
  default:
   return state;
 }
};
```

Action객체는 Dispatch 메소드에 전달되고, Dispatch는 Reducer을 호출해 새로운 state를 생성

(왜 이렇게 사용하는 걸까? - 데이터는 한 방향으로 흘러야하기 때문이다.)

##

**Reducer의 Immutability(불변성)**

Reducer 함수를 작성할 때 **Redux의 state 업데이트를 immutable한 방식으로 변경해야 한다**는 것이다.

Redux의 장점 중 하나인 **변경된 state를 로그로 남기기 위해**서 꼭 필요한 작업이다. 이를 위해서 Object.assign을 통해 새로운 객체를 만들어 리턴할 수 있다.

Reducer은 `combineReducers` 메소드를 통해 여러가지의 메소드를 하나로 합칠 수 있다.

```jsx
const rootReducer = combineReducers({
 itemReducer,
 notificationReducer,
});
```

##

### useSelector()

컴포넌트와 state를 연결하는 역할 ⇒ 컴포넌트에서 `useSelector`를 통해 store의 state에 접근할 수 있다.

- useSelector의 **전달 인자로 콜백 함수**를 받으며, 콜백 함수의 전달 인자로는 **state 값**이 들어간다.

```jsx
import { useSelector } from "react-redux";

useSelector((state) => state.itemReducer);
```

##

### useDispatch()

Action 객체를 Reducer로 전달해주는 메소드이다. Action이 일어나는 곳은 클릭 등의 이벤트가 일어나는 컴포넌트이다.

```jsx
//addToCart라는 Action이 있을 때
export const addToCart = (itemId) => {
 return {
  type: ADD_TO_CART,
  payload: {
   quantity: 1,
   itemId,
  },
 };
};

// 이벤트가 실행되는 컴포넌트 내부, 이벤트 헨들러 내부
// 위에서 작성한 Action을 매개변수로 전달한다. 이를 통해 Reducer로 전달해줄 수 있다.
import { useDispatch } from "react-redux";
const handleClick = (item) => {
 useDispatch(addToCart(item));
};
```

##

---

[React Redux 공식 홈페이지](https://react-redux.js.org/api/hooks)
