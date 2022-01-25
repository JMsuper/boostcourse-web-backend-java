# SpringJDBC
- JDBC 프로그래밍에서 반복적으로 나타나는 부분을 spring이 수행해준다.
- JDBC의 모든 저수준 세부사항을 스프링 프레임워크가 처리해준다.

### Spring jDBC 패키지
###### org.springframework.jdbc.core
jdbc 템플릿 클래스와 다양한 콜백 인터페이스를 포함하며 관련 클래스를 포함한다.
###### org.springframework.jdbc.datasource
datasource에 접근을 쉽게하는 유틸리티 클래스와 datasource 구현체 등을 제공한다.
###### org.springframework.jdbc.object
RDBMS의 조회, 갱신, 저장등을 객체로 제공
###### org.springframework.jdbc.support
sql exception 변환 기능과 약간의 유틸리티 클래스를 제공하고 있다.

### JDBC Template
- spring jdbc core에서 가장 중요한 클래스
- 리소스 생성, 소멸을 관리
- 스테이트먼트의 생성과 실행을 처리
- sql 조회, 업데이트 저장 프로시저를 호출하고 resultSet 반복호출 등을 실행
- JDBC 예외 발생 시 org.springframework.dao패키지에 정의되어 있는 일반적인 예외로 변환

### DTO란?
- DTO란 Data Transfer Object의 약자이다.
- 계층간 데이터 교환을 위한 자바빈즈이다.
- 여기서의 계층이란 컨트롤러 뷰, 비즈니스 계층, 퍼시스턴스 계층을 의미
- 일반적으로 DTO는 로직을 가지고 있지 않고, 순수한 데이터 객체이다.
- 각각의 데이터를 개별적으로 가지고 다니면 불편하기 때문에 가방에 담아다닌 것을 예로 들 수 있다.

### DAO란?
- DAO란 Data Access Object의 약자로 데이터를 조회하거나 조작하는 기능을 전담하도록 만든 객체이다.
- 보통 데이터베이스를 조작하는 기능을 전담하는 목적으로 만들어진다.

### ConnectionPool이란?
- DB에 연결하는 과정은 비용이 많이 든다.
- 커넥션 풀은 이러한 비용을 줄이기 위해 미리 커넥션을 여러 개 맺어둔다.
- 커넥션이 필요하면 커넥션 풀에게 빌려서 사용한 후 반납한다.
- 커넥션을 





