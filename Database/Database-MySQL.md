MySQL 사용 방법을 정리해보았다.

MySQL 실행 → 터미널에서 입력

```jsx
mysql - uroot - p;
```

비번 설정

```jsx
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'yourPassword';
```

#

## SQL DATABASE

**데이터 베이스 만들기**

```jsx
CREATE DATABASE 데이터베이스_이름
```

**데이터 베이스 삭제하기**

```jsx
DROP DATABASE 데이터베이스_이름
```

**데이터베이스 사용하기**

```jsx
USE 데이터베이스_이름
```

`USE`를 통해 데이터 베이스를 선택하면 테이블을 만들 수 있다.

**테이블 만들기**

```jsx
CREATE TABLE 테이블_이름 (
  id int PRIMARY KEY AUTO_INCREMENT, //필드타입 - 숫자, Primary key 이면서 자동 증가하도록 설정
  name varchar(255),//필드타입 - 문자열 (최대 255개의 문자)
  email varchar(255)
);
```

**테이블 정보 확인**

```jsx
DESCRIBE 테이블_이름;
```

**테이블 삭제**

```jsx
DROP TABLE 테이블_이름;
```

**테이블 내 데이터 모두 삭제**

```jsx
TRUNCATE TABLE 테이블_이름;
```

**Column 추가**

```jsx
ALTER TABLE 테이블_이름 ADD 칼럼이름 타입

// Persons라는 테이블에 DATE 타입을 가진 Birthday 칼럼을 추가
ALTER TABLE Persons ADD Birthday DATE
```

**Column 삭제**

```jsx
ALTER TABLE 테이블_이름 DROP COLUMN 칼럼_이름;
```

#

## SQL 기본 쿼리문

**Select**

```jsx
SELECT * FROM Customers; //Customers 테이블에서 모든 내용 선택
SELECT City FROM Customers; // Customers 테이블에서 City 칼럼만 선택

// 유니크한 값을 받고 싶을 때 사용한다. Country를 기준으로 유니크한 값들만 선택
SELECT **DISTINCT** Country FROM Customers;
```

**Where**

필터 역할을 한다. 선택적으로 사용할 수 있다.

```jsx
SELECT * FROM Customers **WHERE** City="Berlin";
SELECT * FROM Customers **WHERE NOT** City="Berlin";
SELECT * FROM Customers WHERE CustomerID = 32;

//and, or을 사용하여 필터링 할 수 있다.
SELECT * FROM Customers WHERE City = 'Berlin' **AND** PostalCode = 12209;
SELECT * FROM Customers WHERE City = 'Berlin' **OR** City = 'London';
```

**Order By**

오름차순으로 정렬한다. DESC를 붙이면 내림차순으로 정렬할 수 있다.

```jsx
SELECT * FROM Customers **ORDER BY** City; //오름차순으로 정렬
SELECT * FROM Customers ORDER BY City **DESC**; //내림차순으로 정렬
SELECT * FROM Customers ORDER BY Country, City; // 먼저 국가 순, 도시 순으로 정렬
```

**Insert Into**

테이블의 칼럼에서 **값을 추가**한다.

```jsx
**INSERT INTO** table_name (column1, column2, column3, ...)
**VALUES** (value1, value2, value3, ...);
```

**Null**

데이터베이스에서 값이 지정되있지 않으면 null로 지정된다. null로 접근할 때는 is 혹은 is not을 붙여서 접근할 수 있다.

```jsx
// PostalCode columns에서 비어있는 것만 고르시오
SELECT * FROM Customers WHERE PostalCode **IS NULL;**

// Postal columns에서 비어있지 않은 것을 고르시오
SELECT * FROM Customers WHERE PostalCode **IS NOT NULL;**
```

**Update A set B = C**

A 테이블 내에서 존재하는 B특성(column)의 C값(records)을 바꾸기 위해 사용된다.

```jsx
UPDATE Customers set City = 'Oslo';

//Customers column에 City
UPDATE Customers set City = 'Oslo' WHERE Country = 'Norway;

UPDATE Customers set City = 'Oslo', Country = 'Norway WHERE CustomerID = 32;
```

**Delete from**

테이블 내에서 존재하는 값(records)를 삭제한다.

```jsx
DELETE FROM Customers
DELETE FROM Customers WHERE Country = 'Norway';
```

**Like**

```jsx
// City column의 값 중 **a로 시작하는 것**을 찾는다.
SELECT * FROM Customers WHERE City **LIKE 'a%';**

// City column의 값 중 **a로 끝나는 것**을 찾는다.
SELECT * FROM Customers WHERE City **LIKE '%a';**

// City column의 값 중 **a를 포함한 것**을 찾는다.
SELECT * FROM Customers WHERE City **LIKE '%a%';**

// City column의 값 중 **a로 시작하고, b로 끝나는 것**을 찾는다.
SELECT * FROM Customers WHERE City **LIKE 'a%b';**

// City column의 값 중 a로 시작하는 것을 **포함하지 않는다+**.
SELECT * FROM Customers WHERE **NOT City LIKE 'a%';**
```

**Wildcards**

