## Sequelize - Association?

관계형 데이터베이스의 JOIN과 같이 관계성을 갖는 데이터 사이의 처리를 위해 사용한다.

sequelize에서 테이블 간의 연결을 생성하기 위해 다음과 같은 함수를 사용한다.

- **hasOne** - (1: 1 관계) target에 Fk를 추가하고, 소스에 하나의 연계 관계를 형성한다.
- **belongsTo** - (1: 1 관계) Fk를 추가하고, 소스에 하나의 연계 관계를 형성한다.
- **hasMany** - (1: N 관계) target에 Fk를 추가하고, 소스에 여러 연계 관계를 형성한다.
- **belongsToMany** - (N:M 관계), 조인테이블로 N:M (다대다) 관계를 만들어내고, 소스에 여러개의 연계 관계를 추가한다. 조인 테이블은 소스id 와 target Id로 만들어진다.

이 함수들은 첫 번째 인수로 다른 모델을 제공한다.

association를 만들어내면 **속성에 foreign key 제약 조건이 추가**된다.

모든 association은 **업데이트 시 `CASCADE`를 사용**해야한다. 그리고 **삭제 시 `SET NULL`을 사용**한다. (n:M관계에서 제외하고 - 이 때는 삭제 시에도 `CASCADE`를 사용한다. )

