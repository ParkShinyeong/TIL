이전에 REST API에 대해 배우면서 CRUD에 맞게 알맞은 HTTP method를 사용하는 것이 좋다고 배웠다.

PUT과 PATCH 두 가지는 모두 UPDATE와 관련된 method인데, 과연 이 둘의 차이는 무엇일까?
([stackoverflow의 답변](https://stackoverflow.com/questions/28459418/use-of-put-vs-patch-methods-in-rest-api-real-life-scenarios/39338329#39338329)을 참조해서 적어보았다.)

#

### 1. PUT

[Section 9.6 RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.6) 에서 명시된 PUT의 정의

> The PUT method requests that the enclosed entity be stored under the supplied Request-URI. If the Request-URI refers to an already existing resource, the enclosed entity **SHOULD be considered as a modified version of the one residing on the origin server**. If the Request-URI does not point to an existing resource, and that URI is capable of being defined as a new resource by the requesting user agent, the origin server can create the resource with that URI.

PUT 메서드는, “요청 시 이미 존재하는 리소스를 참조하는 경우, 동봉된 entity는 **원본 서버에 있는 리소스의 수정된 버전으로 간주**해야 한다. 요청-URI가 기존 리소스를 가리키지 않고 해당 URI를 요청하는 사용자 에이전트에서 새 리소스로 정의할 수 있는 경우 원본 서버는 해당 URI로 리소스를 생성할 수 있다” 라고 한다. (엔티티 전체를 변경한다는 소리인 것 같다)

### 2. PATCH

[RFC 5789](https://www.rfc-editor.org/rfc/rfc5789): 에서 PATCH의 정의는 다음과 같다

> The PATCH method requests that **a set of changes** described in the request entity be applied to the resource identified by the Request- URI.

Patch 메서드는 **일련의 변경 사항을 요청된 리소스에 적용**하도록 한다고 한다. (즉 일부만 변경한다는 소리인 것 같다.)

만약 사용자 데이터 중 { email: “psy503@naver.com”, id: psy503 } 이라는 데이터가 있다고 보자

만약 이메일만 수정하고 싶으면, { email: “qwe531@naver.com” }이라는 json 문서로 PATCH 메서드를 사용할 것이다.

그런데 이런 변경사항을 PUT 메서드로 해결할 수도 있지 않을까? 대신 PATCH와 같은 특정 요소 대신 전체 도면 요소를 보내야 할 것이다. 이러한 점은 **특정 리소스 URI에서 엔티티를 교체/덮어쓰려는 것**으로 볼 수 있다. 이런 점은 entity를 업데이트/patch 하는 것으로 간주되지 않는다. 왜일까?

#

### PUT은 왜 멱등성을 가질까?

우선 멱등성이란 무엇일까? 위키피디아에서 멱등성의 정의를 확인하면

> idempotent(멱등)은 한 번, 혹은 여러 번 실행될 경우, 동일한 결과를 생성하는 연산을 설명하기 위해 사용된다. 임의의 값 x에 대해 f(f(X)) = f(x) 라는 속성을 갖는 함수이다.

**PUT 메서드는 멱등성을 가진다.** 우리가 리소스를 PUT할 때, 다음 두 가정이 적용된다.

1. collection이 아니라 entity를 참조한다.
2. 제공한 entity가 완성되어있다. (전체 entity)

만약 위의 예시에서 PUT을 사용한다고 하면 다음과 같이 요청을 해야한다.

```jsx
PUT /users/1
{
	email: “qwe531@naver.com”,  // 새로운 이메일 주소
	id: psy503
	}
```

PATCH를 사용한다고 하면, 다음과 같이 요청한다.

```jsx
PATCH /users/1
{
	email: “qwe531@naver.com”   // 새로운 이메일 주소
	}
```

즉 PUT에서는 이 사용자에 대한 모든 매개변수가 포함되어 있지만, PATCH에는 수정 중인 매개변수 (이메일)만 포함되어 있다.

- PUT ⇒ 전체 엔티티를 보내고 있으며, 이 **완전한 엔티티가 해당 URI의 기존 엔티티를 대체한다**고 가정한다.
- PATCH ⇒ 제공된 필드만 업데이트하고 나머지는 그대로 둔다.

따라서 PUT 요청은 동일한 요청을 반복적으로 발행하면, 항상 동일한 결과가 있어야 한다. (전송한 데이터가 이제 엔티티 전체 데이터이다.) 따라서 PUT은 멱등성을 가진다고 본다.

#

### PUT을 잘못 사용하면?

만약 patch 시 사용한 데이터를 put으로 보내면 어떻게 될까?

```jsx
GET /users/1
{
	email: “psy503@naver.com”,
	id: psy503
}

PUT /users/1
{
	email: “qwe531@naver.com”  // 새로운 이메일 주소
}

GET /users/1
{
	email: “qwe531@naver.com” // 새로운 이메일 주소
}
```

PUT을 사용할 때 email만 제공했으므로, 이제 엔티티에는 email 데이터만 존재하게 된다. ⇒ id 데이터가 손실되었다. 이 경우 데이터의 무결성이 손상되므로 주의해야한다.

#

### PATCH가 멱등성일 수 있을까?

```jsx
GET /users/1
{
	email: “psy503@naver.com”,
	id: psy503
}

PATCH /users/1
{
	email: “qwe531@naver.com”   // 새로운 이메일 주소
}

GET /users/1
{
	email: “qwe531@naver.com”,   // 새로운 이메일 주소
	id: psy503
}

PATCH /users/1
{
	email: “qwe531@naver.com”   // 새로운 이메일 주소 (반복)
}

GET /users/1
{
	email: “qwe531@naver.com”,   // 새로운 이메일 주소 (반복)
	id: psy503
}
```

이러한 예시는 변경했지만, **동일한 변경을 반복하면, 동일한 결과가 반환**되었다. 즉, 이메일 주소를 새 값으로 변경했다.

이러한 경우, 동일한 PATCH 요청을 발행하면, 동일한 리소스 상태를 가진다고 볼 수 있다. 그런데 왜 **PATCH는 멱등성을 가지지 않는다고 볼까?**

- PATCH가 멱등성을 가지려면, 리소스를 PATCH로 수정한 후 동일한 리소스에 대한 이후의 모든 PATCH 호출은 리소스를 변경하지 않아야 한다.
- PATCH가 멱등성을 가지지 않는 때는 엔티티를 PATCH로 수정한 후 동일한 리소스에 후속 패치 호출이 리소스를 변경한다.

```jsx
GET /users/1
{
	email: "psy503@naver.com",
	id: "psy503"
}

**PATCH /users/1
{
	email: "qwe531@naver.com"   // 새로운 이메일 주소
}**

GET /users/1
{
	email: "qwe531@naver.com",   // 새로운 이메일 주소
	id: "psy503"
}
```

여기서 먼저 이메일을 수정한 후 PATCH를 이용해 id 값을 수정해보자

```jsx
PATCH /users/1
{
	id: "123qwe"   // 새로운 ID
}

GET /users/1
{
	email: "qwe531@naver.com",   // 새로운 이메일 주소
	id: "123qwe"
}
```

그 후 다시 위의 PATCH와 똑같은 명령어를 이용해 이메일 주소를 똑같이 변경한다.

```jsx
**PATCH /users/1
{
	email: "qwe531@naver.com"   // 새로운 이메일 주소
}**

GET /users/1
{
	email: "qwe531@naver.com",   // 새로운 이메일 주소
	id: "123qwe"
}
```

이 경우 맨 위의 PATCH 요청과 동일한 PATCH를 요청했음에도 불구하고 id 값이 바뀌어 있으므로, 같은 결과를 반환 받았다고 볼 수 없다.

#

**ex 2)**

```jsx
GET / users[{ id: 1, username: "firstuser", email: "firstuser@example.org" }];
```

만약 이러한 엔티티가 있다고 생각해보자,

```jsx
PATCH / users[{ op: "add", username: "newuser", email: "newuser@example.org" }];
```

“op” 가 “add” 이므로 새 사용자 목록이 추가된다.

```jsx
GET /
 users[
  ({ id: 1, username: "firstuser", email: "firstuser@example.org" },
  { id: 2, username: "newuser", email: "newuser@example.org" })
 ];
```

그리고 위와 동일한 명령을 한번 더 실행했다.

```jsx
PATCH / users[{ op: "add", username: "newuser", email: "newuser@example.org" }];
```

```jsx
GET /
 users[
  ({ id: 1, username: "firstuser", email: "firstuser@example.org" },
  { id: 2, username: "newuser", email: "newuser@example.org" },
  { id: 3, username: "newuser", email: "newuser@example.org" })
 ];
```

동일한 엔드포인트에 대해 동일한 패치를 발급했음에도 불구하고, /users 리소스가 다시 변경되었다.

따라서 **동일한 명령 실행 시 동일한 결과가 나오지 않았으므로, PATCH는 멱등성을 가지지 않는다.**

따라서 결론적으로 PATCH가 멱등성을 가지는 방식으로 실행될 수 있지만, 항상 멱등성을 가지는 것은 아니다.
