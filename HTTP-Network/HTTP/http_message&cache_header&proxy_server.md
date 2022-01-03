## **HTTP 메세지**

### HTTP 헤더와 바디

⇒ 데이터 **메시지 본문(Message body)**를 통해 **표현(Representation) 데이터**를 전달한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/791df60f-d2ff-4f17-b394-ed42a661a195/Untitled.png)

- 메시지 본문 (Message body) = **페이로드(payload)**
- **표현** = 요청이나 응답에서 전달할 실제 데이터
- **표현 헤더** = 표현 데이터를 해석할 수 있는 정보 제공

## HTTP 헤더

**http 헤더는 http 전송에 필요한 모든 부가 정보를 담기 위해 사용**한다.

- 부가 정보 ex) 메시지 바디의 내용, 바디의 크기, 압축, 인증, 요청 클라이언트, 서버 정보, 캐시 관리 정보 ...
- [http 표준 헤더 종류](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)
- 필요시 임의의 헤더를 추가할 수 있다.
  - Helloworld: hihi

### **HTTP 헤더의 형식**

- <field name>: <field-value>
- ex) Content-Type: application/json
- ex) Authorization: access-Token
- ex) Accept: application/json

### ⭐ **표현 헤더의 종류 [(참고)](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)**

다음은 표현 데이터의 형식, 압축 방식, 자연 언어, 길이 등을 설명하는 헤더이다.

- **Content-Type**: 표현 데이터의 형식. 미디어 타입, 문자 인코딩
  응답 내에 있는 Content-Type은 클라이언트에게 **반환된 컨텐츠의 유형**이 실제로 무엇인지 알려준다.
  [MIME type](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types) ⇒
  - Text/html; charset=urf-8
  - application/json
  - Image/png
- **Content-Encoding**
  - 표현 데이터의 압축 방식, 데이터를 압축하기 위해 사용된다.
  - 데이터를 전달하는 곳에서 압축 후 인코딩 헤더를 추가한다.
  - 데이터를 읽는 쪽에서 인코딩 헤더의 정보로 압축을 해제한다.
  - ex) gzip, deflate, identity
- **Content-Language**
  - 표현 데이터의 자연 언어를 표현한다.
  - ex) ko, en, en-US
- **Content-Length**
  - 표현 데이터의 길이 - 바이트 단위이다.
  - **Transfer-Encoding(전송 코딩)**을 사용하면 Content-Length를 사용하면 안된다.
  - Transfer-Encoding: 전송 시 어떤 인코딩 방법을 사용할 것인가? 를 명시하는데 최근은 Transfer-Encoding 보다는 Content-Encoding 을 사용하며, Transfer Encoding 방식을 사용하는 경우는 chunked 방식으로 사용한다.
  - **chunked 방식 인코딩**은 많은 양의 데이터를 분할하여 보내기 때문에 전체 데이터 크기를 알 수 없으므로, 표현 데이터 길이를 명시해야 하는 Content-Length 헤더와 함께 사용할 수 없다.

표현 헤더는 요청, 응답 시 둘 다에서 사용한다.

