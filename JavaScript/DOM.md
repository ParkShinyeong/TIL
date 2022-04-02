HTML은 프로그래밍 언어가 아닌 마크업 언어이다. 문서 작성을 위한 언어이므로 프로그래밍에 적합하지 않다. 따라서 조건문이나, 반복문을 사용할 수 없고, 정보를 저장하기에도 적합하지 않다. 그래서 **자바스크립트와 DOM을 활용하여 HTML에 접근하고 조작**한다. 

## DOM (Document Object Model)

- html요소를 **object(javascript)**처럼 조작(Manipulation)할 수 있는 모델이다.

자바스크립트에서 DOM은 **document라는 객체**에 구현되어 있다. 브라우저에서 작동되는 자바스크립트 코드는 어디에서나 document객체를 조회할 수 있다. 

---

[DOM vs JavaScript](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction#dom_and_javascript) 

DOM

- DOM은 JS로 쓰여졌지만, DOM을 사용하려면 document의 element에 접근해야 한다.
- 프로그래밍 언어가 아니다.
- JavaScript 언어의 부분이 아니다. 그러나 웹사이트를 구축하는  **WebAPI**로 볼 수 있다.
- DOM이 없으면 JS 언어가 웹페이지 (HTML documents, SVG document 그리고 그 구성요소 등)의 모델이나 개념을 가지지 못한다. (JS가 웹페이지에 접근할 수 있도록 도와준다고 보아야 할까?)
- 모든 element는 document object의 한 부분이다. DOM을 사용할 때 이들에게 접근하고, 조작할 수 있다.
- 문서의 구조적 표현을 단일의 일관된 API에서 이용할 수 있게 하면서, 어떤 특정한 프로그래밍 언어로부터 독립하도록 설계되었다.

JavaScript

- 다른 문맥에서도 사용할 수 있다. 예를 들면 Node.js는 컴퓨터에서 JavaScript를 이용하는데 다른 종류의 API를 제공하며, DOM API는 Node.js에서 핵심 부분이 아니다.

---

### HTML이 JavaScript에서 어떻게 표현될까?

```jsx
<html>
  <body>
    <div id="nav">
      <div class="logo"></div>
      <div class="menu-wrapper">
        <div class="menu"></div>
        <div class="menu"></div>
        <div class="menu"></div>
        <div class="profile-photo"></div>
      </div>
    </div>
    <div id="news-contents">
      <div class="news-content-wrapper">
        <div class="news-picture"></div>
        <div class="news-title"></div>
        <div class="news-description"></div>
      </div>
    </div>
    <div id="footer"></div>
  </body>
</html>
```

### 자식 엘리먼트 찾기

```jsx
//body태그의 자식 요소는 모두 몇개일까? 
document.body  
console.dir(document.body) //body태그의 요소들을 객체 형태로 볼 수 있다. 출력된 요소에서 children 속성을 찾을 수 있다. 

document.body.chilrdren // body 태그의 자식 태그를 확인할 수 있다. 
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2908d69e-a502-41d9-82a2-db76a3075b71/Untitled.png)

console.dir를 사용하면 객체의 형태로 출력된 DOM을 볼 수 있다. [(console method)](https://www.zerocho.com/category/JavaScript/post/5b2b45cf1350f9001b662ba6) (console.log는 태그 형식으로 보여준다.) 

body의 자식 요소는 3개인 것을 알 수 있다. (#nav, #news-contents, #footer) 

### 부모 엘리먼트 찾기

[parentElement](https://www.w3schools.com/jsref/prop_node_parentnode.asp)/ [parentNode](https://www.w3schools.com/jsref/prop_node_parentnode.asp)

```html
<body>
    <div id="d1">hello world</div>
</body>
```

```jsx
let x = document.querySelector('#d1').parentElement; //<body>...</body>
let x = document.querySelector('#d1').parentElement.nodeName; //'BODY', nodeName은 엘리먼트 이름을 대문자로 출력 
let y = document.querySelector('#d1').parentNode //<body>...</body>
```

`parentNode`는 읽기 전용 프로퍼티이며 (수정 불가능) parent node를 리턴한다. parent node가 없으면 `null`로 출력된다. (`document`, `DocumentFragment`는 parent node가 없다. ) 

### `parentNode` vs `parentElement`

둘의 차이는 `parentElement`가 부모 노드가 element node가 아닐때 `null`을 리턴하는 것이다. 

```jsx
document.body.parentNode;  //<html> ...</html>
document.body.parentElement;  //<html> ...</html>

document.documentElement.parentNodel;  //#document -> document node를 리턴한다. 
document.documentElement.parentElement; //null 
```

### DOM 순회하기

# DOM으로 HTML 조작하기

**[CRUD(create, read, update, delete)]** 

Create: createElement

Read: querySelector, querySelectorAll (getElementByTagName) 

Update: textContent, id, classList, setAttribute

Delete: remove, removeChild, `innerHTML = ""`, `textContent = ""`

Append: appendChild (html에 적용(append)하는 메소드) 

innerHTML 과 textContent의 차이 

### CREATE

```jsx
let element = document.createElement('div')

// <div></div> 
```

### READ

```jsx
const body = document.querySelecter('body') //body태그를 읽는다. 
const container = document.querySelecter('#container') //container id를 가진 태그 중 가장 첫번째 태그만 읽는다. 
const containers = document.querySelecterAll('#container') //container id를 가진 모든 태그를 읽는다. 

//옛날 방식 
document.getElementsByTagName('div') //div 태그를 읽는다. 
document.getElementsById('idName')  //id가 idName인것을 읽는다. 
document.getElementsByClassName('className') //class가 className인것을 읽는다. 
```

부모, 자식과 상관없이 모든 태그를 찾는다. 

생성할 노드의 종류에 따라 다음과 같은 메소드를 사용할 수 있습니다.

1. createElement()

2. createAttribute()

3. createTextNode()

### APPEND

```jsx
document.body.append(element) // create한 element를 body 자식 요소로 넣어준다.  

document.querySelecter('#container').appendChild(element) // container의 자식 요소로 element를 넣어준다.
//혹은 
const container = document.querySelecter('#container')
container.append(element); // create한 것을 container 자식 요소로 넣어준다. (트리구조로 만들어준다.) 
```

[**append 와 appendChild의 [차이점](https://dev.to/ibn_abubakre/append-vs-appendchild-a4m)**]

**append:** 한번에 여러 개의 자식 요소를 추가할 수 있다. Node object와 DOMString을 추가할 수 있다. 값을 리턴하지 않는다. 

**appendChild**: 한번에 하나의 자식 요소 추가, Node object만 추가할 수 있다. 추가된 node object를 리턴한다. 

```jsx
const parent = document.createElement('div'); 

parent.append('Append DomString'); //<div>Appending DomString</div>
parent.appendChild('Append DomeString with appendChild');  // Uncaught TypeError
```

### UPDATE

```jsx
//태그 안에 컨텐츠 넣어주기 
element**.textContent** = 'hello'; 
element.innerHTML = 'hello'; //이 방법은 지양하는 것이 좋다. 
// <div>hello</div>

//태그에 class, 혹은 id 넣어서 스타일 지정해주기 
element.classList.add('className')
//<div class = 'className'>hello</div>

element.id = 'java' //혹은 
element.setAttribute('id', 'java') //setAttribute을 통해 어떤 값이 key이고,value인지 지정해주었다. 
//<div id='java'>hello</div>

```

[innerHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML#security_considerations)

### DELETE

```jsx
element.remove() // element를 삭제 
document.querySelector('#container').removeChild(element)
```

**[remove와 removeChild의 차이점]**

[-관련 stackoverflow 답변](https://stackoverflow.com/questions/36998877/what-is-the-difference-between-remove-and-removechild-method-in-javascript) 

**remove:** 노드를 메모리에서 삭제하고 종료한다. 부모 엘리먼트가 필요없다.  아무것도 리턴하지 않는다. 

**removeChild:** 노드를 삭제하는 것이 아니다. 메모리에 해당 노드는 그대로 존재하지만, 부모 - 자식 간의 관계를 끊어 DOM 트리에서 해제하는 것이다. 부모 엘리먼트가 필요하다. 관계를 끊은 노드의 참조를 반환한다.
