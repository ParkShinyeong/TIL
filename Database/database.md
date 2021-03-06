백엔드 개발에서 매우 중요한 데이터베이스를 학습합니다. 이번 유닛은 다음의 세 가지의 큰 흐름을 따라 진행합니다.

- SQL 문법 ✅
- 스키마 디자인 (Schema design)
- Node.js에서 데이터베이스를 사용하는 방법

## **Achievement Goals**

- 3 Tier Architecture 를 이해한다.
- 영속성의 개념을 이해하고, 데이터베이스의 필요성을 인지한다.
- 데이터베이스 종류를 이해한다.
  - 관계형 데이터베이스와 NoSQL의 차이를 이해한다.
  - 관계형 데이터베이스 및 NoSQL이 어떤 경우에 적합한지 이해한다.
  ***
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/248cbe0e-49f6-4240-89f7-1d42b8d1b615/Untitled.png)
  클라 - 서버 - 데이터베이스

### 데이터 다루는 방법 ?

1. **In-Memory**

JavaScript 에서 데이터를 다룰 때에는 프로그램이 실행될 때만 존재하는 데이터가 있다.

변수를 만들어 데이터를 저장한 경우, 프로그램이 종료되면, 해당 프로그램에서 사용하던 데이터도 사라진다. 즉 데이터가 프로그램의 실행에 의존한다.

1. **File I/O**

파일을 읽는 방식으로 작동하는 형태 , 엑셀 시트, CSV같은 파일을 사용

- 데이터가 필요할 때마다 전체 파일을 매번 읽어야 한다. 파일의 크기가 커질수록 버겁고 비효율적
- 파일이 손상되거나, 여러 파일을 동시에 다루는 경우 등 복잡하고 데이터양이 많아질수록 데이터를 불러들이는 작업이 힘들어진다.

1. **관계형 데이터베이스**

- 하나의 CSV파일이나 엑셀 시트를 한 개의 테이블로 저장할 수 있다.
- 한 번에 여러개의 테이블을 가질 수 있어 SQL을 이용해 데이터를 불러오기 수월하다.
- 대용량의 데이터를 저장하기 위한 목적이 아니다.

## SQL

하나의 언어인 **SQL (Structured Query Language)**는 **데이터베이스 언어**로 관계형 데이터베이스에서 사용한다.

데이터베이스에 쿼리를 보내 원하는 데이터를 가져오거나 삽입할 수 있다. 또한 데이터가 구조화된(structured) 테이블을 사용하는 데이터베이스에서 활용할 수 있다.

<aside>
❓ **쿼리(query)**: 저장되어 있는 데이터를 필터하기 위한 질의문

</aside>

데이터 구조가 고정되어 있어야 한다.

### NoSQL

데이터의 구조가 고정되어 있지 않은 데이터베이스

테이블을 사용하지 않고, 데이터를 다른 형태로 저장한다.

ex) MongoDB(문서지향 데이터베이스)

---

### 트랜잭션 (transaction)

여러 개의 작업을 하나로 묶은 실행 유닛.

데이터 베이스의 상태를 변환시키는 논리적인 기능을 수행하기 위해 행해지는 하나 이상의 쿼리를 모아놓은 하나의 작업 단위

- 하나의 특정 작업으로 시작해 묶여 있는 모든 작업을 완료해야 정상적으로 종료한다.
- 하나의 트랜잭션에 속한 단 하나의 작업이 실패하면, 이 트랜잭션에 속한 모든 작업은 실패한 것으로 판단한다.

데이터베이스의 트랜잭션은 **ACID**라는 특징을 가지고 있다.

## ACID

데이터베이스 내에서 일어나는 하나의 트랜잭션의 안정성을 보장하기 위해 필요한 성질이다.

1. **Atomicity (원자성)**

하나의 트랜잭션이 속해있는 모든 작업이 **전부 성공하거나 전부 실패해서 결과를 예측**할 수 있어야 한다.

하나의 단위로 묶여있는 모든 작업이 성공하지 않으면, 모든 작업이 실패하게 만들어 기존 데이터를 보호한다.

1. **Consistency (일관성)**

