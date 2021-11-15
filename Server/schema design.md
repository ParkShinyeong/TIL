- **스키마 디자인**을 할 수 있다.
  - 앱에 필요한 테이블과 필드, 그리고 관계를 부여할 수 있다.
- 1:N, N:N 관계를 이해하고, 데이터베이스에서 테이블을 조작할 수 있다.
  - Foreign Key, Primary Key에 대해 이해할 수 있다.

---

## schema

데이터베이스에서 데이터가 구성되는 방식과 서로 다른 엔티티 간의 관계에 대한 설명

### entity

고유한 정보의 단위 ⇒ 데이터베이스에서 **테이블!**

### Field (필드)

엔티티의 특성을 설명

행렬이라면 **열(Column)**에 해당

### Record

테이블에 저장된 항목

행렬에서의 **행(row)**

### key

테이블의 각 레코드를 구분할 수 있는 값.

1. **Primary Key**

각 테이블의 **고유한 값**을 가지는 필드. 각 테이블의 레코드 하나를 가리키며, 자동적으로 값이 증가. 이 ID 필드는 해당 테이블의 기본 키 (Primary Key)역할을 한다.

- 테이블 내의 하나의 컬럼에만 부여할 수 있다

1. **Foreign key(외래 키)**

다른 테이블에서 **테이블의 기본 키(primary key, 고유 값)를 참조**할 때 해당 값을 외래 키 (Foreign key)라고 한다.

MySQL ⇒ [Foreign key 추가하기](http://tcpschool.com/mysql/mysql_constraint_foreignKey)

---

## 테이블과 테이블 간의 관계

- 1:1 관계
- 1:N 관계
- N:N 관계
- self referencing 관계

### 1:1 관계

하나의 레코드가 다른 테이블의 레코드 한 개와 연결된 경우

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/49e42298-c469-4d86-9ae9-20a5483d9b87/Untitled.png)

자주 사용하지 않는다. 이런 경우에는 그냥 phone_number을 user에 직접 저장하는 것이 나을 수 있다.

### 1: N관계

하나의 레코드가 서로 다른 여러 개의 레코드와 연결된 경우

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d6a3f6d1-909d-4373-a046-a58589fca445/Untitled.png)

이 경우 한 명의 유저가 여러 전화번호를 가질 수 있지만, 여러명의 유저가 하나의 전화번호를 가질 수는 없다. 관계형 데이터베이스에서 가장 많이 사용한다.

### N:N 관계

여러 개의 레코드가 다른 여러 개의 레코드와 연결된 경우 ⇒ Join 테이블을 만들어 관리한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b2929b3a-2166-44b4-a1f3-8f9e8b946ec0/Untitled.png)

여러개의 상품과 고객들 중, 고객 한명이 여러 상품을 구매할 수 있고, 상품 하나를 여러 사람이 살 수도 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ff110cf2-8363-431c-a544-faa4fc7d5d34/Untitled.png)

customer_package는 customer_id 와 Package_id를 묶어주는 역할을 한다.

join 테이블에도, join 테이블을 위한 기본 키가 반드시 존재해야 한다. (cp_id)

만약 외래키를 리스트 형식으로 관리하는 필드가 있다면, 어떤 문제가 발생할 수 있을까요?

### 자기참조 관계 (Self Referencing Relationship)

ex) 추천인이 누구인지 파악할 때

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/67ae3761-5f25-4301-8245-d1277ce0bd32/Untitled.png)

한명의 유저가 한 명의 추천인을 가질 수 있고, 여러 명의 유저가 한 명의 추천인을 가질 수 있어서 1: N의 관계라고 생각할 수도 있지만, 일반적으로 **일대다 관계는 서로 다른 테이블의 관계를 나타낼 때 표현**한다.

간단한 인스타그램 스키마 그려보기!

[DB diagram](https://dbdiagram.io/home)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/aba2542a-639b-4ff3-bac1-16f70dad369b/Untitled.png)

user와 follower의 관계, post와 hasttag의 관계 ⇒ N : M

user와 post의 관계, post와 좋아요, 댓글의 관계 ⇒ 1: N

reference

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ed14cc7d-d568-47e8-9b50-c45b6ce77545/Untitled.png)
