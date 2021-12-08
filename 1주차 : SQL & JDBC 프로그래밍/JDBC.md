# JDBC
### JDBC의 정의
- 자바를 이용한 데이터베이스 접속과 SQL 문장의 실행, 그리고 실행 결과로 얻어진
데이터의 핸들링을 제공하는 방법과 절차에 관한 규약
- 자바 프로그램내에서 SQL문을 실행하기 위한 자바 API
- SQL과 프로그래밍 언어의 통합 접근 중 한 형태
- 매번 SQL문을 입력하여 실행할 수 없기 떄문에 이를 프로그래밍화 한 것
JAVA는 표준 인터페이스인 JDBC API를 제공한다. 또한 데이터베이스 벤더, 또는 써드파티에서는
JDBC 인터페이스를 구현한 드라이버를 제공한다. ~따라서, 어느 회사의 데이터베이스인지 상관없이
똑같이 사용하면 된다.~

벤더란? 판매인 또는 판매업자를 가리키는 말. 예를 들어 컴퓨터의 하드웨어나 소프트웨어 제품을 판매할
경우, 해당 제품에 대해 책임을 지는 기업을 말한다.

### 필요한 환경 구성
- JDK 설치
- JDBC 드라이버 설치

### JDBC 사용 - 단계별 정리
1. import java.sql.*;
2. 드라이버를 로드한다.(각각의 DB벤더가 정한 절차를 따른다.)
3. Connection 객체를 생성한다.(DB접속)
4. Statement 객체를 생성 및 질의 수행
5. SQL문에 결과물이 있다면 ResultSet 객체를 생성한다.
6. 모든 객체를 닫는다.
(항상 연결하는게 있으면 닫아야 한다. DB는 접속할 수 있는 클라이언트가 무한대가 아니다.)

### JDBC 클래스의 생성관계
<img src="https://github.com/JMsuper/boostcourse_web_backend/blob/main/img/JDBC%20%EC%83%9D%EC%84%B1%EA%B4%80%EA%B3%84.PNG" width=500><br>
`DriverManager를 통해 Connection객체 생성` -> `Connection객체를 통해 Statement객체 생성` -> `Statement객체를 통해 ResultSet객체 생성`
닫을 때는 역순이다.
`ResultSet객체 해제` -> `Statement객체 해제` -> `Connection객체 해제`

### 단계별 설명1
- IMPORT
  `import java.sql.*`
- 드라이버 로드
  `Class.forName("com.mysql.jdbc.Driver")`;
  DB벤더에서 제공하는 객체이다. Class클래스의 forName메소드를 이용하면 해당 객체가
  메모리에 올라간다. new를 통한 객체 생성과 비슷한 동작을 한다. 어떤 벤더사의 DB를 사용하냐에
  따라서 forName()의 인자값은 달라진다.
- Connection 얻기
  - `String dburl = "jdbc:mysql://localhost/dbName";`
  - `Connection con = DriverManager.getConnection(dburl,ID,PWD);`
  
### 단계별 설명2
- Statement 생성
  `Statement stmt = con.createStatement();`
- 질의 수행
  `ResultSet rs = stmt.executeQuery("select no from user");`
  어떤 쿼리를 이용하는지에 따라 실행명령어는 달라진다.
  - any SQL : execute()
  - SELECT : executeQuery()
  - INSERT, UPDATE, DELETE : executeUpdate()

### 단계별 설명3
- ResultSet으로 결과 받기
  ```
  ResultSet rs = stmt.executeQuery("select no from user");
  while(rs.next())
    System.out.println(rs.getInt("no"));
  ```
  ResultSet객체인 rs에 저장되는 것은 쿼리 수행 결과값이 아니다. 결과값은 DB가 가지고 있고,
  ResultSet객체에 저장되는 것은 해당 결과를 가리키는 레퍼런스이다. 왜냐하면 쿼리결과가 10,000건이
  넘는 경우, 이를 서버에 바로 전달하면 서버에 무리가 가기 때문에, 서버에서 필요한 값을 꺼내오는
  형식으로 진행된다. 이떄 사용하는 메소드가 next()이다.
  next()는 처음에 쿼리 결과의 첫번째 레코드를 가리키며, 실행 이후에는 다음 레코드를 가리키게 된다.
- Close
  - `rs.close();`
  - `stmt.close();`
  - `con.close();`
