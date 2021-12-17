# Request, Resoponse 객체 이해하기
### 요청과 응답
- WAS는 웹 브라우저로부터 Servlet요청을 받으면,
  - 요청할 때 가지고 있는 정보를 HttpServletRequest객체를 생성하여 저장
  - 웹 브라우저에게 응답을 보낼 때 사용하기 위하여 HttpServletResponse객체를 생성
  - 생성된 HttpServletRequest, HttpServletResponse 객체를 서블릿에게 전달
  - 추상화를 통해 http프로토콜에 맞춰 직접 구현하지 않아도 된다.

<img src="https://github.com/JMsuper/boostcourse_web_backend/blob/main/img/HttpServletRequest.png" width="600"><br>

### HttpServletRequest
- http프로토콜의 request정보를 서블릿에게 전달하기 위한 목적으로 사용
- 헤더정보, 파라미터, 쿠키, URI, URL 등의 정보를 읽어 들이는 메소드를 가지고 있다.
- Body의 Stream을 읽어 들이는 메소드를 가지고 있다.

### HttpServletResponse
- WAS는 어떤 클라이언트가 요청을 보냈는지 알고 있고, 해당 클라이언트에게 응답을 보내기 위한
  HttpServletResponse객체를 생성하여 서블릿에게 전달
- WAS는 클라이언트의 주소를 알고 있어, 개발자는 Response객체에 정보만 담아두면 WAS가
  클라이언트에게 전달해준다.
- 서블릿은 해당 객체를 이용하여 content type, 응답코드, 응답 메시지등을 전송


