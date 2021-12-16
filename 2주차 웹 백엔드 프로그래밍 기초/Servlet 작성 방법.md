# Servlet 작성 방법
실제 개발에서 Servlet을 직접 사용하지는 않는다. 프레임워크들이 더 쉽게 구현할 수 있도록
도와주기 때문에 프레임워크를 쓴다. 그럼에도 불구하고, Servlet이 없이는 프레임워크들도 동작할 수
없기 때문에, Servlet을 이해하는 것은 웹의 동작을 이해하는데 도움이 된다.

### 학습 목표
- 서블릿을 작성할 수 있다.
- 서블릿 버전에 따른 web.xml을 적절하게 작성할 수 있다.

### Servlet 작성방법은 2가지로 나뉨
1. Servlet 3.0 spec 이상에서 사용하는 방법
- web.xml파일을 사용하지 않음.
- 자바 어노테이션(annotation)을 사용
2. Servlet 3.0 spec 미만에서 사용하는 방법
- Servlet을 등록할 때 web.xml파일에 등록

### Servlet 3.0 spec 이상
```
@WebServlet("/ttt")
public class TenServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public TenServlet() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		response.setContentType("text/html;charset=utf-8;");
		PrintWriter out = response.getWriter();
		out.print("<h1>1-10까지 출력!!</h1>");
		for(int i = 1; i <= 10; i++) {
			out.print(i+"<br>");
		}
		out.close(); 
	}
}
```
class 위에 있는 `@WebServlet("/ttt")`문구가 annotation이다. 이를 통해 web.xml없이 "/ttt" 요청이<br>
들어왔을 때, 해당 클래스의 메소드를 이용하도록 만들 수 있다.<br>
`response.setContentType("text/html;charset=utf-8;");`<br>
응답 객체의 ContentType을 지정한다. 클라이언트가 서버로 부터 데이터를 전송받을 때, 해당 데이터가<br>
어떤 타입의 데이터인지 알아야지 이를 parsing할 수 있다.<br>
`PrintWriter out = response.getWriter();`<br>
HttpServletResponse 객체의 메소드인 getWriter()함수는 PrintWriter 객체를 리턴한다.<br>
PrintWriter 객체를 통해 response body에 담길 데이터를 입력할 수 있다.<br>

### servlet 3.0 미만
web.xml 파일 중 일부<br>
```
  <servlet>
    <description></description>
    <display-name>TenServlet</display-name>
    <servlet-name>TenServlet</servlet-name>
    <servlet-class>exam.TenServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>TenServlet</servlet-name>
    <url-pattern>/ten</url-pattern>
  </servlet-mapping>
```
의미 : 클라이언트가 요청할 때 "/ten"이라는 url로 요청하게 되면, servlet-name이 같은 servlet을 찾아서<br>
실제 클래스인 "exam"이라는 패키지 안에 있는 "TenServlet" 실행시켜주세요.<br>
<br>
만약 찾지 못할 경우 404페이지를 리턴한다. 존재한다면, servlet-name을 보고 똑같은 이름의 servlet을 찾는다.











