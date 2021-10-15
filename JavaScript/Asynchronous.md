## 비동기 ([asynchronous](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Introducing))

---

##

### 동기식 (**synchronous)**

요청에 대한 결과가 동시에 일어난다.

- 이전에 JavaScript에서 보아왔던 많은 기능은 동기식(synchronous)이다.
- 코드가 위에서 아래로 차례대로 실행된다. **한 번에 하나의 작업만**, 하나의 main thread에서 처리된다. 그리고 하나의 작업이 끝난 후 다른 작업이 실행된다. → Js는 기본적으로 *single [thread](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Concepts#threads)*이다.

**Blocking**

웹 앱이 브라우저에 특정 코드를 실행하느라 브라우저에게 제어권을 돌려주지 않을 때 브라우저가 마치 정지된 것처럼 보이는 현상

### 비동기식 (asynchronous)

작업이 동시에 실행되지만, 요청에 대한 결과가 동시에 일어나지 않는다.

- 동기적 코드를 사용하여 작업을 처리할 때, 서버에서 정보를 받아오면 네트워크 환경, 다운로드 속도 등 영향을 받아 정보를 즉시 확인할 수 없다. 정보의 크기가 크면 에러가 더 발생할 수도 있다.
- 위의 blocking 문제를 해결하기 위해 프로그램이 **한번에 두 가지 이상의 일**을 할 수 있도록 비동기적으로 작업을 실행한다. (unblocking)

[비동기 프로그램 개념](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Asynchronous/Concepts#%EB%B9%84%EB%8F%99%EA%B8%B0%EC%A0%81asynchronous_%EC%9D%B4%EB%9E%80)

##

---

##

### 비동기의 주요 사례

- **DOM Element의 이벤트 핸들러**
  - 마우스, 키보드 입력 (click, keydown 등)
  - 페이지 로딩 (DOMContentLoaded 등)

###

- **타이머**
  - 타이머 API (setTImeout 등)
  - 애니메이션 API (requestAnimationFrame)

###

- **서버에 자원 요청 및 응답**
  - fetch API
  - AJAX (XHR)

##

---

##

### 비동기 호출 방법

#### 1. callback

Callback - async를 다루기 위한 함수

```js
const printString = (string) => {
 //setTimeout(func(), ms) -> ms만큼 시간이 지나면 func()를 실행한다.
 setTimeout(() => {
  console.log(string);
 }, Math.floor(Math.random() * 100) + 1);
};

const printAll = () => {
 printString("A");
 printString("B");
 printString("C");
 printString("D");
};

printAll(); // 이 경우 printString()이 동시에 실행되지만, 결과가 나오는 시간이 다 다르므로, 순서가 뒤죽박죽 나온다.
```

```jsx
const printString = (string, callback) => {
 setTimeout(() => {
  console.log(string);
  callback();
 }, Math.floor(Math.random() * 100) + 1);
};

const printAll = () => {
 printString("A", () => {
  printString("B", () => {
   printString("C", () => {
    printString("D", () => {});
   });
  });
 });
};

printAll(); //A, B, C, D 순서대로 출력된다.
```

**Callback hell**
이렇게 콜백 함수를 사용할 때 콜백 함수가 너무 많아지면 가독성이 떨어지고, 관리하기가 어렵다.

##

#### 2. Promise

- 비동기 실행을 위한 자바스크립트 객체이다. 일반 객체와 다른 Promise객체라고 부른다. [(mdn)](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- callback chain을 어떻게 다룰 것인가?
- state(상태): pending(대기) → fulfilled(이행) or rejected(거부)
  - 대기(_pending)_: 이행하거나 거부되지 않은 초기 상태.
  - 이행(_fulfilled)_: 연산이 성공적으로 완료됨.
  - 거부(_rejected)_: 연산이 실패함.

**1. producer**

새로운 프로미스가 만들어지면 우리가 전달한 콜백함수가 바로 실행된다.

- resolve → 다음 액션으로 넘어간다.
- reject → 에러를 핸들링 할 수 있다.

```jsx
const promise = new Promise((resolve, reject) => {
 console.log("doing something"); //> 프로미스가 만들어지면 함수가 바로 실행된다.
 setTimeout(() => {
  resolve("shinyeong"); // 성공적으로 기능을 수행했으면 resolve를 호출한다.
  // reject(new Error('no network')); // 에러가 발생하면 reject로 에러메시지를 보내준다.
 }, 2000); //'shinyeong'라는 값을 전달하는 promise 생성
});
```

**2. consumer** : then, catch, finally

```jsx
promise**.then**((value) => {
    console.log(value);
// 새로운 프로미스를 리턴하면 그 다음 then에서 새 프로미스 리턴 값을 사용한다 .
    return new Promise ((resolve, reject) => {
        setTimeout(() => resolve('siena'), 2000)
    })
})
// promise값을 콘솔창에서 확인하고 싶으면 then을 사용한다.
**.then**(value => console.log(value))

.then(() => {
    return new Error("hey")
})

//error handling
**.catch**(error => {
    console.log(error);
})
//then을 호출하게 되면 promise가 리턴되고, 리턴된 프로미스에 catch를 등록하는 것이다.
//에러 값이 리턴되면 catch에 전달되어 한꺼번에 에러를 처리한다.

**.finally**(() => {console.log('finally')})
//성공하든 실패하든 상관없이 어떤 기능을 마지막으로 수행하고 싶을 때 finally 사용!
```

#### [promise.all](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)

##

### 3. Async / await

promise를 좀 더 간결하게 쓸 수 있는 방법 - 그러나 절대적으로 promise를 대체할 수 없다.

promise를 chaining하면 좀 더 복잡하게 보일 수 있다.

기존의 promise 위에 좀 더 간편한 API를 제공한다. (이렇게 기존에 존재하는 것 위에 더 간편하게 사용할 수 있는 API를 제공하는 것을 **syntactic sugar**이라고 한다. )

```jsx
function delay(ms) {
    //ms초가 지나면 resolve를 실행하는 promise를 만든다.
    return new Promise(resolve => setTimeout(resolve, ms));
}

// async를 사용하면 바로 promise를 만들 수 있다. 상태 (fulfilled)
**async** function getApple() {
    **await** delay(1000); // delay가 실행된 후 '사과'를 리턴한다.
    return '사과'
}

getApple().then((value) => console.log(value))
```
