[PostgreSQL 입문수업](https://youtu.be/dKuLA5BGPTY)

### 설치

- https://www.enterprisedb.com/downloads/postgres-postgresql-downloads
- \*error 발생 시 해결 방법
  https://heeonii.tistory.com/8

### 구조

- PostgreSQL은 서버 클라이언트 구조를 갖고 있음
- 사용자는 서버에 데이터를 직접 다룰 수 없지만, postgre 클라이언트를 이용해서 서버가 관리하는 데이터에 접근할 수 있음(ex) pgAdmin 4 GUI, psql CLI)
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e22bb858-5e03-4939-9893-160ef81a8c5b/Untitled.png)
- 여러 개의 table이 필요하고, table을 그룹핑한 게 schema, schema를 관리하는 그룹이 database, database를 그룹핑한 게 cluster (database server), cluster가 database의 실체
- 서버를 조작할 때 SQL이라는 언어를 사용 → SQL로 cluster에 접근해서 순차적으로 안쪽으로 접근
- table를 대상으로 필요한 명령을 보냄 → SQL이 전달 받은 database 서버는 사용자가 요청한 작업을 처리한 후에 그 결과를 클라이언트에 응답함
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6c5abf9b-b0eb-41f6-bddb-451a380cb9cd/Untitled.png)

### Client pgAdmin

- Connection
  - Host name/address - localhost
  - Port - 5432
  - Maintenance - postgres
  - Username - postgres

### Database

- Database 생성
- General - Database 이름을 정의하고, SQL 탭을 보면,
  ```sql
  CREATE DATABASE opentoturials
      WITH
      OWNER = postgres
      ENCODING = 'UTF8'
      CONNECTION LIMIT = -1
      IS_TEMPLATE = False;
  ```
- 기본적으로 postgres는 public이라는 schema가 존재함

### Table

- Table 생성
  ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5aa8c1c4-9510-462a-9d0e-6c269b42e047/Untitled.png)
  ```sql
  CREATE TABLE public.topic
  (
      id serial NOT NULL,
      title character varying(50) NOT NULL,
      body text,
      created timestamp with time zone NOT NULL DEFAULT now(),
      PRIMARY KEY (id)
  );

  ALTER TABLE IF EXISTS public.topic
      OWNER to postgres;
  ```

### SQL

- Structured Query Language
- CRUD = Insert/Select/Update/Delete
- SQL 작성 시 관습적으로 명령어는 대문자로 작성
  ```sql
  INSERT INTO topic (title, body) VALUES('Select', 'Select is ...');
  ```
- SELECT
  - 전체 데이터 SELECT
    ```sql
    SELECT * FROM topic;
    ```
  - SQL 명령어를 통해 데이터의 양을 줄일 수 있음
    ```sql
    SELECT id, title FROM topic;
    ```
    ```sql
    SELECT id, title FROM topic WHERE id = 1;
    ```
    ```sql
    SELECT id, title FROM topic WHERE id <> 1;
    ```
    ```sql
    SELECT id, title FROM topic WHERE id > 1;
    ```
    ```sql
    SELECT id, title FROM topic WHERE id > 1 ORDER BY id DESC;
    ```
    ```sql
    SELECT id, title FROM topic WHERE id > 1 ORDER BY id DESC LIMIT 2;
    ```
- UPDATE
  - 일부 데이터 수정
    ```sql
    UPDATE topic SET title = 'Delete', body = 'Delete is ...' WHERE id = 4;
    ```
  - WHERE문 빠뜨리면 모든 데이터 변경됨
- DELETE
  - 일부 데이터 삭제
    ```sql
    DELETE FROM topic WHERE id = 4;
    ```
  - WHERE문 빠뜨리면 인생이 뒤바뀜
