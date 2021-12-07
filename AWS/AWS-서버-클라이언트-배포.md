[AWS](https://ap-northeast-2.console.aws.amazon.com/console/home?region=ap-northeast-2)

AWS로 배포를 해보자!

# 서버 배포 (EC2)

1. EC2 콘솔을 통해 EC2 인스턴스를 생성
2. 간단한 서버 애플리케이션을 생성하고 EC2 인스턴스에 코드를 배포
3. 서버를 실행시키고, 브라우저에서 서버에 접속할 수 있어야 한다.

### **EC2 인스턴스 생성**

- AWS 메뉴 - EC2 서비스 - 인스턴스 시작 버튼 누르기
- 용도에 맞는 AMI 선택 (프리티어 사용 가능 태그 확인 → 과금 X)
  - Ubuntu 인스턴스 18버전으로 생성한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7eac87bf-3b91-4b74-b031-5b86a5124703/Untitled.png)

- 인스턴스 유형 선택
  CPU, RAM, 용량에 대한 선택 가능

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/498bd201-7184-474b-b180-76187b519990/Untitled.png)

- 검토 및 시작 버튼 - 시작하기
- 새 키 페어 생성 - key 이름 정한다. - **키 페어** 다운로드 하기 **(.pem 확장자 파일)** - 인스턴스 시작

**SSH 프로토콜을 통한 원격 접속과 키 페어(.pem) 파일**

- SSH 프로토콜(Secure Shell) 서로 다른 PC가 인터넷과 같은 Public Network를 통해 통신을 할 때 보안상 안전하게 통신을 하기 위한 통신 규약
- 주고받는 데이터를 암호화해서 해당 키 페어를 가지지 않은 사람은 통신 데이터를 알아볼 수 없기 때문에 보안상 안전한 통신 방법이다
- 위에서 다운로드한 **키 페어 파일**은 **SSH 통신을 위한 키 페어 중 프라이빗 키가 기록된 파일**이다. (.pem 확장자)
- 이 키 페어 파일은 **EC2 인스턴스에 연결할 때 사용하는 암호**가 담긴 파일이다. 그러므로 관리에 유의!

인스턴스가 많을 경우 → 인스턴스 ID를 통해 인스턴스를 구분할 수 있다. Name 칼럼에서 해당 인스턴스 이름을 알아보기 쉽게 설정해둔다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7d0a43bb-47f1-4787-a484-aec78fdf5832/Untitled.png)

### 인스턴스를 어떻게 사용하지? - EC2 인스턴스 연결

생성한 인스턴스를 제어하기 위해, 인스턴스와 연결한다.

- 인스턴스 탭에서 연결하고자 하는 인스턴스 선택 - 연결 버튼 클릭
- SSH 방법으로 연결을 해보자 ! ⇒ SSH 클라이언트

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ddb9a836-dfef-4dfb-a94d-1a7ac7b555d7/Untitled.png)

로컬 터미널에서 SSH 프로토콜을 이용해 인스턴스와 연결이 가능하다. 그 전에 다운로드 했던 **키 페어 파일 (.pem)의 권한을 수정**해주어야 한다.

```jsx
//(터미널에서 입력)
chmod 400 <키 페어 파일 이름>
chmod 400 pratice_AWS.pem
```

pem 파일이 누구나 접근할 수 있는 권한이 부여되어 있다면, 인스턴스는 연결을 거부한다.

(400 ⇒ 나에게만 읽기 권한만 부여한다.)

권한을 수정하지 않으면, 권한이 너무 open 되어 있다는 경고 메시지와 함께 접속이 거절된다

- 권한 설정 완료 ⇒ SSH 명령어를 통해 인스턴스에 접속한다. (위의 사진에서 예: 다음에 나오는 명령어)

