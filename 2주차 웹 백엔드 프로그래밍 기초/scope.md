# scope

### 4가지의 scope
- Application : 웹 어플리케이션이 시작되고 종료될 때까지 변수가 유지되는 경우 사용
- Session : 웹 브라우저 별로 변수가 관리되는 경우 사용
- Request : http요청을 WAS가 받아서 웹 브라우저에게 응답할 때까지 변수가 유지되는 경우 사용
- Page : 페이지 내에서 지역변수처럼 사용
<img src="https://github.com/JMsuper/boostcourse_web_backend/blob/main/img/JSP%20scope.png" width=500><br>

### Page scope
- PageContext 추상 클래스를 사용한다.
- JSP 페이지에서 pageContext 라는 내장 객체로 사용가능 하다.
- forward가 될 경우 해당 Page scope에 지정된 변수는 사용할 수 없다.
- 사용방법은 다른 scope들과 동일하다
- 마치 지역변수처럼 사용된다는 것이 다른 Scope들과 다르다.
- JSP에서 pageScope에 값을 저장 한 후 해당 값을 EL표기법에서 사용할 때 사용된다.
  지역 변수처럼 해당 JSP나 서블릿이 실행되는 동안에만 정보를 유지하고자 할 때 사용된다.