> ❓ **MIME Type [(참고)](https://developer.mozilla.org/ko/docs/Web/HTTP/Basics_of_HTTP/MIME_types)**
>
> 클라이언트에 전송된 문서의 다양성을 알려주기 위한 매커니즘
>
> 웹에서 파일의 확장자는 별 의미가 없으므로, 각 문서와 함께 올바를 MIME type을 전송하도록 서버가 정확히 설정하는 것이 중요하다. 브라우저들은 리소스를 내려받았을 때 해야 할 기본 동작이 무엇인지 결정하기 위해 MIME Type을 사용한다.

### ⭐ 콘텐츠 협상 (Content negotiation)

클라이언트가 선호하는 표현 요청

- **Accept**
  클라이언트가 선호하는 미디어 타입 전달
- **Accept-Charset**
  클라이언트가 선호하는 문자 인코딩
- **Accept-Encoding**
  클라이언트가 선호하는 압축 인코딩
- **Accept-Language**
  클라이언트가 선호하는 자연 언어
  - 특정 웹사이트에 접속할 때 콘텐츠 협상이 제대로 적용되지 않으면, 서버는 요청으로 받은 우선순위가 없으므로 기본 언어로 설정된 영어로 응답한다.
  - 클라이언트에서 Accept-Language: ko를 작성해 요청하면, 서버는 해당 우선 순위 언어를 지원할 수 있으므로, 한국어로 된 응답을 돌려준다.
  - 만약 여러 언어를 다중 지원하는 웹사이트에서 기본 언어가 독일어로 설정되어 있다. 한국어로 보고 싶은데, 한국어는 지원하지 않고, 영어로라도 응답을 받길 원한다. 이러한 경우, 클라이언트가 최우선으로 선호하는 언어가 지원되지 않으면..? ⇒ 협상 헤더에서 우선 순위를 지원할 수 있다.
    - **Accept-Language: ko-KR, ko;q=0.9, en-US;q=0.8, en;q=0.7**
    - **Quality Values(q)** 값 사용 (0~1), 클수록 높은 우선순위를 가진다. (1은 생략할 수 있다.

협상 헤더는 **요청 시에만 사용**한다.

### 그 외 요청(Request)에서 사용되는 헤더

- **From**
  유저 에이전트의 이메일 정보
  - 일반적으로 잘 사용하지 않고, 검색 엔진에 주로 사용한다.
- **Referer**
  이전 웹 페이지 주소
  - A ⇒ B로 이동하는 경우 B를 요청할 때, Referer: A를 포함해서 요청한다.
  - Referer을 사용해서 유입 경로를 수집할 수 있다.
- **User-Agent**
  유저 에이전트의 애플리케이션 정보
  - 클라이언트의 애플리케이션 정보(웹 브라우저 정보 등등)
  - 통계 정보
  - 어떤 종류의 브라우저에서 장애가 발생하는지 파악할 수 있다.
  - e.g.
    user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36
- **Host**
  요청한 호스트 정보 (도메인)
  - 필수 헤더
  - **하나의 서버가 여러 도메인을 처리해야 할 때 호스트 정보를 명시**하기 위해 사용된다.
  - 하나의 IP 주소에 **여러 도메인이 적용되어 있을 때 호스트 정보를 명시**하기 위해 사용
- **Origin**
  서버로 post 요청을 보낼 때 요청을 시작한 주소를 나타낸다.
  - 여기서 요청을 보낸 주소와 받는 주소가 다르면 CORS 에러가 발생한다.
  - 응답 헤더의 **Access-Control-Allow-Origin**과 관련되어 있다.
- **Authorization**
  인증 토큰(JWT)를 서버로 보낼 때 사용하는 헤더
  - “토큰의 종류 + 실제 토큰 문자” 를 전송
  - Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l

### 그 외 응답(Response)에서 사용하는 헤더

- ⭐ **Server**
  요청을 처리하는 ORIGIN 서버의 소프트웨어 정보
  - Server: Apache/2.2.22(Debian)
  - Server: nginx
- **Date**
  메시지가 발생한 날짜와 시간
- **Location**
  페이지 리디렉션
  - 웹 브라우저는 3xx 응답의 결과에 Location 헤더가 있으면, Location 위치로 리다이렉트(자동 이동)
  - 201(Created) : Location 값은 요청에 의해 생성된 리소스 URI
  - 3xx (Redirection) : Location 값은 요청을 자동으로 리디렉션 하기 위한 대상 리소스를 가리킴
- **Allow**
  허용 가능한 HTTP 메서드
  - 405(Method Not Allowed) 에서 응답에 포함
  - Allow: GET, HEAD, PUT
- **Retry-After**
  유저 에이전트가 다음 요청을 하기까지 기다려야 하는 시간
  - 503(Service Unavailable): 서비스가 언제까지 불능인지 알려줄 수 있다.
  - Retry-After: Fri, 31 Dec 2020 23:59:59 GMT(날짜 표기)
  - Retry-After: 120(초 단위 표기)

## 캐시와 관련된 헤더

**캐시** ⇒ 데이터나 값을 미리 복사해 놓는 임시 장소

캐시가 없다면..?

- 데이터가 변경되지 않아도 계속 네트워크를 통해서 데이터를 다운로드 받아야 한다.
- 인터넷 네트워크는 매우 느리고 비싸다.
- 브라우저 로딩 속도가 느리다.
- 느린 사용자 경험을 제공한다.

### 캐시 적용

캐시의 접근 시간에 비해 원래 데이터를 접근하는 시간이 오래 걸리는 경우나 값을 다시 계산하는 시간을 절약하고 싶은 경우에 사용한다.

`**cache-control` 헤더\*\*

- 캐시가 유효한 시간을 지정
- 캐시의 여부 지정

```jsx
// 브라우저 캐시에 해당 응답 결과를 저장하고, 이는 60초간 유효하다.
// max-age  캐시의 유효시간(초단위)
Cache-Control: max-age=60
```

이제 두 번째 요청에서는 캐시를 우선 조회한다.

이렇게 캐시를 사용하게 되면

- 캐시 가능 시간 동안 네트워크를 사용하지 않아도 된다.
- 비싼 네트워크 사용량을 줄일 수 있다.
- 브라우저 로딩 속도가 매우 빠르다.
- 빠른 사용자 경험을 제공한다.

### 캐시 적용 - 캐시 시간이 초과했을 경우

이 경우에는 다시 서버에 요청을 하고 60초간 유효한 데이터를 응답 받는다. ⇒ 다시 네트워크 다운로드 발생 ⇒ 캐시 유효 시간이 초기화 된다.

### 캐시 검증 헤더와 조건부 요청

만약 캐시의 유효시간이 지났지만, 데이터 자체에 변경이 없어, 해당 데이터를 써도 되는 상황이라면, 이를 검증하고, 다시 사용할 수 없을까?

### 1. `Last-Modified` 와 `If-Modified-Since`

**Last-Modified**: 캐시의 수정 시간. 데이터가 마지막으로 수정된 시간 정보를 헤더에 포함한다.

⇒ 응답 결과를 캐시에 저장할 때, 데이터 최종 수정일도 저장된다.

```jsx
Last-Modified: 2021년 3월 3일 08:00:00 // 데이터가 마지막에 수정된 시간
```

**유효 시간이 초과되었을 때**

`**If-Modified-Since**` 헤더를 이용해 조건부 요청을 한다.

```jsx
If-Modified-Since: 2021년 3월 3일 08:00:00
```

- 만약 If-Modified-Since 헤더의 날짜와, 데이터 최종 수정일이 같다면
  - 바디를 제외한 HTTP 헤더만 전송한다. ⇒ 304 Not Modified
  - 헤더만 전송하고 바디가 빠졌으므로, 용량이 줄어서 원래 데이터를 네트워크로 내보낼 때 보다 속도가 더 빠르다.
  - 브라우저 캐시에서 응답 결과를 재사용. 메타데이터 또한 갱신
  - 브라우저는 캐시에서 조회한 데이터를 렌더링

**단점**

- 1초 미만 단위로 캐시 조정이 불가능하다.
- 날짜 기반의 로직 사용
- 데이터를 수정해서 날짜가 다르지만, 같은 데이터를 수정해서 데이터 결과가 똑같은 경우
- 서버에서 별도의 캐시 로직을 관리하고 싶은 경우
  ex) 스페이스나 주석처럼 크게 영향이 없는 변경에서 캐시를 유지하고 싶은 경우

## 2. `ETag` 와 `If-None-Match`

**ETag(Entity Tag)**

캐시용 데이터에 임의의 고유한 버전의 이름을 달아둔다.

**서버에서 완전히 캐시를 컨트롤**하고 싶은 경우, ETag를 사용할 수 있다.

데이터가 변경되면 이 이름을 바꾸어 변경한다. (Hash를 다시 생성)

단순하게 ETag만 보내서 같으면 유지, 다르면 다시 받는 방식

```jsx
ETag: "845eed07c5887cf";
```

서버 헤더에서 ETag를 작성해 응답하면, **클라이언트 캐시에서 해당 ETag 값을 저장**한다.

그 후 클라이언트는 단순히 이 값을 서버에 제공만 한다.

- ex) 서버는 배타 오픈 기간인 3일 동안 파일이 변경되어도 ETag를 동일하게 유지한다.
- ex) 애플리케이션 배포 주기에 맞추어 ETag를 갱신한다.

