# Servlet
학습목표
- 자바 웹 어플리케이션의 구조를 이해한다.
- 서브릿에 대하여 이해한다.

### 자바 웹 어플리케이션(Java Web Application)
- WAS에 설치되어 동작하는 어플리케이션
- 자바 웹 어플리케이션에는 HTML,CSS,이미지,자바로 작성된 클래스, 각종 설정 파일 등이 포함된다.
- 블로그, 쇼핑몰과 같은 것들이 web app이다.

### 자바 웹 어플리케이션의 폴더 구조
<img src="https://github.com/JMsuper/boostcourse_web_backend/blob/main/img/%EC%9E%90%EB%B0%94%20%EC%9B%B9%20%EC%96%B4%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98%20%ED%8F%B4%EB%8D%94%20%EA%B5%AC%EC%A1%B0.png" width=700><br>
- WEB-INF 폴더와 web.xml파일은 중요하다.
- web.xml파일은 웹 어플리케이션에 관한 정보를 다 가지고 있는 파일이다.
- WEB-INF 폴더 하위 폴더인 lib 폴더에는 각종 jar파일 형태의 라이브러리 파일들이 들어간다.
- 작성한 servlet 파일들은 classes 폴더안에 들어간다.

### 이클립스에서 실행된 Dynamic Web Project
- 이클립스에서 Dynamic Web Project의 Servlet을 실행하면, 해당 프로젝트가 이클립스가 관리하는
  .metadata 폴더아래에 자바 웹 어플리케이션 폴더 구조로 만들어져 실행된다.
- 경로 `elipse-workspace` -> `.metadata` -> `.plugins` -> `org.elipse.wst.server.core`<br>
  -> `tmp0` -> `wtpwebapps`<br><br>
  <img src="https://github.com/JMsuper/boostcourse_web_backend/blob/main/img/%EC%9E%90%EB%B0%94%20%EC%9B%B9%20%ED%8F%B4%EB%8D%94.png" width="700"><br>
  실제 폴더에서 web.xml파일은 ROOT 폴더 하위 폴더에 들어가있다.
- web.xml 파일
  ```
  <?xml version="1.0" encoding="ISO-8859-1"?>
  <web-app version="2.5" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee"> </web-app>
  ```
### Servlet이란?
- 자바 웹 어플리케이션의 구성요소 중 동적인 처리를 하는 프로그램의 역할
- 서블릿을 정의 해보면
  - 서블릿(servlet)은 WAS에서 동작하는 Java 클래스이다.
  - 서블릿은 HttpServlet 클래스를 상속받아야 한다.
  - 서블릿만으로 HTML을 표현하기는 무리가 있다.
  - 서블릿과 JSP로부터 최상의 결과를 얻으려면, 웹 페이지를 개발할 때 이 두가지를
    조화롭게 사용해야 한다.
    예 : 웹 페이지를 구성하는 화면(HTML)은 JSP로 표현하고,
    복잡한 프로그래밍은 서블릿으로 구현한다.
    
### Servlet의 탄생배경
먼저, CGI(Common Gateway Interface)에 대해서 알아야 한다. 웹의 역사에서 동적 페이지를 필요해짐에 따라,
서버는 클라이언트의 요청에 응답하여 처리하는 프로그램이 필요해졌다. 즉, 미리 저장해두었던 정적 페이지가 아닌
요구사항에 맞춰 만들어진 동적 페이지를 응답해줘야 하는 상황에 직면했다.
이를 해결하기 위해 등장한 것이 CGI이다.

CGI란 웹 서버와 외부 프로그램 사이에 정보를 주고받는 일종의 규약을 말한다.
CGI의 의미는 공통 게이트웨이 인터페이스인데, 이를 풀어서 생각해보자.
`공통` : 너도 나도 포함된다. `게이트웨이` : 연결 다리. '인터페이스' : 상호간의 연결을 쉽게 해주는 것.<br>
<img src="https://github.com/JMsuper/boostcourse_web_backend/blob/main/img/CGI%20%ED%94%8C%EB%A1%9C%EC%9A%B0.png" width="700"><br>
그런데 CGI는 프로세스 단위로 실행된다는 단점을 가지고 있었다.
웹 서버에 요청이 많아지면 그 만큼 실행되는 프로세스가 많아져 서버에 부하가 발생한다.
이를 보완하기 위해 프로세스가 아닌 스레드 단위로 동작하는 프로그램이 등장하게 된다.
그것이 바로 Servlet이다.