```jsx
// City column의 값 중 **두번째가 a**인 것을 찾는다.
SELECT * FROM Customers WHERE City LIKE **'_a%';**

// City column의 값 중 **첫번째가 a 혹은 c 혹은 s인 것**을 찾는다.
SELECT * FROM Customers WHERE City LIKE **'[acs]%';**

// City column의 값 중 **첫번째가 a에서 f사이인 것**을 찾는다.
SELECT * FROM Customers WHERE City LIKE **'[a-f]%';**

// City column의 값 중 **첫번째가 a 혹은 c 혹은 s가 아닌 것**을 찾는다.
SELECT * FROM Customers WHERE City LIKE **'[!acs]%';**
```

**In A**

column의 값 중 A 값과 같은 모든 것을 선택한다.

```jsx
// Country column의 값 중 France 혹은 Norway인 모든 것을 선택
SELECT * FROM Customers WHERE Country **IN** ('Norway', 'France')**;**

// Country column의 값 중 France 혹은 Norway이 아닌 것을 선택
SELECT * FROM Customers WHERE Country **NOT IN** ('Norway', 'France')**;**
```

**Between A and B**

A와 B 사이의 값을 선택한다.

```jsx
SELECT * FROM Products WHERE Price **BETWEEN 10 AND 20;**
SELECT * FROM Products WHERE Price **NOT BETWEEN 10 AND 20;**
SELECT * FROM Products WHERE Price **BETWEEN 'Geitost' AND 'Pavlova';**
```

**Alias (별칭)**

```jsx
//Customer table에서 PostalCode column을 Pno로 나타낸다.
SELECT CustomerName,Address,**PostalCode AS Pno** FROM Customers;

// Customers table을 Consumers로 나타낸다.
SELECT * FROM **Customers AS Consumers**;

```

**JOIN [(참고)](https://www.w3schools.com/sql/sql_join.asp)**

```jsx
// Orders와 Customers 두 테이블 중 서로 공통된 부분인 CustomerID로 서로 연결한다.
SELECT * FROM Orders LEFT JOIN Customers ON Orders.CustomerID = Customers.CustomerID

// Orders와 Customers 두 테이블 중 서로 공통된 부분인 CustomerID로 서로 연결한다.
SELECT * FROM Orders RIGHT JOIN Customers ON Orders.CustomerID = Customers.CustomerID

// Orders와 Customers 두 테이블 중 서로 공통된 부분인 CustomerID로 서로 연결한다.
SELECT * FROM Orders INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID
```

user, reservation이라는 2개의 테이블을 만들었다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a6579125-1d19-4af9-bd25-1a4c37ad38d2/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/10be6006-af7e-4160-8eb0-3b37850743e2/Untitled.png)

1. **(Inner) join**

⇒ `select * from user join reservation on [user.name](http://user.name/) = [reservation.name](http://reservation.name/);`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9d268736-809d-491f-a02f-7e9cb9969bc0/Untitled.png)

inner join을 실행하면 user와 reservation 이름이 같은 것만 내용이 합쳐져서 나오게 된다.

1. **Outer join**

**Left (outer) join** ⇒ `select * from user left join reservation on [user.name=reservation.name](http://user.name%3Dreservation.name/)`

user 테이블에 reservation 내용이 합쳐진다. reservation에서 없는 값은 null로 표시가 된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca2adb49-d4f5-4383-85fc-f7513c16c932/Untitled.png)

**Right (outer) join** ⇒ `select * from user right join reservation on[user.name=reservation.name](http://user.name%3Dreservation.name/);`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/db3d4148-cfd9-42a5-9ebd-0e071f6850ce/Untitled.png)

reservation 테이블을 기준으로 합쳐진다. user에서 없는 값은 null로 표시가 된다.

**Group By**

```jsx
// 각 Country마다 customer의 수를 정렬한다. (즉 Country값이 같은 것끼리 그룹화한다)
SELECT COUNT(CustomerID), Country, FROM Customers **GROUP BY** Country;

// 각 Country마다 customer의 수를 정렬한 후 가장 많은 수부터 정렬한다.
SELECT COUNT(CustomerID) AS Cnt, Country, FROM Customers **GROUP BY** Country **ORDER BY** Cnt DESC;
```

ex) user이라는 테이블이 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0e1b937b-8027-41ef-bbd9-62fcf518a111/Untitled.png)

**각 성별**마다 **평균 점수**를 보여준다.

⇒ `select **sex, avg(score)**as average from user **group by sex;**`

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/98c24486-e061-4114-9116-d714f01d432c/Untitled.png)

---

#

### SQL function - 집계 함수

**MIN(a)**

a column에서 가장 작은 값을 찾는다.

```jsx
SELECT MIN(Price) FROM Products;
```

**Max(a)**

a column에서 가장 큰 값을 찾는다.

```jsx
SELECT MAX(Price) FROM Products;
```

**COUNT**(a)

a의 총 개수를 계산해준다.

```jsx
SELECT COUNT(*) FROM Products WHERE Price=18;
// 모든 column 중 Price 값이 18인 것을 센다.
```

**AVG(a)**

a의 평균값을 계산한다. (average)

```jsx
SELECT AVG(Price) FROM Products;
```

**SUM(a)**

a의 합을 계산한다.

```jsx
SELECT SUM(Price) FROM Products;
```

**SQL GUI Support Tool**

- [MySQL Workbench](https://www.mysql.com/products/workbench/)
- [Sequel Pro](https://www.sequelpro.com/) (OSX 전용)
- [Table Plus](https://tableplus.com/)
- [DBeaver](https://dbeaver.io/download/)
- [DataGrip](https://www.jetbrains.com/datagrip/)