[https://sequelize.org/v3/api/associations/](https://sequelize.org/v3/api/associations/)

### [해야 할 것]

- 현재 urls 모델만 존재하는데, users table을 새로 만든다.
- users와 urls는 1:N의 관계이어야 한다.
  - 새 migration 파일을 생성해서 urls에 userId 필드를 만든다. (이 migration 파일은 순수하게 필드 수정만을 담당한다.
  - migration 파일에 FK를 설정한다.
- Association을 정의한다. (urls와 users가 서로서로가 HasMany, BelongsTo로 정의될 수 있다. )

### 1. users 모델 생성

```jsx
$ npx sequelize-cli model:generate --name **users** --attributes **email:string,password:string,phone:integer**
```

users라는 이름을 가진 모델을 생성했다.

테이블의 필드는 email, password, phone을 넣어주었다.

그 다음 migration을 실행해주면, users migration이 생성된다.

```jsx
npx sequelize-cli db:migrate
```

### 2. Association을 정의해준다.

여기서 헷갈렸던 부분이 1:N 관계이므로 HasMany와 BelongsTo를 사용하면 되는 것은 이해했는데, user에 HasMany를 사용하고, urls에 BelongsTo를 사용해야 할지 아니면 그 반대인지가 헷갈렸다.

- users가 여러 개의 urls를 가진다. ⇒ `users.HasMany(urls)`
- urls는 하나의 user을 가진다. ⇒ `urls.BelongsTo(users)`
  여기서 소스 모델은 users, target 모델은 url 이라고 보면 될 것 같다.

**users 모델**

```jsx
const url = require('./').url;
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class users extends Model {
    static associate(models) {
      // 여기서 association을 정의해준다.
      **models.users.hasMany(models.url);**
    }
  };
  users.init({
    email: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    password: {
      type: DataTypes.STRING,
      allowNull: false,
    },
    phone: {
      type: DataTypes.INTEGER,
      allowNull: false,
    }
  }, {
    sequelize,
    modelName: 'users',
  });
};
```

**url 모델**

```jsx
'use strict';
const {
  Model
} = require('sequelize');
const users = require('./users');
module.exports = (sequelize, DataTypes) => {
  class url extends Model {
    static associate(models) {
      // 여기서 association을 정의해준다.
      **models.url.belongsTo(models.users);**
    }
  };
  url.init({
    url: DataTypes.STRING,
    title: DataTypes.STRING,
    visits: {
      type: DataTypes.INTEGER,
      defaultValue: 0
    }
  }, {
    sequelize,
    modelName: 'url',
  });
  return url;
};
```

이렇게 각 모델에 association을 정의해주어도 되고, 아니면 `index.js` 파일에서 한꺼번에 association을 정의해주어도 된다.

```jsx
const { url, users } = sequelize.models;
url.belongsTo(users);
users.hasMany(url);
```

처음에는 hasMany, belongsTo를 사용하면서 Fk 설정도 한꺼번에 해주려고 했다.

```jsx
users.hasMany(url, {
 foreignKey: "userId",
 onDelete: "cascade",
});

url.belongsTo(users, {
 onDelete: "cascade",
 foreignKey: {
  allowNull: true,
 },
});
```

그런데 우리는 새로운 migration에서 Fk를 지정해주려고 하므로, 위와 같이 작성하지는 않았다.

### 3. 새로운 migration 파일 생성 후, Fk를 설정 [(참고)](https://sequelize.org/master/manual/migrations.html#migration-skeleton)

새로운 migration 파일을 생성한다. (이름은 userId)

```jsx
$ npx sequelize migration:create --name userId
```

```jsx
// 20211223063609-userId.js 파일
"use strict";
module.exports = {
 up: async (queryInterface, Sequelize) => {},

 down: async (queryInterface, Sequelize) => {},
};
```

그러면 이렇게 새로운 migration 파일이 생성된다.

- **up**: migration을 통해 수정할 모델을 작성하는 소스 코드 로직
- **down**: migration을 통해 되돌릴 때 수행되는 로직

나는 지금 urls 모델에 userId 라는 컬럼을 추가하고, association 속성 값에 제약 조건을 추가하고 싶다.

따라서

- **up** ⇒ userId 컬럼 추가, association 제약 조건 추가
- **down** ⇒ userId 컬럼 삭제, association 제약 조건 삭제

```jsx
// 20211223063609-userId.js 파일
"use strict";
module.exports = {
 up: async (queryInterface, Sequelize) => {
  // 컬럼 추가
  await queryInterface.addColumn("urls", "userId", Sequelize.INTEGER);

  // foreign key 추가
  await queryInterface.addConstraint("urls", {
   fields: ["userId"],
   type: "foreign key",
   name: "FK_any_name",
   references: {
    table: "users",
    field: "id",
   },
   onDelete: "cascade",
   onUpdate: "cascade",
  });
 },
 down: async (queryInterface, Sequelize) => {
  // foreign key 삭제
  await queryInterface.removeConstraint("urls", "FK_any_name");
  //컬럼 삭제
  await queryInterface.removeColumn("urls", "userId");
 },
};
```

여기서 **queryInterface**는 모든 database와 소통하기 위해 사용된다. [(참고)](https://sequelize.org/master/class/lib/dialects/abstract/query-interface.js~QueryInterface.html)

**addColumn**

테이블에 칼럼을 추가하는 메서드

```jsx
queryInterface.addColumn("테이블_이름", "추가할_칼럼", {
 type: Sequelize.INTEGER,
});
```

**removeColumn**

테이블의 칼럼을 제거하는 메서드

```jsx
queryInterface.removeColumn("테이블_이름", "삭제할 칼럼");
```

**addConstraint [(참고)](https://sequelize.org/master/class/lib/dialects/abstract/query-interface.js~QueryInterface.html#instance-method-addConstraint)**

테이블에 제약 조건을 추가한다.

```jsx
queryInterface.addConstraint("테이블_이름", "옵션");
//테이블 이름: string, 옵션: object
```

[추가할 수 있는 제약 조건]

- UNIQUE
- DEFAULT (MySQL only)
- CHECK (MySQL-Ignored by the database engine)
- FOREIGN KEY
- PRIMARY KEY

```jsx
// 옵션
		{
      fields: ['userId'], // 추가한 필드 이름
      type: 'foreign key', // 제약 조건의 타입
      name: 'userIdFK', // 제약 조건의 이름
      references: { // foreign key 생성을 위해 참조하는 값
        table: 'users', // 참조할 table
        field: 'id' // 참조할 칼럼 값
      },

```

옵션 다음 매개 변수로 `onDelete: 'cascade'`, `onUpdate: 'cascade'`값이 들어있는데, 아마 association 업데이트와 삭제 시 cascade를 사용해야 한다는 부분 때문에 적어주는 것 같다.

[참고 사이트](https://sequelize.org/master/class/lib/dialects/abstract/query-interface.js~QueryInterface.html)에 foreign key를 추가하는 것 외에도 다른 옵션을 추가하는 예시가 나와있으니까 한번 확인해보자!

**removeConstraint [(참고)](https://sequelize.org/master/class/lib/dialects/abstract/query-interface.js~QueryInterface.html#instance-method-removeConstraint)**

```jsx
queryInterface.removeConstraint("테이블_이름", "제약조건의_이름", "옵션");
```

### 4. migration 실행

이렇게 해준 뒤 다시 migration을 실행해주면, url 테이블에 userId가 생길 것이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a54bd0fd-52c6-418c-b1f7-ae5c15d089cc/Untitled.png)

---

이번에 이렇게 sequelize에서 1:N 관계를 가진 테이블을 생성해보았는데, 사실 공식 문서만 보고 하려니, 너무 어려운 점이 많았고, 제약 조건을 추가하는 부분도 헷갈려서, 코드를 작성하기가 쉽지 않았던 것 같다.

그래도 계속해서 이렇게 공식 문서를 보는 법을 익히다 보면, 익숙해져서 나중에 더 쉽게 볼 수 있지 않을까? 싶다
