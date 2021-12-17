# JSP(JavaServer Pages)

### JSP란?
- 서블릿을 통해 HTML을 문서를 작성할 수 있지만, print()안에 일일이 넣어야 한다는<br>
  단점이 존재했다.
- 이를 해결하기 위해 HTML형식으로 작성할 수 있는 JSP가 등장하였다.
- JSP는 실제 서블릿 기술을 사용한다.
- 즉, HTML과 비슷하게 생겼지만, HTML처럼 실행되는 것이 아니라 서블릿으로 봐뀐 이후에<br>
  서블릿 작동 방식으로 실행된다. 따라서 Lifecycle 또한 서블릿과 동일하다.

EX)
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

<%
	int total = 0;
	for(int i = 1; i <= 10; i++){
		total = total + i;
	}
%>

1부터 10까지의 합 : <%=total %>

</body>
</html>
```
Java코드는 <% %> 안에 집어넣는다. 그리고 <% %>안에 있던 코드들의 값들을 HTML에 포함시켜<br>
전송하기 위해서는 <%=변수명 %>을 사용한다. <%=변수명 %>은 println(변수명)과 동일한 의미이다.

### JSP 라이프싸이클
WAS는 웹 브라우저로부터 JSP에 대한 요청을 받게 되면, JSP코드를 서블릿 소스코드로 변환한 후 컴파일<br>
하여 실행되게 된다. 서블릿으로 컴파일되어 실행될 때 상황에 따라서 어떤 메소드들이 실행되는지 잘<br>
알아야, JSP를 알맞게 작성할 수 있다.<br>
<br>
JSP파일은 컴파일 시 Servlet코드로 변환된다. 변환된 파일은 아래 경로를 통해 찾을 수 있다.<br>
`.metadata` -> `.plugins` -> `org.eclipse.wst.server.core` -> `tmp0` -> `work` -> `Catalina`<br>
-> `localhost` -> `프로젝트명` -> `org` -> `apache` -> `jsp`<br>
경로에 들어가면 class파일과 java파일을 확인할 수 있다.<br>
<br>
java파일<br>
```
      out.write("\r\n");
      out.write("<!DOCTYPE html>\r\n");
      out.write("<html>\r\n");
      out.write("<head>\r\n");
      out.write("<meta charset=\"UTF-8\">\r\n");
      out.write("<title>Insert title here</title>\r\n");
      out.write("</head>\r\n");
      out.write("<body>\r\n");
      out.write("\r\n");

	int total = 0;
	for(int i = 1; i <= 10; i++){
		total = total + i;
	}

      out.write("\r\n");
      out.write("\r\n");
      out.write("1부터 10까지의 합 : ");
      out.print(total );
      out.write("\r\n");
      out.write("\r\n");
      out.write("</body>\r\n");
      out.write("</html>");
```
HTML문서의 각 줄을 out.writer()를 통해 변환된 것을 알 수 있다.

### sum10.jsp가 실행될 때 벌어지는 일
- 이클립스 워크스페이스 아래의 .metadata 폴더에 sum10_jsp.java파일이 생성된다.
- 해당 파일의 "jspService()"메소드 안을 살펴 보면 jsp파일의 내용이 변환되서 들어가<br>
  있는 것을 확인할 수 있다.
- sum10_jsp.java는 서블릿 소스로 자동으로 컴파일 되면서 실행되어 그 결과가 브라우저에 보여진다.

### JSP의 실행순서
1. 브라우저가 웹서버에 JSP에 대한 요청 정보를 전달한다.
2. 브라우저가 요청한 JSP가 최초로 요청했을 경우만
  1) JSP로 작성된 코드가 서블릿 코드로 변환된다. (java 파일 생성)
  2) 서블릿 코드를 컴파일해서 실행가능한 bytecode로 변환한다. (class 파일 생성)
  3) 서블릿 클래스를 로딩하고 인스턴스를 생성한다.
3. 서블릿이 실행되어 요청을 처리하고 응답 정보를 생성한다.

### 스크립트 요소의 이해
- JSP페이지에서는 선언문, 스크립트릿, 표현식 이라는 3가지의 스크립트 요소를 제공한다.
- 선언문(Declaration) - <%! %> : 전역변수 선언 및 메소드 선언에 사용
- 스크립트릿(Scriptlet) - <% %> : 프로그래밍 코드 기술에 사용
- 표현식(Expression) - <%=%> : 화면에 출력할 내용 기술에 사용

### 스크립트릿
```
<%
for(int i = 1; i <= 5; i++){
%>
<H<%=i %>> 아름다운 한글 </H<%=i %>>
<%
}
%>
```
위와 같은 방식을 수행했을 때, for문 안에있는 HTML코드가 반복되어 실행된다.<br>

### 주석
- 사용가능한 주석 : HTML 주석, Java 주석, JSP 주석
- JSP 주석은 JSP에서 Java코드로 변환될 때 주석부분은 변환되지 않는다.
- Java 주석은 JSP에서 Java코드로 변환될 떄는 함께 변환되지만,<br>
  Java에서 HTML으로 변환될 때 Java주석부분은 변환되지 않는다.
- HTML주석은 함께 변환되지만, 브라우저에서는 주석으로 표시되어 화면에 나타나지 않는다.

### JSP 내장객체란
- JSP를 실행하면 서블릿 소스가 생성되고 실행된다.
- JSP에 입력한 대부분의 코드는 생성되는 서블릿 소스의 _ jspService() 메소드 안에 삽입되는 코드로 생성된다.
- _ jspService()에 삽입된 코드의 윗 부분에 미리 선언된 객체들이 있는데, 해당 객체들은 jsp에서도 사용가능하다.
- response, request, application, session, out과 같은 변수를 내장객체라고 한다.
```
      response.setContentType("text/html; charset=UTF-8");
      pageContext = _jspxFactory.getPageContext(this, request, response,
      			null, true, 8192, true);
      _jspx_page_context = pageContext;
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
      _jspx_out = out;
```

### 내장 객체의 종류
- request : HTML Form 요소 선택 값과 같은 사용자 입력 정보를 읽어올 때 사용
- response : 사용자 요청에 대한 응답을 처리할 때 사용
- pageContext : 현재 JSP 실행에 대한 context 정보를 참조하기 위해 사용
- session : 클라이언트 세션 정보를 처리하기 위해 사용
- application : 웹 서버의 어플리케이션 처리와 관련된 정보를 참조하기 위해 사용
- out : 사용자에게 전달하기 위한 output 스트림을 처리하기 위해 사용
- config : 현재 JSP에 대한 초기화 환경을 처리하기 위해 사용
- page : 현재 JSP 페이지에 대한 클래스 정보
- exception : 예외 처리를 위해 사용