**데이터 베이스 상태는 일관**되어야 한다.

하나의 트랜잭션 이전과 이후, 데이터 베이스의 상태는 이전과 같이 유효해야 한다.

즉 하나의 트랜잭션 이후 **데이터 베이스는 그 제약이나 규칙을 만족**해야 한다.

ex) 고객의 이름, 나이가 포함된 데이터베이스가 있을 때

- 이름 없는 고객 추가 하는 쿼리

- 기존 고객의 이름을 삭제하는 쿼리

이 두 가지 경우는 일관성을 위배한다.

1. **Isolation (격리성, 고립성)**

모든 트랜잭션은 다른 트랜잭션으로부터 독립되어야 한다.

동시에 여러 개의 트랜잭션들이 실행되었을 때, 각 트랜잭션은 격리되어 있어 연속적으로 실행된 것과 동일한 결과를 나타낸다.

1. **Durability (지속성)**

하나의 트랜잭션이 성공적으로 수행되었으면, 해당 트랜잭션에 대한 로그가 남아야 한다.

런타임 오류/ 시스템 오류 등이 발생해도 해당 기록은 영구적이어야 한다.

ex) 은행의 데이터베이스

## SQL vs NoSQL

**SQL** 구조화 쿼리 언어

- 관계형 데이터 베이스
- 만들어진 방식: 테이블의 구조, 데이터 타입을 사전에 정의하고, 테이블에 정의된 내용에 알맞은 형태의 데이터만 삽입한다.
- 행(row)과 열(column)으로 구성된 테이블에 데이터를 저장
  - 열 - 하나의 속성에 대한 정보
  - 행 - 각 열의 데이터 형식에 맞는 데이터가 저장
- 특정한 형식을 지키므로, 데이터를 사용하기 수월하다.
- 데이터를 입력할 때 스키마에 맞게 입력한다.
- 스키마가 뚜렷하게 보인다. 즉 테이블 간의 관계를 직관적으로 파악할 수 있다.

ex) MySQL, Oracle, SQLite, PostgresSQL, MariaDB 등등

**NoSQL** 비구조화 쿼리 언어

- 비관계형 데이터베이스
- 데이터가 고정되어 있지 않은 데이터베이스
- 데이터를 읽어올 때 스키마에 따라 데이터를 읽어온다. ('schema on read')라고 한다.
- 데이터를 입력하는 방식에 따라, 데이터를 읽어올 때 영향을 미친다.

ex)

- key-value타입 : 속성을 key-value로 나타냄 - _[Redis](https://redis.io/), [Dynamo](https://aws.amazon.com/ko/dynamodb/)_
- 문서형 (document) 데이터베이스 : 데이터를 테이블이 아닌 문서처럼 저장. JSON과 같은 유사한 형식의 데이터를 문서화하여 저장한다. _[MongoDB](https://www.mongodb.com/cloud/atlas)_
- **Wide-Column 데이터베이스** : 데이터베이스의 열(column)에 대한 데이터를 집중적으로 관리하는 데이터베이스입니다. 각 열에는 key-value 형식으로 데이터가 저장되고, 컬럼 패밀리(column families)라고 하는 열의 집합체 단위로 데이터를 처리할 수 있습니다. 하나의 행에 많은 열을 포함할 수 있어 유연성이 높습니다. 데이터 처리에 필요한 열을 유연하게 선택할 수 있다는 점에서 규모가 큰 데이터 분석에 주로 사용되는 데이터베이스 형식입니다. 대표적인 wide-column 데이터베이스에는 _[Cassandra](https://cassandra.apache.org/), [HBase](https://hbase.apache.org/)_ 가 있습니다.
- **그래프(Graph) 데이터베이스** : 자료구조의 그래프와 비슷한 형식으로 데이터 간의 관계를 구성하는 데이터베이스입니다. 노드(nodes)에 속성별(entities)로 데이터를 저장합니다. 각 노드 간 관계는 선(edge)으로 표현합니다. 대표적인 그래프 데이터베이스에는 _[Neo4J](https://neo4j.com/), [InfiniteGraph](https://objectivity.com/infinitegraph/)_ 가 있습니다.
