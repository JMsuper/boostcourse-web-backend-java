# Maven
참고링크 : https://www.boostcourse.org/web326/lecture/58937?isDesc=false
### Maven의 정의
-  애플리케이션 개발을 위해 반복적으로 진행해왔던 작업들을 지원하기 위한 도구
-  빌드, 패키징, 문서화, 테스트와 테스트 리포팅, git, 의존성관리, svn등과 같은 형상관리서버와 연동 및
배포 등의 작업을 손쉽게 할 수 있다.
- Maven을 이해하기 위한 Coc
Coc : 일종의 관습. 프로그램의 소스파일은 어떤 위치에 있어야 하고, 소스가 컴파일된 파일들은 어떤 위치에
있어야 하고 등을 미리 정해놓은 것.
- Maven을 사용한다는 것은 Coc를 알아가는 것이라고 말할 수 있다.

### Maven의 이점
- 의존성 라이브러리 관리
- 설정 파일에 몇 줄 적어줌으로써 직접 다운로드 받지 않아도 라이브러리를 사용 가능
- Maven을 사용하게 되면 Maven에 설정한 대로 모든 개발자가 일관된 방식으로 빌드를 수행 가능
- 또한 다양한 플러그인을 제공하여, 많은 일들을 자동화 가능

### Maven 기본
Archetype을 이용하여 Maven기반 프로젝트를 생성하면, 프로젝트 하위에 pom.xml파일이 생성됨
```
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>kr.or.connect</groupId>
    <artifactId>examples</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>mysample</name>
    <url>http://maven.apache.org</url>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>3.8.1</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```
- project : pom.xml 파일의 최상위 루트 엘리먼트
- modelVersion : POM model의 버전
- groupId : 프로젝트를 생성하는 조직의 고유 아이디를 결정. 일반적으로 도메인 이름을 거꾸로 적은 것
- artifacId : 버전 없는 jar파일의 이름.
- packaging : 해당 프로젝트를 어떤 형태로 packaging할 것인지 정함. jar, war,ear 등
- version : 프로젝트의 현재 버전. 개발 중인 프로젝트는 SNAPSHOT 접미사를 사용함.
- name : 프로젝트의 이름
- url : 프로젝트 사이트가 있을 경우 URL을 등록함
- dependencies : Dependency Management 기능의 핵심. 해당 엘리먼트 안에 필요한 라이브러리를 지정함.
