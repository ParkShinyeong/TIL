### **MVC 패턴**

- MVC 디자인 패턴과 같이, 코드를 각각 다른 부분으로 나누어 작성해야 하는 이유를 이해할 수 있다.
- Model, View, Controller가 각각 어떤 역할을 하는지 이해할 수 있다.

### **Cmarket Database**

- SQL을 Node.js 앱에서 쿼리할 수 있다.
- 클라이언트의 HTTP 요청에 따라 CRUD API를 구현할 수 있다. (CRUD: Create, Read, Update, Delete)

## MVC (Model View Controller)

SW Architechture design pattern 

어플리케이션을 기능별로 (model, view, controller) 구분한다. 

조직적으로 프로그래밍을 진행한다. 

**MVC 사용을 위한 웹 프레임워크**

- Express(JS)
- Backbone (JS)
- Angular (JS)
- Flask (python)

1. **Model** 
- 데이터를 다루는 곳
- 데이터베이스와 직접 상호작용 하는 곳 (SELECT, INSERT, UPDATE, DELETE)
- controller와 상호작용한다.

1. **View** 
- 모델의 시각적인 표현을 담당
- 사용자들이 보는 것 (UI)
- 보통 HTML/CSS 로 구성되어 있다.
- controller와 상호작용한다.
- controller로부터 동적인 데이터를 전달받는다.

1. **Controller** 
- 입력값을 받거나(from view, url) , 요청을 처리한다. (GET, POST, PUT, DELETE)
- 모델로부터 데이터를 받아온다.
- 받아온 데이터를 view에 넘긴다.

[관련 유튜브](https://www.youtube.com/watch?v=pCvZtjoRq1I) / [생활코딩](https://opentutorials.org/course/697/3828) 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5869bab6-7ae2-4f41-b173-70dfaa515a28/Untitled.png)

1. 사용자가 **웹 사이트에 접속**한다. 
2. controller는 **사용자가 요청한 데이터**를 얻기 위해 model을 호출한다. 
3. model은 **데이터베이스 / 파일 등 데이터 소스를 제어**한 뒤 그 결과를 리턴한다.
4.  controller은 그 결과를 받아, **view에 반영**한다. 
5. 데이터가 반영된 view는 **사용자**에게 보여진다.