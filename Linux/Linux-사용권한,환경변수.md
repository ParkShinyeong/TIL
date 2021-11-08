# Linux 사용 권한과 소유자

Linux 폴더와 파일은 사용 권한이 존재한다.

- 읽기 (read), 쓰기 (write), 실행 (execute)

1. 폴더/ 파일 생성

```jsx
mkdir linux //폴더 생성
nano helloworld.js // 파일 생성
```

##

### 사용 권한 확인 - `ls -l`

`ls-l` 명령어로 폴더와 파일, 그리고 사용 권한을 확인할 수 있다. => 터미널에 입력해보기

**d/ rwx / rwx / rwx** : d (폴더) - 소유자(owner) - 그룹( group) - 다른 사용자 (other)

- **user**: 파일의 소유자
- **group**: 여러 user, 그룹에 속한 모든 user은 파일에 대해 동등한 group 액세스 권한을 가진다. (많은 사람들이 파일에 액세스 해야하는 프로젝트에서 활용)
- **other:** 파일에 대한 액세스 권한이 있는 다른 user. 파일을 만들지 않은 다른 모든 user을 의미한다.
  other 권한을 설정 → 해당 권한은 **global 권한 설정**이라고 볼 수 있다.

##

### 권한 변경 `chmod`

`chmod` 명령어로 폴더/ 파일의 읽기, 쓰기, 실행 권한을 변경할 수 있다.

- OS에 로그인 한 사용자 = 폴더/파일의 소유자 ⇒ 바로 변경 가능
- OS에 로그인 한 사용자 != 폴더/파일의 소유자 ⇒ `sudo` 명령어로 관리자 권한 획득 후 변경

**권한 변경 방법**

1. **Symbolic method**

Access class, Operator, Access Type 을 알아야 사용할 수 있다.

[제목 없음](https://www.notion.so/573d305bd7f2459aaa423d75d2240166)

```jsx
chmod g-r filename // remove read permission from group
chmod g+r filename // add read permission from group
chmod a=rw filename // -rw-rw-rw-
chmod a+rwx filename //-r-xrwxrwx
chmod go-wx helloworld.js // -r-xr--r--
chmod a= helloworld.js // ----------
chmod u+rwx helloworld.js // -rwx------
```

1. **Absolute form**

숫자 7 까지 나타내는 3 bits 의 합으로 표기한다.

[제목 없음](https://www.notion.so/9a99bbe7049541138149e7c54a48e435)

```jsx
// u=rwx(4+2+1) , go=r(4+0+0 = 4)
chmod 744 filename // => -rwxr--r--
```

[제목 없음](https://www.notion.so/d5ecec824f8248a4b4a0a3f924bbbbc6)

[+) 파일 권한과 관련된 레퍼런스](https://kb.iu.edu/d/abdb)

#

# 환경 변수

API key와 같이 공개할 수 없는 정보가 코드에 포함될 경우 네트워크를 통해 API key가 공개될 수 있다. 이런 일을 방지하기 위해 API key를 PC에 저장해두고 사용해야 한다.

<aside>
❓ **[API key](https://www.fortinet.com/resources/cyberglossary/api-key)** : 사용 및 결재에 관한 프로젝트와 관련된 요청을 인증하는 고유 식별지

</aside>

Linux 기반 운영체재 PC는 **시스템 자체에 전역 변수**를 설정할 수 있다. ⇒ **환경 변수**

`export`를 이용해 환경 변수를 설정할 수 있다.

##

### 환경 변수 확인하기 / 환경 변수 임시 적용

`export` 를 이용하면 환경 변수를 확인하고, 새로운 환경 변수를 추가할 수 있다.

```jsx
// 환경 변수 확인
**export**

// 환경 변수 추가 (등호 표시 뒤에 공백이 없어야한다.)
**export** myage="23"

// 환경 변수의 값을 확인
**echo $**myage // 23
```

이렇게 **`export`로 적용한 환경 변수**는 **현재 사용 중인 터미널에서만 임시로 사용**이 가능하다.

##

### JavaScript에서 환경 변수 사용하기

`dotenv` - npm 모듈을 사용하면 자바스크립트에서 환경 변수를 사용할 수 있다.

```jsx
mkdir environment_variable
cd environment_variable
npm init
npm i dotenv //dotenv 모듈을 설치한다.
```

Node.js 의 내장 객체 `process.env`를 이용하면 `export`로 확인한 내용과 동일한 내용을 **객체 형태**로 출력한다.

- node.js 환경에서 process.env를 이용해 환경 변수에 접근할 수 있다.
- env 파일에 환경 변수 저장, dotenv 모듈을 설치하여 JS 파일에서 사용할 수 있다.

##

### .env: Node.js 에서 환경변수 영구 적용

환경 변수를 Linux 운영 체제에 저장하려면 Node.js에서 **파일 .env 를 만들어 저장하는 방법**을 사용한다.

```jsx
nano.env;
// env 파일을 생성하고, 사용하고자 하는 환경 변수를 입력한 뒤 저장한다.
// .env 파일 내에서
myname = Parkshinyeong;
```

```jsx
const dotenv = require("dotenv");
dotenv.config(); // 메소드를 이용해 .env를 process.env 에 적용할 수 있다.
console.log(process.env.myname);
```

`node index.js`를 실행하면 myname 변수 값인 Parkshinyeong이 출력된다.

##

### 환경 변수를 이용하면 좋은 점

- API key, DB password와 같이 **민감한 정보를 저장하고 관리**할 수 있다.
- **서로 다른 PC, 여러 .env 파일에서 같은 변수 이름에 다른 값을 할당**할 수 있다.
  실제 서비스 개발 과정에서
  - 개발 환경 (local, development )
  - 테스트 서버 환경 (test)
  - 실제 제품을 제공하는 환경 (production) 이 있다.
  ###
  **개발 환경** - 개인 API 키 사용
  **제품 서비스**에서 개인 API 키 사용 = 일일 요청량을 초과하면 제품이 정상적인 동작을 할 수 없다.
  ⇒ 기업용 API 키 사용
  ###
  개발 환경과 제품을 제공하는 환경에서 사용하는 API 키가 다른 경우, 환경 변수를 이용해 환경을 구분하여 코드를 작성할 수 있다.
  ##
- 마찬가지로 데이터 베이스도 개발, 테스트, 제품 환경으로 구분할 수 있다.

```jsx
// 개발 한경에서 접근할 데이터 베이스
DATABASE_NAME = my_app_dev;

// 테스터 환경에서 접근할 데이터 베이스
DATABASE_NAME = my_app_test;

// 제품 환경에서 접근할 데이터 베이스
DATABASE_NAME = my_app_production;
```

⇒ 하나의 변수 이름을 환경에 따라 다르게 설정할 수 있다