```jsx
ssh -i "pratice_AWS.pem" ubuntu@ec2-52-79-240-44.ap-northeast-2.compute.amazonaws.com
// ssh => 프로토콜을 통해
// -i"~" => ~~~라는 이름의 pem을 가지고 (다운로드한 키 페어 파일의 경로)
// ubuntu => ubuntu라는 사용자 이름으로 (ubuntu 인스턴스를 생성하면 기본적으로 ubuntu라는 사용자 이름을 생성한다.)
// @~.com => ~~ 주소의 가상 PC에 접속하겠습니다!
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c7527c9e-466b-47c8-b4b0-9d13e13d3953/Untitled.png)

⇒ EC2에 처음 접속 시 나오는 경고 ⇒ yes

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c115c455-1e5c-4db7-a3f3-b9fd2c28000d/Untitled.png)

이런 식으로 나오면 SSH 프로토콜을 이용해 원격 접속이 성공적으로 연결 된 것이다.

이제 명령어를 통해 AWS에서 빌린 가상 PC를 사용할 수 있다.

### EC2 인스턴스 상에서 서버 실행

**인스턴스에 개발 환경 구축하기**

EC2 인스턴스에 처음 접속하면 서버를 구동하는 데 필요한 개발 환경을 구축해야 한다.

- 패키지 매니저가 관리하는 패키지 정보를 최신 상태로 업데이트 한다.

```jsx
$ sudo apt update
```

- nvm 설치 - [(링크 참조)](https://github.com/nvm-sh/nvm)
- node.js 설치

```jsx
$ nvm install node
```

- npm 설치

```jsx
$ sudo apt install npm
```

**git을 통해 서버 코드 클론 받기**

```jsx
$ git clone <깃헙 레포지토리 주소>
```

그런데 처음 클론 받을 때, 레포지토리가 존재하지 않는다는 에러가 나타난다.

- **깃헙에서 권한 설정**을 해주어야 한다.
  ```jsx
  // 깃 설치
  $ sudo apt-get install git

  //설치됐는지 확인
  $ git --version

  // github에서 clone 및 push 권한 설정
  $ ssh-keygen -t rsa -C "syngh503@gmail.com"
  ```
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/842bf682-8276-468c-b1b4-3864439ef868/Untitled.jpeg)
  이런식으로 **키 페어가 생성**된다.
  ```jsx
  $ cat {저장된 경로/id_rsa.pub}
  ```
  명령어를 입력하면 ssh-rsa 로 시작하는 시퀀스가 나오는데, 시퀀스를 복사해준다.
  깃허브 페이지 - Settings - SSH and GPG keys - new SSH key)
  타이틀은 자신이 하고 싶은거, key에 아까 복사한 public key를 넣어준다. ⇒ git 연동 끝!

서버 코드를 클론 받으면, 서버를 실행 시켜준다.

```jsx
node app.js
//package json에서 npm start로 지정해놓았으므로, npm start로 실행시켜준다.
```

> Error: listen EACCES: permission denied 0.0.0.0:80 at Function (/home ~~~) ~~ Emitted 'error' event on Server instance at: code: 'EACCES' errno: -13, syscall: 'listen', address: '0.0.0.0' port: 80

서버를 실행시키면 위와 같은 에러가 발생한다. 이는 **1024번 아래의 포트 번호를 이용해 서버를 실행하려면 관리자 권한이 필요**하기 때문이다.⇒ `sudo npm start`

이렇게 서버를 실행하면, 80번 포트에서 서버가 작동 중이라는 메시지를 확인할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0d33bf6c-2175-48ea-9a5f-f5b7e9108064/Untitled.jpeg)

**퍼블릭 IPv4 주소**와 **퍼블릭 IPv4 DNS** 는 형태만 다를 뿐 같은 주소이다.

둘 중 아무 주소나 사용해도 상관없다. IP 주소로 접속하면

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bf4e5f99-415f-4da3-897a-cd8a4e99fc8b/Untitled.png)

이러한 오류 메세지가 뜬다. ⇒ **보안 그룹을 설정**해주어야 한다.

### Security Group (보안 그룹)

AWS에서 임대한 인스턴스의 가상 방화벽

인바운드(Inbound)와 아웃바운드(Outbound)에 대한 규칙을 설정할 수 있다.

- **인바운드(Inbound)**
  - EC2 인스턴스로 들어오는 트래픽에 대한 규칙이다.
  - 기본적으로 SSH 접속을 위한 SSH 규칙만 생성되어 있다.
    - 'SMB 프로토콜, 445포트로 들어가고 싶어요' ⇒ 허용 X ⇒ 요청 거절
    - 'SSH 프로토콜, 22포트로 들어가고 싶어요' ⇒ 허용된 규칙 ⇒ 인스턴스에 접근 가능
- **아웃바운드(Outbound)**
  - 인스턴스에서 나가는 트래픽
  - 따로 설정하지 않을 경우, 기본적으로 EC2에서 나가는 모든 트래픽을 허용한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3de6fd97-52f0-4e2f-9f89-899e76044a2e/Untitled.png)

[(이미지 출처)](https://interconnection.tistory.com/43)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a2cf5275-a9a2-459d-a4f0-c8c4f212bb06/Untitled.png)

인스턴스 탭에서 인스턴스가 속한 보안 그룹을 확인할 수 있다.

보안 그룹 탭에서, 인스턴스 탭에서 확인한 보안그룹을 클릭하면, 해당 보안 그룹의 규칙을 설정할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1265b218-54b0-4efc-9a83-3b677d331105/Untitled.png)

규칙 추가를 통해 새로운 규칙을 추가할 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f1173e07-9b53-4297-974e-462912f12b39/Untitled.png)

서버가 인터넷에서 요청을 받을 수 있도록 인바운드 규칙을 설정해주었다.

**접근하는 트래픽의 프로토콜** ⇒ HTTP 프로토콜,

**소스** ⇒ anywhere IPv4, anywhere IPv6 로 설정해주었다. ⇒ 모든 주소에 대해 요청을 허락한다.

- 소스는 특정 보안그룹일 수도 있고, 특정 IP 주소일수도 있고, 둘 다 아닐 수도 있다.

그 외 포트 등 규칙을 설정할 수 있다.

규칙 저장을 누르면 보안 그룹을 설정하는 과정이 완료된다

⇒ 브라우저나 postman으로 접속 테스트를 진행해보자!

# 클라이언트 배포 (S3)

S3 버킷을 이용해 정적 웹사이트를 호스팅해보자!

1. 정적 웹 페이지 빌드
2. S3 콘솔을 통한 버킷 생성 및 정적 웹 사이트 호스팅 용으로 구성
3. 빌드된 정적 웹 페이지 업로드
4. 퍼블릭 액세스 차단 해제 및 정책 생성

### 정적 웹페이지 빌드 (build)

**빌드(build)?**

작성한 코드의 불필요한 데이터를 없애고, 통합 및 압축하여 배포하기 이상적인 상태를 만드는 과정

- 빌드 과정을 통해 코드의 데이터 용량이 줄어들고, 웹사이트 로딩 속도가 빨라진다.

**빌드하기 전 환경 변수를 담은 .env 파일을 확인한다.**

- .env 파일의 REACT_APP_API_URL에 `http://` 혹은 `[https://`를](https://를) 포함한 퍼블릭 IPv4 DNS를 넣어준다.
- 환경 변수를 제대로 설정해주어야 서버에 제대로 요청을 보낼 수 있다.

