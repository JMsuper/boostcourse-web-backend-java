# SQL
- SQL은 데이터를 보다 쉽게 검색하고 추가, 삭제, 수정 같은 조작을 할 수 있도록 고안된
컴퓨터 언어이다.
- 관계형 데이터베이스에서 데이터를 조작하고 쿼리하는 표준 수단이다.
- DML(Data Manipulation Language):데이터를 조작하기 위해 사용한다.
INSERT, UPDATE, DELETE, SELECT 등이 여기에 해당한다.
- DDL(Data Definition Language):데이터베이스의 스키마를 정의 하거나 조작하기 위해 사용한다.
CREATE, DROP, ALTER 등이 여기에 해당한다.
- DCL(Data Control Language):데이터를 제어하는 언어이다. 권한을 관리하고, 데이터의 보안,
무결성등을 정의한다. GRANT, REVOKE 등이 여기에 해당한다.

### 데이터베이스 생성하기
`create database connectdb`

### 데이터베이스 사용자 생성과 권한 주기
- Database를 생성했다면, 해당 데이터베이스를 사용하는 계정을 생성해야 한다. 또한, 해당 계정이<br>
데이터베이스를 이용할 수 있는 권한을 줘야 한다. 아래와 같은 명령을 이용해서 사용자 생성과 권한을<br>
줄 수 있다. db이름 뒤의 * 는 모든 권한을 의미한다. @'%' 는 어떤 클라이언트에서든 접근가능하다는 의미이고,
@'localhost'는 해당 컴퓨터에서만 접근가능하다는 의미이다.
- flush privileges는 DBMS에게 적용을 하라는 의미이다. 해당 명령을 반드시 실행해줘야 한다.
- 사용자 계정 : connectuser, 암호 : connect123!@#<br>
`grant all privileges on connectdb.* to connectuser@'%'identified by 'connect123!@#';`<br>
`flush privilegees`

### 생성한 db에 접속하기
`mysql -h127.0.0.1 -uconnectuser -p connectdb`

### MYSQL 연결 끊기
`quit`

### MYSQL 버전과 현재 날짜 구하기
`SELECT VERSION(), CURRENT_DATE;`

### 쿼리를 이용해서 계산식의 결과도 구할 수 있다.
`SELECT SIN(PI()/4), (4+1)*5`

### 테이블의 구성요소
- 테이블 : RDBMS의 기본적 저장구조. 한 개 이상의 column과 0개 이상의 row로 구성
- 열 : 테이블 상에서의 단일 종류의 데이터를 나타냄. 특정 데이터 타입 및 크기를 가지고 있음
- 행 : Column들의 값의 조합. 레코드라고 불린다. 기본키에 의해 구분되며 기본키는 중복될 수 없다.
- field : 행과 열의 교차점으로 field는 데이터를 포함할 수 있고 없을 때는 NULL을 가질 수 있다.

### examples.sql 파일을 가져와 데이터베이스에 저장하기
cmd창에서 해당 파일의 경로로 들어간다.<br>
`mysql -uconnectuser -p connectdb < examples.sql`