**유효시간이 초과되었을 경우**

`In-None-Match` 를 요청 헤더에 작성한다.

```jsx
In-None-Match: "845eed07c5887cf"
```

캐시 유효 시간이 초과하면, ETag 값을 검증하는 If-None-Match 를 요청 헤더에 작성해서 보낸다.

- 서버에서 데이터가 변경되지 않을 경우 ⇒ ETag는 동일 ⇒ If-None-Match는 거짓! ⇒ 304 Not Modified
- 역시 바디를 제외한 헤더만 전송한다.
- 브라우저는 캐시의 응답 결과를 재 사용한다. 헤더 메타데이터 또한 갱신
- 브라우저는 캐시에서 조회한 데이터를 렌더링한다.

보통 Etag 방법과 날짜를 이용하는 두 가지 방법을 모두 다 사용한다. (이중 인증)

Etag는 파일 내부의 변경이 없는데,, 경로만 바뀌어도 ETag가 변경될 수 있다.

### 정리

**검증 헤더** (Validator)

- ETag: “v1.0”
- Last-Modified: Wed, 25 Dec 2020 12:10:20 GMT

**조건부 요청 헤더**

- If-Match, If-None-Match: ETag 값 사용
- If-Modified-Since, If-Unmodified-Since: Last-Modified 값 사용

