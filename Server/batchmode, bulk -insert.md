### 배치 모드 [(Batch Mode)](https://dev.mysql.com/doc/refman/8.0/en/batch-mode.html)

mysql 에서 필요한 명령을 정리한 파일을 작성하여, 배치모드로 실행 중인 mysql 서버에 한번에 적용한다.

**example ⇒ )**

cmarket 데이터 베이스 생성

```jsx
//mysql에 접속해서 생성
CREATE DATABASE cmarket;
```

schema.sql 파일 작성 - 데이터 베이스 테이블 생성

```jsx
CREATE TABLE users (
  id INT AUTO_INCREMENT,
  username varchar(255),
  PRIMARY KEY (id)
);

CREATE TABLE orders (
  id INT AUTO_INCREMENT,
  user_id INT,
  total_price INT,
  created_at datetime DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (id)
);

ALTER TABLE orders ADD FOREIGN KEY (user_id) REFERENCES users (id);
```

seed.sql 파일 작성 - 테이블에 레코드 삽입

```jsx
INSERT INTO items (name, price, image) VALUES ("노른자 분리기", 9900, "../images/egg.png"), ("2020년 달력", 12000, "../images/2020.jpg"), ("개구리 안대", 2900, "../images/frog.jpg"), ("뜯어온 보도블럭", 4900, "../images/block.jpg"), ("칼라 립스틱", 2900, "../images/lip.jpg"), ("잉어 슈즈", 3900, "../images/fish.jpg"), ("웰컴 매트", 6900, "../images/welcome.jpg"), ("강시 모자", 9900, "../images/hat.jpg");
INSERT INTO users (username) VALUES ("김코딩")
```

미리 구성되어 있는 Cmarket 스키마를 기반으로 MySQL 에서 데이터베이스 테이블을 작성하고 데이터를 저장한다.

```jsx
//CLI 터미널 환경에서 레포지토리에 진입하여 커맨드 입력
mysql -u root -p < root/schema.sql  //테이블 작성
//Path/to/schema.sql => 터미널의 현재 위치에서 schema.sql 위치까지의 경로
mysql -u root -p < root/seed.sql   //데이터 저장
```

### 잘못 생성된 데이터베이스 삭제 후 재생성

```jsx
	DROP DATABASE IF EXISTS 데이터베이스이름 CREATE DATABASE 데이터베이스이름
```

### mysql 비밀번호는 환경 변수로 분리

비밀번호를 코드에 작성할 수도 있지만, 보안상/ 편의상 이유로 환경변수로 분리해놓는다.

`.env` 파일 생성 → 환경 변수로 저장

`.env` 파일은 `.gitignore`에 저장

### 데이터베이스와 서버 인스턴스 연결 ⇒ mysql 모듈 사용

```jsx
var mysql = require("mysql");
var db_info = {
 host: "localhost",
 port: "3306",
 user: "username",
 password: "password",
 database: "db_name",
};

mysql.createConnection(db_info);
```

[https://poiemaweb.com/nodejs-mysql](https://poiemaweb.com/nodejs-mysql)

### 방금 삽입한 레코드의 id 가져오기

INSERT INTO 가 리턴하는 값을 살펴본다.

```jsx
{
  fieldCount: 0,
  affectedRows: 14, //삽입한 레코드의 개수..?
  **insertId: 0, // 방금 삽입한 레코드의 id**
  serverStatus: 2,
  warningCount: 0,
  message: '\'Records:14  Duplicated: 0  Warnings: 0',
  protocol41: true,
  changedRows: 0
}
```

### 여러 레코드 한번에 넣기

[https://www.w3schools.com/nodejs/nodejs_mysql_insert.asp](https://www.w3schools.com/nodejs/nodejs_mysql_insert.asp)

```jsx
INSERT INTO customers (name, address) VALUES ?
```

```jsx
if (err) throw err;
  console.log("Connected!");
  var sql = "INSERT INTO customers (name, address) VALUES **?**";
  var values = [
    ['John', 'Highway 71'],
    ['Peter', 'Lowstreet 4'],
    ['Amy', 'Apple st 652'],
    ['Hannah', 'Mountain 21'],
    ['Michael', 'Valley 345'],
    ['Sandy', 'Ocean blvd 2'],
    ['Betty', 'Green Grass 1'],
    ['Richard', 'Sky st 331'],
    ['Susan', 'One way 98'],
    ['Vicky', 'Yellow Garden 2'],
    ['Ben', 'Park Lane 38'],
    ['William', 'Central st 954'],
    ['Chuck', 'Main Road 989'],
    ['Viola', 'Sideway 1633']
  ];

  con.query(sql, **[values]**, function (err, result) {
    if (err) throw err;
    console.log("Number of records inserted: " + result.affectedRows);
  });
```