**환경 변수 설정이 완료**되면 터미널에 `npm run build` 명령어를 입력해 빌드 과정을 진행한다.

빌드에 성공하면 'Compiled successfully' 메세지가 뜨고, **빌드 파일이 생성**된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7799020e-5eb6-4772-baa7-41544756983a/Untitled.png)

### S3 콘솔에서 버킷 생성 및 정적 웹 사이트 호스팅 용으로 구성

AWS에서 [S3 창](https://s3.console.aws.amazon.com/s3/home?region=us-east-2#)에 접속한다. ⇒ 버킷 만들기

일반 구성

- 버킷 이름 작성 ( **국제적인 각 리전에서 고유**해야 한다.)

⇒ 버킷 만들기 클릭 ⇒ 버킷이 생성!

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8eb17751-4a5b-4222-a905-00758fe10c51/Untitled.png)

버킷 아이디 클릭 후, 속성 메뉴로 이동한다.

- **정적 웹 사이트 호스팅 옵션**
  - 편집 버튼 ⇒ 정적 웹 사이트 호스팅의 **활성화 옵션**을 선택한다.
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d3ede1ee-0b99-4feb-a071-cda641c57caf/Untitled.png)
  - **인덱스 문서를 작성**한다. - 웹 사이트 주소에 처음 접속했을 때 보일 **기본 페이지를 지정** (index.html)
  - **오류 문서**는 공란으로 비워도 되지만, 만약 **오류가 발생**하면 메인 페이지로 반환하기 위해 index.html을 기입한다.

        ⇒ 변경 사항 저장

