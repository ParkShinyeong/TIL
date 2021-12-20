## ORM (Object -Relational Mapping)

### Sequelize

- Promise를 기반으로 한 node.js ORM
- Postgres, MySQL, MariaDB, SQLite, Microsoft SQL Server 을 지원한다.

### sprint

- [bit.ly](https://bitly.com/)와 같이 긴 URL을 짧게 단축시켜주는 앱 만들기!

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/164fe17e-89d5-43af-84f6-5318750471bb/Untitled.png)

이렇게 내 notion url도 짧게 줄여보았다!

url 테이블의 스키마

```jsx
mysql> describe urls;
+-----------+--------------+------+-----+---------+----------------+
| Field     | Type         | Null | Key | Default | Extra          |
+-----------+--------------+------+-----+---------+----------------+
| id        | int          | NO   | PRI | NULL    | auto_increment |
| url       | varchar(255) | YES  |     | NULL    |                |
| title     | varchar(255) | YES  |     | NULL    |                |
| visits    | int          | YES  |     | NULL    |                |
| createdAt | datetime     | NO   |     | NULL    |                |
| updatedAt | datetime     | NO   |     | NULL    |                |
+-----------+--------------+------+-----+---------+----------------+
```

urls라는 테이블을 만들어, 원본 url과 단축 url의 방문 횟수를 기록한다.

- 원본 url을 요청 받는다.
- 요청받은 url을 단축한 url로 바꾼다.
  - 단축하려는 url을 url 필드에 담는다.
  - title ⇒ 웹사이트의 제목을 title 필드에 담는다. ( request 라이브러리를 이용해서 크롤링을 해온다.)
  ⇒ POST (성공 시 - 201)
- url로 접속할 시, 방문 횟수를 기록하기 위해 visit 필드의 횟수를 증가시킨다. ⇒ GET
- visits의 기본 값은 0으로 설정

### Sequelize 모듈 설치

```jsx
$ npm install --save sequelize
$ npm install --save**-dev** sequelize-cli (개발 단계에서만 사용)
```

### Project bootstrapping

sequelize를 사용할 수 있도록 기본 세팅을 진행한다.

```jsx
npx sequelize-cli init
```

그러면 파일 내부에 config, models, migrations, seeders 폴더 및 파일들이 생성된다.

config 파일 내부에서 데이터베이스와 연결 방법을 작성한다.

```jsx
{
  "development": {
    "username": "root",
    "password": null,
    "database": "database_development",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "test": {
    "username": "root",
    "password": null,
    "database": "database_test",
    "host": "127.0.0.1",
    "dialect": "mysql"
  },
  "production": {
    "username": "root",
    "password": null,
    "database": "database_production",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}
```

mysql이 기본으로 설정되어 있으며, 다른 언어를 사용하고 싶으면 dialect을 변경한다.

password에 mysql 비밀번호를 적을 수 있다.

.env파일을 이용해 비밀번호를 환경변수로 지정해서 넣어줄 수 있다.

위의 데이터 베이스는 mysql에서 직접 만들어준다.

```sql
CREATE DATABASE database_development
CREATE DATABASE database_test
CREATE DATABASE database_production
```

### Model (과 Migration) 생성

```jsx
$ npx sequelize-cli model:generate --name url --attributes url:string,title:string,visits:integer
```

CLI창에 위와 같이 명령어를 입력한다. 모델 이름은 `url`, 각 모델 속성은 `필드 네임: 필드 타입`으로 적어주었다.

- mysql 에서 varchar(255) ⇒ string
- mysql 에서 int ⇒ integer

로 작성하면 된다.

이렇게 하면 models 폴더에 `user.js`, migration 폴더에 `xxxxxxxxxxxxxx-create-url.js`라는 migration파일이 만들어진다.

- models : sequelize에서 테이블 표현으로 사용한다.
- migration: 모델이나 테이블에 변경 사항으로 CLI에서 사용한다. 데이터베이스에서 변경 사항에 대한 커밋이나 로그와 같이 처리한다.

..?

model과 migration의 차이 알아보기!

### running migration

현재 데이터베이스에는 아무것도 없는 상태이다. (모델이나, 마이그레이션에만 테이블 필드와 타입을 정의해 놓았다.) 따라서 데이터베이스에 테이블을 생성하기 위해 마이그레이션을 실행시켜준다.

```sql
$ npx sequelize-cli db:migrate
```

마이그레이션을 취소하려면

```sql
npx sequelize-cli db:migrate:undo
```

### url 모델 생성

다음과 같이 url 모델을 만들어주었다.

url 모델을 만들어 준 이유는 요청 받은 url 및 title, visits를 **데이터베이스에 저장**하고 get 요청을 받았을 때 데이터베이스에서 값을 **응답**으로 보내주기 위해서 모델을 만들어주었다!

```sql
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class url extends Model {
    /**
     * Helper method for defining associations.
     * This method is not a part of Sequelize lifecycle.
     * The `models/index` file will call this method automatically.
     */
    static associate(models) {
      // define association here
    }
  };
  url.init({
    **url: DataTypes.STRING,
    title: DataTypes.STRING,
    visits:{
      type: DataTypes.INTEGER,
      defaultValue: 0
    }**
  }, {
    sequelize,
    modelName: 'url',
  });
  return url;
};
```

방문 횟수는 모델 파일 내에서 `defaultValue: 0` 으로 설정해주었다. ([default value 설정](https://sequelize.org/master/manual/model-basics.html#default-values))

```jsx
post: (req, res) => {
 const url = req.body.url;
 //url이 유효하지 않을 때 => 요청이 잘못 옴 400
 if (!isValidUrl(url)) {
  return res.status(400);
 }

 getUrlTitle(url, (err, title) => {
  if (err) {
   // title을 읽어오는데 에러가 발생했다.
   console.log(err);
   return res.status(400);
  }

  urlModel.findOrCreate({
   where: { url: url },
   defaults: {
    title: title,
   },
  });

  return res.status(201).json(urlModel);
 });
};
```

이렇게 코드를 짰을 때 id, url, title, visits, updatedAt, createdAt 이 포함된 객체가 응답으로 나와야 하는데, 아무것도 전달되지 않는다.

```jsx
post: (req, res) => {
 const url = req.body.url;
 //url이 유효하지 않을 때 => 요청이 잘못 옴 400
 if (!isValidUrl(url)) {
  return res.status(400);
 }

 getUrlTitle(url, (err, title) => {
  if (err) {
   // title을 읽어오는데 에러가 발생했다.
   console.log(err);
   return res.status(400);
  }

  urlModel
   .findOrCreate({
    where: { url: url },
    defaults: {
     title: title,
    },
   })

   .then(([result, created]) => {
    if (created) {
     return res.status(201).json(result); // created
    }
    res.status(201).json(result); // find
   })
   .catch((err) => {
    res.sendStatus(500);
   });
 });
};
```

findOrCreate 같은 sequelize의 메소드는 promise 기반으로 만들어졌기 때문에, then을 이용해 값을 출력할 수 있다.