## 프록시 서버 & 프록시 캐시(Proxy Cache)

### 프록시 서버

클라이언트와 서버 사이에 대리로 통신을 수행하는 것을 가리켜 **프록시(Proxy)**라고 하며, 그 중계 기능을 하는 서버를 **프록시 서버**라고 한다.

클라이언트가 다른 네트워크 서비스에 간접적으로 접속할 수 있게 하는 컴퓨터 시스템이나 응용 프로그램을 의미하는데, 이를 통해 **보안, 캐싱을 통한 성능, 트래픽 분산** 등의 장점을 가진다.

- 보안 → Server 헤더에 원 서버가 아닌 프록시 서버를 넣는다.
- 캐시가 쉽다 → 이 때 캐시 ⇒ `Cache-Control: public`

우리가 유튜브와 같은 해외 사이트에서 영상을 보는데, 원서버가 미국에 있음에도 불구하고, 불편함 없이 영상을 시청할 수 있다. ⇒ **클라이언트와 원 서버 사이에 위치한 프록시 캐시 서버**로 인해 가능

한국에 프록시 캐시 서버를 두고, 프록시 캐시 서버를 통해 자료를 가져오면 빠른 속도로 자료를 가져올 수 있다.

- 프록시 캐시 서버 ⇒ public 캐시
- 클라이언트에서 사용하고 저장하는 캐시 ⇒ private 캐시

### 프록시 캐시와 관련된 헤더

`Cache-Control: public`

- 응답이 public 캐시에 저장되어도 됨

`Cache-Control: private`

- 응답이 해당 사용자만을 위한 것, private 캐시에 저장해야 함(기본 값)

`Cache-Control: s-maxage`

- 프록시 캐시에만 적용되는 max-age

`Age: 60` (HTTP 헤더)

- 오리진 서버에서 응답 후 프록시 캐시 내에 머문 시간(초)

클라이언트가 캐시를 적용하지 않아도 브라우저가 캐시를 적용하는 것이 기본 값이다.

⇒ 특정 페이지에서 캐시되면 안되는 정보가 있는 경우(ex) 통장 잔고) - **캐시를 무효화하는 방법?**

### 캐시를 무효화할 수 있는 헤더

`**Cache-Control: no-cache**`

- 데이터는 캐시해도 되지만, 항상 원(Origin)서버에 검증하고 사용
- 검증 후 304 (Not Modified) ⇒ 브라우저의 캐시 데이터를 사용
- 만약 캐시 서버와 원 서버 간 네트워크 연결이 단절되어 접근이 불가능할 때
  ⇒ 오류가 아닌, 200(OK) 상태가 나오며, 오래된 캐시 데이터라도 보여준다.

`**Cache-Control: no-store**`

- 데이터에 민감한 정보가 있으므로 저장하면 안됨 ⇒ 데이터가 캐시되지 않는다.
  (메모리에서 사용하고 최대한 빨리 삭제)

`**Cache-Control: must-revalidate**`

- 캐시 만료 후 최초 조회 시 원 서버에 검증해야 함
- 원 서버에 접근 실패 시 반드시 오류가 발생해야 함 (504-Gateway Timeout)
- must-revalidate는 캐시 유효 시간이라면 캐시를 사용한다.
  ⇒ 통장 잔고 등 중요한 정보는 오류로 인해 오래된 정보가 나오면, 그걸로도 큰 문제가 생기기 때문

`**Pragma: no-cache**`

- HTTP 1.0 하위 호환

확실한 캐시 무효화 응답을 하고 싶다면 아래와 같이 모든 지시어를 넣어준다.

```jsx
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
```

Pragma 같은 하위 호환까지 포함하여 확실하게 적용한다.

`Expires`

- 캐시 만료일을 정확한 날짜로 지정
  ```jsx
  Expires: Mon, 01 Jan 1990 00:00:00 GMT
  ```
- HTTP 1.0부터 사용
- 지금은 더 유연한 Cache-Control: max-age 권장
- Cache-Control: max-age과 함께 사용하면 Expires 는 무시된다.
