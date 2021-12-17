# scope

### 4가지의 scope
- Application : 웹 어플리케이션이 시작되고 종료될 때까지 변수가 유지되는 경우 사용
- Session : 웹 브라우저 별로 변수가 관리되는 경우 사용
- Request : http요청을 WAS가 받아서 웹 브라우저에게 응답할 때까지 변수가 유지되는 경우 사용
- Page : 페이지 내에서 지역변수처럼 사용<br>
<img src="https://github.com/JMsuper/boostcourse_web_backend/blob/main/img/JSP%20scope.png" width=500><br>

### Page scope
- PageContext 추상 클래스를 사용한다.
- JSP 페이지에서 pageContext 라는 내장 객체로 사용가능 하다.
- forward가 될 경우 해당 Page scope에 지정된 변수는 사용할 수 없다.
- 사용방법은 다른 scope들과 동일하다
- 마치 지역변수처럼 사용된다는 것이 다른 Scope들과 다르다.
- JSP에서 pageScope에 값을 저장 한 후 해당 값을 EL표기법에서 사용할 때 사용된다.<br>
  지역 변수처럼 해당 JSP나 서블릿이 실행되는 동안에만 정보를 유지하고자 할 때 사용된다.
- pageContext는 실제 사용할 일이 많이 없고, JSP에서 서블릿으로 변환될 때 내장 객체 초기화에<br>
  이용된다.

JSP를 서블릿으로 변환한 코드 중 일부
```
pageContext = _jspxFactory.getPageContext(this, request, response,
      			null, true, 8192, true);
      _jspx_page_context = pageContext;
      application = pageContext.getServletContext();
      config = pageContext.getServletConfig();
      session = pageContext.getSession();
      out = pageContext.getOut();
```

### request scope
- http요청을 WAS가 받아서 웹 브라우저에게 응답할 때까지 변수값을 유지하고자 할 경우
- HttpServletRequest객체를 사용한다.
- JSP에서는 request내장 변수를 사용한다.
- 서블릿에서는 HttpServletRequest객체를 사용한다.
- 값을 저장할 때는 request객체의 setAttribute()메소드를 사용한다.
- 값을 읽어들일 때는 request객체의 getAttribute()메소드를 사용한다.
- forward시 값을 유지하고자 사용한다.
- 앞에서 forward에 대하여 배울 때 forward하기 전에 request객체의 setAttribute()메소드로 값을<br>
  설정한 후, 서블릿이나 jsp에게 결과를 전달하여 값을 출력하도록 하였는데 이렇게 포워드 되는<br>
  동안 값이 유지되는 것이 Request scope를 이용했다고 한다.

forward()를 수행하는 서블릿
```
request.setAttribute("v1", v1);
...
requestDispatcher.forward(request,response);
```
forward() 당하는 서블릿
```
int v1 = (int)request.getAttribute("v1");
```

### Session scope
- 웹 브라우저 별로 변수를 관리하고자 할 경우 사용한다.
- 웹 브라우저간의 탭간에는 세션정보가 공유되기 때문에, 각각의 탭에서는 같은 세션정보를<br>
  사용할 수 있다.
- HttpSession인터페이스를 구현한 객체를 사용한다.
- JSP에서는 session내장 변수를 사용한다.
- 서블릿에서는 HttpServletRequest의 getSession()메소드를 이용하여 session객체를 얻는다.
- 값을 저장할 때는 session 객체의 setAttribute()메소드를 사용한다.
- 값을 읽어들일 때는 session 객체의 getAttribute()메소드를 사용한다.
- 장바구니처럼 사용자별로 유지가 되어야 할 정보가 있을 때 사용한다.

### Application scope
- 웹 어플리케이션이 시작되고 종료될 때까지 변수를 사용할 수 있다.
- ServletContext 인터페이스를 구현한 객체를 사용한다.
- jsp에서는 application 내장 객체를 이용한다.
- 서블릿의 경우는 getServletContext()메소드를 이용하여 application 객체를 이용한다.
- 웹 어플리케이션 하나당 하나의 application객체가 사용된다.
- 값을 저장할 때는 application 객체의 setAttribute()메소드를 사용한다.
- 값을 읽어들일 때는 application 객체의 getAttribute()메소드를 사용한다.
- 모든 클라이언트가 공통으로 사용해야할 값들이 있을 때 사용한다.

Servlet1
```
    ServletContext application = getServletContext();
		int value = 1;
		application.setAttribute("value", value);
```
Servlet2
```
    ServletContext application = getServletContext();
    int value = (int)application.getAttribute("value");
    value++;
    application.setAttribute("value", value);
```
위에서 application 객체에 집어넣은 value값은 다른 클라이언트에서 접속할 때도 공유되는 값이다.







