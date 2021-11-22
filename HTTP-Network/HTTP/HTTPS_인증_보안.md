- **Achievement Goals**

    - **암호화**와 **hashing, salting** 등의 개념을 이해할 수 있다.
    - **HTTP와 HTTPS의 차이점**을 이해할 수 있다.
    - 권한 부여(Authorization)와 인증(Authentication)에 대해 이해할 수 있다.
    - **쿠키**의 작동 원리를 이해할 수 있다
    - **세션 및 쿠키 / 토큰 / OAuth**를 통해 인증 구현을 할 수 있다.
    - 클라이언트, 서버, 데이터베이스의 전체 동작을 이해할 수 있다.
    - 회원가입 및 로그인 등의 **유저 인증**에 대해 구현하고 이해한다.
    - **서비스의 보안**과 관련된 방법을 알아보고 원리 및 장점 및 단점을 이해한다.


    #

## HTTPS (**H**yper **T**ext **T**ransfer **P**rotocol **S**ecure Socket layer)

- HTTP 프로토콜 내용을 SSL, TLS라는 알고리즘을 이용해 통신 내용을 암호화하여 데이터를 전송한다.
- HTTPS는 보안성을 위해 아래와 같은 방식을 사용한다.
    - **인증서 (Certificate)**
    - **CA (Certificate Authority) -** 공인 인증서 발급 기관 ****
    - **비대칭 키 암호화**
        - 공개키와 비공개 키가 다르다
        - 암호화된 데이터를 주고받는다.
        
        ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/63c601f5-b868-461e-8f86-d45de9aff7ce/Untitled.png)
        
##
### HTTPS 특징

- 기밀성(Privacy)
    - 아무도 메시지를 가로챌 수 없다.
    - 메시지는 읽을 수 없다.
    - 암호화되어 있다.
- 무결성(integrity)
    - 메세지가 목적지로 가는 도중에 조작되지 않음
    - 원본 그대로 잘 도착
    ##

### 인증서

HTTPS 프로토콜은 **브라우저가 응답과 함께 전달된 인증서 정보를 확인**할 수 있다

인증서에서 해당 인증서를 발급한 **CA**를 확인하고, 인증된 CA가 발급한 인증서가 아니면 **경고창**을 띄워 서버와 연결이 안전하지 않다는 화면을 보여준다. 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0114b958-ad36-4d62-9462-c127087f64af/Untitled.png)

이러한 화면을 통해 브라우저는 사용자가 안전한 서버를 사용할 수 있도록 사용자를 유도한다. 

**인증서는 무엇을 보장할까?**

- 데이터 제공자 신원 보장
- **도메인 종속**
    - 브라우저는 **인증서의 도메인**과 **데이터를 제공한 제공자의 도메인**을 비교할 수 있어, 이 둘의 정보가 다른 '중간자 공격'을 감지하여 보안 위협으로부터 사용자 및 사용자의 데이터를 보호할 수 있다.
- 브라우저에 접속한 서버가 "의도한"서버임을 보장
- 브라우저와 서버가 통신할 때 암호화할 수 있도록 서버의 공개키 제공
- **데이터 제공자의 신원을 보장받는 것이** 중요한 이유
    - 클라이언트는 데이터 제공자가 제공해준 데이터를 사용할 수 밖에 없다. 클라는 서버에 데이터를 요청하고, 받은 데이터를 이용해 화면을 렌더링 하는 작업을 해야한다.
    - 그래서 요청 및 응답을 중간에서 가로채는 **중간자 공격**에 취약하다. (중간자 공격: 클라와 서버 사이에서 공격자가 서로의 요청, 응답의 데이터를 탈취 및 변조하여 다시 전송하는 공격)
    - 데이터가 중간에 다른 도메인을 거쳐 전달되기 때문에 서버가 `해당 데이터는https://example.com 도메인에서 제공되었습니다`라는 추가 데이터를 응답 객체에 실어 보낸다면, 클라이언트는 **데이터를 제공한 도메인과 전달 받은 도메인**을 비교하여 중간자 공격인지 아닌지 확인할 수 있다.
    - 중간자 공격으로 인해 추가 데이터도 변질될 수 있으므로, 데이터를 **암호화**시키는 작업이 필요하다.
    ##

