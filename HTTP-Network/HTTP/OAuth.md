## OAuth

인증을 위한 프로토콜의 한 종류 (인증 중개 매커니즘)

보안 된 리소스에 액세스하기 위해 클라이언트에게 권한을 제공(Authorization)하는 프로세스를 **단순화**하는 프로토콜 중 한 방법

ex) 소셜 로그인 인증 방식(Google, Github, facebook 등)

- 자주 사용하고 중요한 서비스들의 ID와 Password만 기억해놓고 해당 서비스들을 통해서 소셜 로그인을 할 수 있다.
- 보안상의 이점도 있다. 검증되지 않은 app에서 OAuth를 사용해 로그인을 한다면, 직접 유저의 민감한 정보가 App에 노출될 일이 없고, 인증 권한에 대한 허가를 미리 유저에게 구해야하기 때문이다.

인증만 다른 서비스에서 받는 것이다!

접근 권한 관리는 내 서버의 몫이다.

- 인증: 사용자가 유효한지 판단(credential, 자격 증명)
- 권한 부여: 어떤 권한을 획득할 수 있는지 결정

#

## OAuth에서 꼭 알아둘 용어!

**Resource Owner**

- 액세스 중인 리소스의 유저. 만약 박신영의 구글 계정을 이용하여 app에 로그인 할 경우, resource owner은 박신영이다.

**Client**

- Resource owner을 대신해 보호된 리소스에 액세스하는 응용 프로그램. 클라이언트는 서버, 데스크탑, 모바일 또는 기타 장치에서 호스팅할 수 있다.
- 박신영의 구글 계정을 이용해 app에 로그인할 때 ⇒ app이 client!

**Resource server**

- client의 요청을 수락하고 응답할 수 있는 서버

**Authorization server**

- Resource server가 액세스 토큰을 발급받는 서버. 즉 클라이언트 및 리소스 소유자를 성공적으로 인증한 후 **액세스 토큰을 발급하는 서버**

**Authorization grant**

- 클라이언트가 액세스 토큰을 얻을 때 사용하는 자격 증명의 유형
- **grant type**
  #
  ### Grant type
  Client가 액세스 토큰을 얻는 방법
  **Grant type 의 종류**
  - **Authorization Code Grant Type**
  - **Refresh Token Grant Type**
  - Implicity Grant Type
  - Client Credentials Grant Type
  - Resources Owner Credentials Grant Type
  1. **Authorization Code Grant type**
  - 액세스 토큰을 받아오기 위해 **먼저 authorization code를 받아 액세스 토큰과 교환**하는 방법
  - 이 절차를 거치는 이유는 **보안성 강화**에 목적이 있다.
  - client 에서 client-secret을 공유하고 액세스 토큰을 가지고 오는 것은 탈취될 위험이 있기 때문에 client에서는 **authorization 코드**만 받아오고 **server에서 access token 요청**을 진행한다.
  1. **Refresh Token Grant Type**
  - 일정 기간 **유효 시간이 지나서 만료된 access 토큰을 편리하게 다시 받아오기 위해 사용**하는 방법
  - access token보다 refresh token의 유효 시간이 대체로 조금 더 길게 설정하기 때문에 가능한 방법
  - server마다 refresh token에 대한 정책이 다 다르므로, refresh token을 사용하기 위해 사용하고자 하는 server의 정책을 살펴볼 필요가 있다.

#

**Authorization code**

- access token을 발급받기 전 필요한 code
- client Id로 이 code를 받아온 후, client secret과 code를 이용해 access token을 받아온다.

**Access token**

- 보호된 리소스에 액세스하는데 사용되는 credentials
- Authorization code와 client secret을 이용해 받아온 이 Access token으로 resource Server에 접근할 수 있다.

**Scope**

- 토큰의 권한을 정의
- 주어진 액세스 토큰을 사용하여 액세스 할 수 있는 리소스의 범위

#

### 쿠키 vs 세션 vs 토큰 vs OAuth

|                 | 설명                                                                                                       | 접속 상태 저장                                | 단점                                               |
| --------------- | ---------------------------------------------------------------------------------------------------------- | --------------------------------------------- | -------------------------------------------------- |
| 쿠키            | http의 stateless 한 것을 보완해주는 도구                                                                   | 클라이언트                                    | 쿠키 그 자체는 인증이 아니다.                      |
| 세션            | 접속 상태를 서버가 갖고 있다. (stateful) 접속 상태와 권한 부여를 위해 세션 아이디(토큰)을 쿠키로 전송한다. | 서버                                          | 하나의 서버에서만 접속 상태를 가지므로 분산에 불리 |
| 토큰 방식 (jwt) | 토큰 자체가 무결성(integrity)를 증명할 수 있다. 서버가 접속 상태를 갖고 있지 않아도 된다. (stateless)      | 클라이언트(쿠키, react의 state, localstorage) | 모든 요청에 토큰을 실어 보내야 한다.               |
| OAuth           | 제 3자로부터 인증을 대행하고, access token을 받는다. 인증만 대신하고 권한 관리는 토큰 방식이랑 유사하다.   | 위와 같다.                                    | 위와 같다.                                         |
