# redirect & forward

### redirect
- 리다이렉트는 http프로토콜로 정해진 규칙이다.
- 서버는 클라이언트로부터 요청을 받은 후, 클라이언트에게 특정 URL로 이동하라고 요청할 수 있다.<br>
  이를 리다이렉트라고 한다.
- 서버에서는 클라이언트에게 응답으로 상태코드를 302와 함께 이동할 URL정보를 Location 헤더에 담아<br>
  전송한다. 클라이언트는 서버로 부터 받은 상태값이 302이면 Location헤더값으로 재요청을 보내게 된다.<br>
  이때 브라우저의 주소창은 전송받은 URL로 바뀌게 된다.
- 서블릿이나 jsp는 redirect하기 위해서 HttpServletResponse가 가지고 있는 sendRedirect()메소드를<br>
  사용한다.
- 리다이렉트하면 클라이언트가 요청을 2번하게 된다.
- 처음 요청에 의해 생긴 객체와 리다이렉트 이후 생긴 객체는 서로 다른 객체이다.

EX)
```
response.sendRedirect("redirect02.jsp");
```

### redirect 동작 설명
<img src="https://github.com/JMsuper/boostcourse_web_backend/blob/main/img/redirect%20process.png" width=600>

### forward
<img src="https://github.com/JMsuper/boostcourse_web_backend/blob/main/img/forward%20process.png" width=600><br>
1. 웹 브라우저에서 Servlet1에게 요청을 보낸다.
2. Servlet1은 요청을 처리한 후, 그 결과를 HttpServletRequest에 저장한다.<br>
   HttpServletRequest 객체는 요청 과정동안 살아있기 때문에 해당 객체에 저장하는 것이다.
3. Servlet1은 결과과 저장된 HttpServletRequest와 응답을 위한 HttpServletResponse를 같은 웹<br>
   어플리케이션 안에 있는 Servlet2에게 전송한다.(forward)<br>
   Servlet2는 HttpServletRequest,Response 객체의 래퍼런스를 모르기 때문에 알려줘야 한다.
4. Servlet2는 Servlet1으로 부터 받은 HttpServletRequest와 HttpServletResponse를 이용하여<br>
   요청을 처리한 후 웹 브라우저에게 결과를 전송한다.

EX)
Servlet1.java<br>
```
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		int diceValue = (int)(Math.random() * 6) + 1;
		request.setAttribute("dice", diceValue);
		
		RequestDispatcher requestDispatcher = request.getRequestDispatcher("/next");
		requestDispatcher.forward(request, response);
	}
```
`request.setAttribute("dice", diceValue);` : request 객체에 "dice"라는 속성값으로 diveValue를 저장한다.<br>
이때 diveValue는 Object로 저장된다. 왜냐하면 다양한 type의 데이터가 저장될 수 있기 때문이다.<br>
`RequestDispatcher requestDispatcher = request.getRequestDispatcher("/next");` : 파라미터는 forward할 servlet의 경로이다.<br>
파라미터 값에 "/"를 가장 앞단에 두면 relative path로 작동한다. 본 servlet이 위치하던 폴더를 기준으로 servlet을 검색할 것이다.<br>
RequstDispathcer 클래스는 현재 request에 담긴 정보를 저장하고 있다가 그 다음 페이지에도 해당 정보를 볼 수 있게하는 클래스이다.<br>
`requestDispatcher.forward(request, response);` : request, response객체를 resquestDispatcher가 가지고 있는 경로의 servlet으로 이동한다.<br>

Servlet2.java<br>
```
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html");
		PrintWriter out = response.getWriter();
		out.println("<html>");
		out.println("<head><title>form</title></head>");
		out.println("<body>");
		
		// getAttribute는 Object를 리턴하기 때문에 
		// Interger로 형변환해줘야 한다.
		int dice = (Integer)request.getAttribute("dice");
		out.println("dice: " + dice + "<br>");
		for(int i = 0; i < dice; i++) {
			out.print("hello<br>");
		}
		out.println("</body>");
		out.println("</html>");
	}
```
`int dice = (Integer)request.getAttribute("dice");` : request에 저장된 attribute는 기본적으로 Object type이다.<br>
따라서 이를 알맞은 type으로 형변환해줘야 한다.<br>

### Servlet과 JSP연동
- Servlet은 프로그램 로직이 수행되기에 유리하다. IDE 등에서 지원을 좀 더 잘해준다.
- JSP는 결과를 출력하기에 Servlet보다 유리하다. 필요한 html문을 그냥 입력하게 된다.
- 프로그램 로직 수행은 Servlet에서, 결과 출력은 JSP에서 하는 것이 유리하다.
- Servlet과 JSP의 장단점을 해결하기 위해서 Servlet에서 프로그램 로직이 수행되고, 그 결과를
- JSP에게 포워딩하는 방법이 사용되게 되었다. 이를 Servlet과 JSP연동이라고 한다.

EX)
```
<%
	int v1 = (int)request.getAttribute("v1");
	int v2 = (int)request.getAttribute("v2");
	int result = (int)request.getAttribute("result");
%>
<%=v1 %> + <%=v2 %> = <%=result %>
```
forward()를 실행하는 Servlet에서의 코드는 서블릿과 서블릿과의 forward()와 동일하다.<br>
다만, JSP의 경우 forward()를 받는 서블릿의 코드를 JSP문법에 맞게 작성하면된다.<br>
이때 request는 JSP코드가 서블릿코드로 변환되면서 초기화되는 내장객체이기 때문에 사용가능하다.<br>