그 후, 다시 속성 페이지로 리디렉션 된다.

다시 정적 웹사이트 호스팅 옵션 부분으로 이동

⇒ **버킷 웹 사이트 엔드포인트**가 생성되어 있다

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f9487ce5-0e82-4ee1-b89f-d70e23c6af9a/Untitled.png)

해당 엔드포인트로 이동하면 403 Forbidden 에러메시지가 뜬다.

⇒ 버킷에 **정적 웹 페이지 파일을 아직 업로드** 하지 않았고, **퍼블릭 액세스 설정 변경**과 **정책 생성**을 하지 않았기 때문이다.

### 빌드 된 정적 웹페이지 업로드

속성 메뉴 옆의 '객체' 메뉴를 클릭 ⇒ 업로드 버튼 클릭!

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4bf3316f-88c4-4a2b-8dcb-0f220462d7a2/Untitled.png)

여기에 빌드 파일 내용을 업로드 한다. (빌드 파일 자체를 업로드 하는 것이 아닌, 빌드 폴더 내부의 저장된 파일만 업로드!)

그러면 업로드 대기 목록에 파일들이 추가된다. ⇒ 업로드 클릭!

### 퍼블릭 액세스 차단 해제

S3 메인 화면 - 생성한 버킷 클릭 - 속성 메뉴 옆 '권한' 메뉴 클릭

⇒ 퍼블릭 액세스 차단(버킷 설정) 옵션 ⇒ 편집 버튼 클릭!

⇒ **모든 퍼블릭 액세스 차단 옵션을 해제**한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/373fd171-1d96-4230-aa30-d372eb35a78a/Untitled.png)

### 정책 설정

권한 메뉴 - '버킷 정책 옵션' ⇒ '편집' 버튼 클릭!

⇒ **정책 생성기** 버튼 클릭!

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b1ab0e2b-c1c2-4f65-b5ca-a9a1d318bbb3/Untitled.png)

- **Select Type of Policy** - S3 Bucket Policy 선택
- **Principal** - 권한을 적용할 사용자. 모두에게 공개할거라서 \*를 입력
- **Actions** - Get object 옵션 선택. 버킷에 접근하는 모든 사용자가 버킷 내의 저장된 객체 데이터를 읽을 수 있다. (버킷을 웹사이트 용도로 구성할 때 선택한다.)

⇒ Generate Policy 버튼을 누르면 정책이 생성

⇒ 정책은 JSON 형태로 생성

⇒ 생성된 정책을 드래그해서 복사한 뒤 Close 버튼을 누른다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2112b6a9-096c-483d-b56b-bdc8e2db5fdf/Untitled.png)

**버킷 정책 편집 페이지**로 돌아가 생성한 버킷 정책을 붙여넣는다.

⇒ 변경사항 저장

그 후 정적 웹사이트 호스팅 옵션을 찾은 뒤, 버킷 웹 사이트 엔드포인트 주소를 클릭해 접속이 잘 되는지 확인해보자!
