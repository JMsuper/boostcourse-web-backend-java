# Servlet 라이프 싸이클
### 학습목표
- 서블릿의 생명주기를 이해한다.

### LifecycleServlet 작성
- 서블릿 생명주기를 확인할 수 있는 LifecycleServlet을 작성한다.
- HttpServlet의 3가지 메소드를 오버라이딩
  - init()
  - service(request, response)
  - destroy()
<br>
과정<br>
`클라이언트가 url을 통해 서버에게 요청` -> `서버는 url을 확인하여 mapping 되어있는 클래스를 찾음`
-> `해당 클래스가 메모리에 존재하는 지 확인` -> `존재하면 service()를 호출하고 존재하지 않으면 클래스의 객체 생성`

<img src="https://github.com/JMsuper/boostcourse_web_backend/blob/main/img/LifecycleServlet.png" width="400"><br>
init() : 클래스의 생성자와 유사하게 클래스가 최초 생성될 때 1번 호출된다.<br>
service() : 클라이언트로 부터 요청이 있을 때마다 호출된다.<br>
destroy() : 서버 코드의 수정과 같은 변화가 있거나, WAS가 종료될 때 호출된다.<br>

### service(request, response) 메소드
- HttpServlet의 service메소드는 템플릿 메소드 패턴으로 구현
  - 클라이언트의 요청이 GET일 경우에는 자신이 가지고 있는 doGet(request,response)를 호출
  - 클라이언트의 요청이 POST일 경우에는 자신이 가지고 있는 doPost(request,response)를 호출
- 즉, service 메소드는 클라이언트의 요청 종류에 따라 지정된 메소드를 호출한다.

service(request, response)메소드의 oracle document
`Receives standard HTTP requests from the public service method and dispatches them to the doXXX methods defined in this class.`

궁금증??
html에서 <form>을 통해 POST 요청을 서버에서 보냈다. 이때 input값이 파라미터값으로 함께 서버에
넘겨지는데, url에는 파라미터가 나타나지 않고 있다. 내 생각에는 url이 아니면, body에 담아서
보낸 것 같기도 하다. 이를 어떻게 찾아볼 수 있을까?
Network탭에서 headers가 아닌 payload를 확인해보면 request의 body 부분을 볼 수 있다.
