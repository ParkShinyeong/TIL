var vs let vs const , hoisting, TDZ, 즉시실행함수

변수의 실행 단계

[https://poiemaweb.com/es6-block-scope#13-호이스팅](https://poiemaweb.com/es6-block-scope#13-%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85)

## Hoisting

- **var선언문이나 function 선언문 등을 해당 스코프의 선두로 옮긴 것처럼 동작하는 특성**

- 자바스크립트에서는 let, const, var, function, class 등 **모든 선언을 호이스팅한다**.
- 그러나 var과 달리 let으로 선언된 변수를 선언문 이전에 참조하면 참조 에러(Reference Error)가 발생한다.
  → let으로 선언된 변수는 스코프의 시작에서 변수의 선언까지 **일시적 사각지대(Temporal Dead Zone, TDZ)** 에 빠지기 때문이다.

### [변수의 생성 단계와 호이스팅]

1. **선언 단계 (Declaration phase)**

   변수를 [실행 컨텍스트](https://poiemaweb.com/js-execution-context)의 변수 객체(Variable Object)에 등록한다. 이 변수 객체는 스코프가 참조하는 대상이다.

   실행 컨텍스트: 실행 가능한 코드가 실행되기 위해 필요한 환경 (실행 가능한 코드: 전역 코드, Eval 코드, 함수 코드)

2. **초기화 단계 (Initialization phase)**

   변수 객체(Variable Object)에 등록한 변수를 위한 공간을 메모리에 확보한다. 여기서 변수는 undefined로 초기화된다.

3. **할당 단계 (Assignment phase)**

   undefined로 초기화된 변수에 실제 값을 할당한다.

**[var]**

**var로 선언된 변수는 선언 단계와 초기화 단계가 한번에 이루어진다.** 즉 스코프에 변수를 등록하고, 메모리에 변수를 위한 공간을 확보하고, undefined로 초기화한다. 따라서 변수 선언문 이전에 변수에 접근하여도 스코프에 변수가 존재하기 때문에 에러가 발생하지 않지만, undefined를 반환한다. 이후 할당문에 도달하면 비로소 값이 할당된다. 이런 현상을 **변수 호이스팅**이라고 한다.

```jsx
console.log(yap); //undefined

var yap;
console.log(yap); //undefined

yap = 100;
console.log(yap); //100
```

**[let]**

**let으로 선언된 변수는 선언 단계와 초기화 단계가 분리되어 진행된다.** 즉 스코프에 변수를 등록하지만, 초기화는 변수 선언문에 도달했을 때 이루어진다. 초기화 이전에 변수에 접근하려고 하면 참조 에러(Reference Error)가 발생한다. 즉 변수를 위한 메모리가 아직 확보되지 않았기 때문이다. 따라서 스코프의 시작부터 초기화 시작 지점까지는 변수를 참조할 수 없다.

→ 스코프의 시작 지점부터, 초기화 시작 지점 까지의 구간을 **'일시적 사각지대 (Temporal Dead Zone, TDZ)'**이라고 한다.

그러면 ES6에서는 **호이스팅이 발생하지 않는 것일까?** → **그렇지 않다!**

```jsx
let hello = 'hi!'; //전역변수

{
  console.log(hello); //Reference Error: hello is not defined
  let hello = "what's up!"; //지역변수
}
```

이 예제의 경우 전역 변수의 hello가 출력될 것 같지만, 여기서도 hoisting이 발생하기 때문에, Reference Error가 발생하게 된다.

let으로 선언된 변수는 **블록 스코프**를 가지므로 **블록 내에서 선언된 변수는 지역 변수**이다. 지역 변수가 해당 스코프에서 호이스팅이 되고, 블록의 선두부터 초기화가 이루어지는 지점까지 TDZ에 빠진다. 결국 참조 에러가 발생하게 된다.

### 전역 객체와 let

전역 객체: 모든 객체의 유일한 최상위 객체를 의미한다. 브라우저 상에서는 window 객체, 서버 상에서는 global 객체를 의미한다.

- var로 선언된 변수를 전역변수로 사용하면 전역 객체의 프로퍼티가 된다.

```jsx
var yap = 111;
console.log(window.yap); // 111
```

- let 으로 선언된 변수를 전역변수로 사용했을 때, let 전역 변수는 전역 객체의 프로퍼티가 아니다. 즉 전역 객체로 접근할 수 없다. let 전역 변수는 보이지 않는 개념적인 블록 내에 존재한다.

```jsx
let yap = 123;
console.log(window.yap); //undefined
```

### const

- 상수를 위해 사용한다. 하지만 반드시 상수만을 위해 사용하지는 않는다.
- 특징은 let과 대부분 동일하나, 재할당이 금지된다.
- 선언과 동시에 할당이 이루어져야 한다. 그렇지 않으면 문법 에러가 발생한다.

```jsx
//선언만 했을 시
const Hello; // SyntaxError: Missing initializer in const declaration

//재할당 했을 시
const FOO = 123;
FOO = 456; // TypeError: Assignment to constant variable.
```

- const 변수 타입이 객체인 경우, 객체에 대한 참조를 변경하지 못한다. 그러나 객체의 프로퍼티는 보호되지 않는다. 즉, 재할당은 할 수 없지만, 할당된 객체의 내용(프로퍼티 추가, 삭제, 프로퍼티 값의 변경)은 변경할 수 있다.

### Var vs Let vs Const

- 변수 선언에는 기본적으로 **const**를 사용
  - 객체를 재할당하는 경우는 흔치 않다. const를 사용하면 의도치 않은 재할당을 방지해주므로 안전하다.
  - 원시 값의 경우, 가급적 상수를 사용한다.
- **let**은 재할당이 필요한 경우에 한정해서 사용한다. 이 때 변수의 스코프는 최대한 좁게 만든다.
- ES6를 사용하면 var은 사용하지 않는다.

## var vs let vs const

**var**

- 블록 스코프 무시
- 함수 스코프 따름
- 재선언을 할 수 있음
- 재할당 가능

  **let**

- 블록 스코프를 따름
- 함수 스코프 따름
- 재선언 불가능
- 재할당 가능

  **const**

- 블록 스코프를 따름
- 함수 스코프 따름
- 재선언을 방지
- 재할당이 불가능
- 바뀌지 않는 값인 상수를 선언할 때 사용
