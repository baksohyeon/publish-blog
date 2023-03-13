---  
category: 질문 목록/Solved Problems/Concept  
share: true  
title: Schema와 Table 차이 (mySQL , PostgreSQL)  
---  
  
# Schema와 Table 차이 (mySQL , PostgreSQL)  
  
**관련 글**: [PostgreSQL 세팅](../../../Project/TIL%20%EB%A9%94%EB%AA%A8/PostgreSQL%20%EC%84%B8%ED%8C%85.md)  
  
---  
  
## 질문 내용  
  
뭔 차이임  
  
## 질문 관련 서치 내용  
  
> 참고 문서 1) [Difference Between Schema / Database in MySQL - Stack Overflow](https://stackoverflow.com/questions/11618277/difference-between-schema-database-in-mysql/19257781#19257781)  
  
- mySQL  
  [MySQL :: MySQL 8.0 Reference Manual :: 13.1.12 CREATE DATABASE Statement](https://dev.mysql.com/doc/refman/8.0/en/create-database.html)  
  schema is a collection of tables and a Database is a collection of schemas.  
  
`CREATE DATABASE` creates a database with the given name.  
To use this statement, you need the CREATE privilege for the database.  
`CREATE SCHEMA` is a synonym for CREATE DATABASE as of MySQL 5.0.2.  
  
- postgreSQL  
  [PostgreSQL: Documentation: 15: 5.9. Schemas](https://www.postgresql.org/docs/current/static/ddl-schemas.html)  
  PostgreSQL supports schemas, which is a subset of a database:  
  
> A database contains one or more named schemas, which in turn contain tables.  
> **Schemas** also contain other kinds of named objects, including data types, functions, and operators.  
> The same object name can be used in different schemas without conflict; for example, both schema1 and myschema can contain tables named mytable.  
> Unlike databases, schemas are not rigidly separated: a user can access objects in any of the schemas in the database they are connected to, if they have privileges to do so.  
  
in MySQL **schema** is synonym of **database**. Its quite confusing for beginner people who jump to MySQL and very first day find the word schema, so guys nothing to worry as both are same.  
  
When you are starting MySQL for the first time you need to create a database (like any other database system) to work with so you can **CREATE SCHEMA** which is nothing but **CREATE DATABASE**  
  
In some other database system schema represents a part of database or a collection of Tables, and collection of schema is a database.  
  
## PostgreSQL | 스키마(Schema) | 데이터베이스, 스키마, 테이블의 관계  
  
> 참고 문서 2) [PostgreSQL | 스키마(Schema) | 데이터베이스, 스키마, 테이블의 관계 | devkuma](https://www.devkuma.com/docs/postgresql/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4-%EC%8A%A4%ED%82%A4%EB%A7%88-%ED%85%8C%EC%9D%B4%EB%B8%94%EC%9D%98-%EA%B4%80%EA%B3%84/)  
  
PostgreSQL에서는 데이터베이스와 테이블과는 다른 스키마라는 것이 존재한다.  
  
### 스키마  
  
PostgreSQL에서 실제 데이터는 테이블에 저장된다.  
테이블은 목적에 따라 여러 개 만들 수 있고 그것을 정리된 것이 데이터베이스이며, 거기에 PostgreSQL에서는 데이터베이스에 스키마라는 것이 있다.  
  
<font style="color:indianred">스키마는 데이터베이스에 작성되는 테이블이나 함수 등의 개체를 그룹화하는 것</font>이다.  
스키마가 다르면 동일한 데이터베이스에도 동일한 테이블 이름으로 테이블을 만들 수 있다.  
데이터베이스를 작성하면 자동으로 public라는 특별한 스키마가 작성된다.  
  
public 스키마는 기본적으로 모든 역할에 권한과 CREATE 권한이 부여되며, public 스키마에 어떤 역할도 테이블을 만들 수 있다.  
  
public 스키마와는 별도로 스키마를 데이터베이스에 만들 수 있다. 다른 데이터베이스 시스템에서는 사용자 이름과 동일한 이름의 스키마 이름을 가진 스키마만 작성할 수 없는 것도 있지만, PostgreSQL에서는 모든 이름의 스키마를 만들 수 있다.  
  
### 테이블  
  
테이블은 스키마에 작성한다. 스키마가 다르면 같은 테이블 이름의 테이블도 만들 수 있다. 또한 스키마마다 테이블 등의 오브젝트를 작성할 수있는 권한을 설정할 수 있다.  
  
CREATE TABLE 명령으로 테이블을 만들 경우 테이블 이름에 스키마를 생략하면 기본적으로 public 스키마에 테이블이 만들어 진다. (역할 이름과 같은 스키마가 생성 된 경우는 제외)  
  
---  
