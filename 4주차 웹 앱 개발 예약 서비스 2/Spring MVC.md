# Spring MVC
## MVC?
- MVC는 Model-View-Controller의 약자이다.
### MVC Model 1
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/mvc1.JPG" width=700>
1. 클라이언트는 JSP page에 대한 요청을 애플리케이션 서버에 보낸다.<br>
2. JSP는 Java Bean에 접근하고 비즈니스 로직을 호출한다.<br>
3. Java Bean은 DB에 접근하여 데이터 처리를 진행한다<br>
4. JSP로 부터 생성된 응답은 클라이언트에게 전송된다.<br><br>

위에서 말하는 Java Bean은 자바에서 말하는 자바 빈이 아니라, DAO와 같이 데이터 처리를 수행하는 객체를 말한다.<br>
MVC model 1은 간단하게 구현할 수 있다는 장점이 있다. 하지만, JSP page에서 다음 페이지를 결정하는 로직을 포함하고 있으며,<br>
비즈니스 로직과 뷰 로직이 섞여있어 확장이 어렵다는 단점이 있다.<br>

### MVC Model 2
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/mvc2.JPG" width=700>
- Model : 모델은 뷰가 렌더링하는데 필요한 데이터이다. 예를 들어 사용자가 요청한 상품 목록이나, 주문 내역이 있다. model 1에서 Java Bean이<br>
  수행하는 것을 Model이 수행한다.<br>
- View : 웹 애플리케이션에서 뷰(View)는 실제로 보이는 부분이며, 모델을 사용해 렌더링한다.<br>
- Controller : 컨트롤러는 사용자의 액션에 응답하는 컴포넌트이다. 컨트롤러는 모델을 업데이트하고, 다른 액션을 수행한다.<br>
  컨트롤러는 모델과 뷰와 소통하면서 중간다리 역할을 하는 컴포넌트이다.
<br><br>
model 1에서 JSP가 요청에 대한 자바 빈을 호출하고 뷰를 생성하던 것을 Controller와 View로 나눈 것이다.<br>
MVC model 2의 장점은 navigation control이 중앙화되어있어 컨트롤러에서만 페이지를 라우팅하는 로직을 처리하면 된다.<br>
그러나, 컨트롤러의 코드가 수정되면 재배포해야한다는 단점이 있다.<br>


### MVC Model 2의 발전된 형태
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/mvc2_upgrade.JPG" width=700>
Mvc model 2는 컨트롤러에서 JSP로 forwarding하는 코드들이 반복되고 있다. 이러한 중복들을 제거하기 위해 프론트 컨트롤러를 생성한다.
프론트 컨트롤러는 요청을 받아 실제 로직을 처리하는 컨트롤러에게 전달한다. 이를 통해 로직을 처리하는 컨트롤러는 서블릿일 필요가 없어진다.
왜냐하면 프론트 컨트롤러에서 라우팅을 수행하기 때문이다.

#### 예제
Servlet1.java
```
@WebServlet("/1")
public class Servlet1 extends HttpServlet {
	
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String viewPath = "WEB-INF/jsp1.jsp";
		RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
		dispatcher.forward(request, response);
	}
}
```
Servlet2.java
```
@WebServlet("/2")
public class Servlet1 extends HttpServlet {
	
	@Override
	protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String viewPath = "WEB-INF/jsp2.jsp";
		RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
		dispatcher.forward(request, response);
	}
}
```
MVC model 2의 경우 각 요청마다 이에 응답하는 서블릿이 필요하다. 때문에 위 코드와 같이 중복되는 코드가 발생한다.<br>
이를 해결하기 위해 프론트 컨트롤러를 사용하는 것이다. 프론트 컨트롤러를 사용할 때의 코드는 아래와 같다.

Controller.java
```
public interface Controller {
	View process(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException;
}
```
Controller1.java
```
public class Controller1 implements Controller {
	
	@Override
	public
	View process(HttpServletRequest request, HttpServletResponse response) {
		String viewPath = "/WEB-INF/jsp1.jsp";
		return new View(viewPath);
	}

}
```
Controller2.java
```
public class Controller2 implements Controller {
	
	@Override
	public
	View process(HttpServletRequest request, HttpServletResponse response) {
		String viewPath = "/WEB-INF/jsp2.jsp";
		return new View(viewPath);
	}

}
```
FrontControllerServlet.java
```
@WebServlet("/")
public class FrontControllerServlet extends HttpServlet {
	
	private Map<String, Controller> controllerMap = new HashMap<String, Controller>();
	
	public FrontControllerServlet() {
		controllerMap.put("/frontcontroller/1", new Controller1());
		controllerMap.put("/frontcontroller/2", new Controller2());
	}
	
	@Override
	public void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		String url = request.getRequestURI();
		System.out.println(url);
		Controller controller = controllerMap.get(url);
		if(controller != null) {
			View view = controller.process(request, response);
			view.render(request, response);
		}else {
			response.setStatus(HttpServletResponse.SC_NOT_FOUND);
			return;
		}
	}
}
```
View.java
```
public class View {
	private String viewPath;
	
	public View(String viewPath) {
		this.viewPath = viewPath;
	}
	
	public void render(HttpServletRequest request, HttpServletResponse response) 
			throws ServletException, IOException {
		RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
		dispatcher.forward(request, response);
	}
}
```
기존에 중복되었던 코드들을 FrontController와 View를 통해 해결할 수 있다.


