웹 애플리케이션은 HTTP 메소드를 이용해 서버와 통신하는데, 요청과 응답을 할 때, '제대로 보내고 받을 수 있는' 일종의 규약이 존재한다.

## REST API (Representational State Transfer API)

- 로이 필딩이 웹(http)의 장점을 최대한 활용할 수 있는 아키텍처로 처음 소개
- 웹에서 사용되는 데이터나 자원(Resource)를 HTTP URL로 표현하고, HTTP 프로토콜을 통해 요청과 응답을 정의하는 방식

### 좋은 REST API를 디자인 하는 방법

**REST 성숙도 모델**

- REST API를 잘 적용하기 위한 4단계 모델
- 레오나르도 리차드슨이 만들었다.
- 로이 필딩은 이 모델의 모든 단계를 충족해야 REST API라고 부를 수 있다고 주장했다.
- 실제로 엄밀하게 3단계까지 지키기 어렵기 때문에 2단계까지만 적용해도 좋은 API디자인이라고 볼 수 있다. (이 경우 HTTP API라고도 부른다.)
  - level 0 : HTTP 사용
  - level 1: 개별 리소스와의 통신 준수
  - level 2: HTTP 메소드 원칙 준수
  - level 3: HATEOAS 원칙 준수

### level 0

- 단순히 HTTP 프로토콜을 사용한다.

ex) 김의사라는 주치의의 예약가능한 시간을 확인하고, 어떤 특정 시간에 예약하는 상황을 예로 들어보자

[표1](https://www.notion.so/aaa15d0c12574d9fafcf1695e24c574f)

### level 1

- **개별 리소스와의 통신을 준수**한다.
- REST API는 웹에서 사용되는 모든 데이터나 리소스를 HTTP URL로 표현한다.
- 모든 자원은 개별 리소스에 맞는 **엔드 포인트**를 사용해야 하고, 요청하고 받은 자원에 대한 정보를 응답으로 전달해야 한다.
  - 김의사라는 의사의 예약 가능한 시간을 응답으로 받는다. → 요청 시 /doctor/김의사 라는 엔드포인트를 사용했다.
  - 특정 시간대에 예약하게 되면, 실제 slot이라는 리소스의 123이라는 id를 가진 리소스가 변경된다. → `/slots/123`으로 실제 변경되는 리소스를 엔드포인트로 사용하였다.

[표2](https://www.notion.so/34dac936db7a4b1683fb5cc98dc7beb9)

- 어떤 리소스를 변화시키는지, 혹은 어떤 응답이 제공되는지에 따라 **다른 엔드포인트를 사용**하기 때문에, **적절한 엔드포인트를 작성**해야한다.
  - 동사, HTTP 메소드, 행위에 대한 단어 사용은 지양
  - 리소스에 집중해 **명사 형태의 단어**로 작성
- 요청에 따른 응답으로 리소스를 전달할 때도, 사용한 리소스에 대한 정보와 함께 리소스의 사용에 대한 성공/ 실패 여부를 반환해야 한다.

[표3](https://www.notion.so/42534fb7a70d4a98894c2d18e85391fd)

### level 2

- CRUD에 맞게 적절한 HTTP 메소드를 사용한다.
- 예약 가능 시간을 확인하는 것은 예약 가능한 시간을 조회(READ)한다. → GET 메소드를 사용한 요청
  - GET 메소드는 body를 가지지 않기 때문에 **query parameter을 사용**하여 필요한 리소스를 전달한다.
  - GET, HEAD, DELETE, OPTIONS 처럼 리소스를 가져오는 요청은 보통 body를 가지지 않는다.
- 예약을 생성(CREATE)한다. → POST 메소드를 사용한다.
- 2단계에서는 POST 요청에 대한 응답이 어떻게 반환되는지도 중요하다
- 응답은 새롭게 생성된 리소스를 보내준다. → `201 Created`
- 관련 리소스를 클라이언트가 **Location 헤더에 작성된 URI를 통해 확인할 수 있도록 해야**, 완벽하게 REST 성숙도 모델 2단계를 충족했다고 볼 수 있다.

[표4](https://www.notion.so/55f05334805a4bbcaf207340229f588f)

### level 3 (HATEOAS- Hypertex As The Engine Of Application State)

- 하이퍼미디어 컨트롤을 적용한다.
- 요청은 2단계와 동일하지만, 응답에는 리소스의 URI를 포함한 링크 요소를 삽입하여 작성한다는 것이 다르다.

[표5](https://www.notion.so/9aece7d4369f400482a93b05d5defe01)

- 예약 가능 시간을 확인 후 그 시간대에 예약을 할 수 있는 링크를 삽입하거나, 특정 시간에 예약을 완료하고 나서는 그 예약을 다시 확인할 수 있도록 링크를 작성해 넣을 수도 있다.
- 좀 더 쉽고, 효율적으로 리소스와 기능에 접근할 수 있게 하는 트리거가 될 수 있다.

### 그 외 참고 사이트

- [5가지의 기본적인 REST API 디자인 가이드](https://blog.restcase.com/5-basic-rest-api-design-guidelines/)
- [호주 정부 API 작성 가이드](https://api.gov.au/standards/national_api_standards/)
- [구글 API 작성 가이드](https://cloud.google.com/apis/design?hl=ko)
- [MS의 REST API 가이드라인](https://github.com/Microsoft/api-guidelines/blob/master/Guidelines.md)

### HTTP methods - 멱등성

[멱등성 표](https://www.notion.so/a1c6a5a5ee844512b4f224c59d898777)

매 요청마다 같은 리소스를 반환하는 특징 → 멱등(idempotent)하다고 한다.

멱등성을 가지는 메소드 PUT과 그렇지 않은 POST는 구분하여 사용해야 한다. PUT은 교체, PATCH는 수정의 용도로 사용한다.

### Query parameters

- **Paging**
  ?page=1&per_pave=30
- **Filtering**
  ?status=open
- **Sorting**
  ?direction = desc
- **Searching**
  ?q=javascript

### Status Codes

- **200** - OK (Everything is working)
- **201** - CREATED (A new resource has been created)
- **204** - NO CONTENT (The resource was successfully deleted, no response body)

- **304** - NOT MODIFIED (The date returned is cached dat (data has not changed))
  → 반복된 요청

[400 → client fault]

- **400 -** BAD REQUEST (The request was invalid or cannot be served)
- **401 -** UNATHORIZED ( The request requires user authentication)
- **403 -** FORBIDDEN (The server understood the request but is refusing it or the access is not allowed)
- **404 -** NOT FOUND (There is no resource behind the URI)

[500 → server fault]

- **500 -** INTERNAL SERVER ERROR \*\*\*\*

### **HTTP API 테스트 도구**

**HTTP API 테스트 도구 (CLI)**

- `curl` (대부분의 리눅스 환경에 내장되어 있습니다.)
- [wuzz](https://github.com/asciimoo/wuzz)

**HTTP API 테스트 도구 (GUI)**

- [Postman](https://www.postman.com/)
- [Insomnia](https://insomnia.rest/)