**로컬 환경(localhost)**에서 **인증서 생성**하기
    
    `mkcert`라는 프로그램을 이용해 로컬 환경에서 신뢰할 수 있는 인증서를 만들 수 있다. 
    
    ```jsx
    $ sudo apt install libnss3-tools
    $ wget -O mkcert https://github.com/FiloSottile/mkcert/releases/download/v1.4.3/mkcert-v1.4.3-linux-amd64
    $ chmod +x mkcert
    $ sudo cp mkcert /usr/local/bin/
    ```
    
    **인증서 생성 -** 로컬을 인증된 발급기관으로 추가 
    
    ```jsx
    $ mkcert -install
    ```
    
    로컬 환경에 대한 인증서 만들기 
    
    ```jsx
    $ mkcert -key-file key.pem -cert-file cert.pem **localhost 127.0.0.1 ::1**
    ```
    
    ⇒ 옵션으로 추가한 `localhost`, `127.0.0.1`(IPv4), `::1`(IPv6) 에서 사용할 수 있는 인증서 생성 
    
    key.pem, cert.pem 파일이 생성된다. 
    
    - **cert.pem** ⇒ 데이터 보안에 중점을 둔 **공개키**
    - **key.pem** ⇒ 인증과정에 중점을 둔 **비공개 키**
    
    ---
    
    node.js 환경에서 https 서버를 작성하려면 `https`내장 모듈을 이용하면 된다. 
    
    ```jsx
    const https = require('https'); 
    const fs = require('fs'); 
    
    https
      .createServer(
        {
          key: fs.readFileSync(_dirname + '/key.pem', 'utf-8'),
          cert: fs.readFileSync(_dirname + '/cert.pem', 'utf-8'),
        }, 
        function (req, res) {
          res.write('Congrats! You made https server now:)'); 
          res.end(); 
        }
      }
    .listen(3001);  
    
    ```
    
    이렇게 서버를 실행하면, https://localhost:3001로 접속하면 브라우저 url창에 자물쇠가 잠겨있는 https 프로토콜을 이용하는 것을 볼 수 있다. 
    
    express.js를 이용해도 서버를 작성할 수 있다. https.createServer의 두번째 파라미터에 들어가는 callback 함수를 express 미들웨어로 교체해주면 된다. 
    
    ```jsx
    const https = require('https');
    const fs = require('fs');
    const express = require('express');
    
    const app = express();
    
    https
      .createServer(
        {
          key: fs.readFileSync(__dirname + '/key.pem', 'utf-8'),
          cert: fs.readFileSync(__dirname + '/cert.pem', 'utf-8'),
        },
        **app.use('/', (req, res) => {
          res.send('Congrats! You made https server now :)');
        })**
      )
      .listen(3001);
    ```
    
    프로젝트를 진행할 때, 로컬 환경이 아닐 때 인증서를 받으려면 [Let's encrypt](https://letsencrypt.org/ko/)에서 인증서를 발급받자! 여기가 무료 인증서를 제공해서 굿굿 
    

+) 기존에 작성한 서버를 [ngrok](https://ngrok.com/)을 이용해 https로 터널링 시킬 수 있다. (로컬에서 구성한 개발환경을 급하게 외부에 공개해야 할 경우 사용)  
##

### 암호화 (Encryption)

일련의 정보를 임의의 방식을 사용해 다른 형태로 변환하여 해당 방식에 대한 정보를 소유한 사람을 제외하고 이해할 수 없도록 알고리즘을 이용해 정보를 관리하는 과정 

- HTTPS 프로토콜은 **암호화된 데이터**를 주고 받는다.
- 중간에 인터넷 요청이 탈취되어도 그 내용을 알아볼 수 없다.
- 기존 HTTP 프로토콜은 요청 및 응답을 탈취하면 wireshark같은 프로그램을 이용해 데이터의 내용을 확인할 수 있다.
- HTTPS 프로토콜을 사용하면 비밀번호와 같은 중요한 데이터가 유출될 가능성이 현저히 적어진다.

##
### Hashing

입력받은 **데이터를 고정된 길이의 데이터로 변환**할 때, **이전의 데이터를 알아볼 수 없게** 만드는 것 

- sha1 / sha256 / sha512 / base64
- input ⇒ output이 항상 동일한 **순수 함수**이다.
- 해싱된 값은 **복호화가 불가능**하다.
- 모든 값을 계산하는데 오래 걸리지 않아야 한다.
- 최대한 해시 값을 피해야 하며, 모든 값은 고유한 해시 값을 가진다.
    - 아주 작은 단위의 변경이라도 완전히 다른 해시 값을 가진다.
    
   
    
##
### Salt(솔트)

- 암호화 해야하는 원본 값에 어떤 **별도의 값을 추가**해서 결과를 변형한다.
    
    암호화만 해놓으면 해시된 결과가 늘 동일 ( [레인보우 테이블](http://wiki.hash.kr/index.php/%EB%A0%88%EC%9D%B8%EB%B3%B4%EC%9A%B0_%ED%85%8C%EC%9D%B4%EB%B8%94) ) 해서 원본 값에 임의로 약속된 별도의 문자열을 추가하여 해시를 진행하면 기존 해시값과 다른 해시값이 반환되어 알고리즘이 노출되어도 원본 값을 보호할 수 있다. 
    
- 솔트는 사용자와 비밀번호 별로 **유일한 값**을 가져야한다.
- 절대 재사용하면 안된다.
    - 사용자 계정 생성, 혹은 비밀번호 변경 시 새로운 임의의 salt를 사용해서 해싱한다.
- 데이터베이스 사용자 테이블에 해싱된 패스워드 값과 솔트값을 함께 저장한다.
- DB의 유저 테이블에 같이 저장되어야 한다.