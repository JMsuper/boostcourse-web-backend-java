# EL(Experssion Language)
참고자료 : https://www.boostcourse.org/web326/lecture/258517?isDesc=false
EL은 값을 표현하는 데 사용되는 스크립트 언어로서 JSP의 기본 문법을 보완하는 역할을 한다.<br>
JSP에 자바 코드가 들어가면 프론트엔드 개발자가 봤을 때 당황할 수 있다. 이러한 점을 고려하여<br>
JSP를 이해하기 쉽게 표현하기 위한 EL이 등장했다.

##### 주요 기능
- JSP의 스코프에 맞는 속성 사용. JSP의 내장객체보다 더 쉽게 스코프에 접근할 수 있다.
- 집합 객체에 대한 접근 방법 제공. 
- 수치 연산, 관계 연산, 논리 연산자 제공
- 자바 클래스 메소드 호출 기능 제공
- 표현언어만의 기본 객체 제공

##### 표현 방법
- 문법
  `${expr}` : expr은 EL에서 정의한 문법에 따라 값을 표현하는 식이다.
- 예제
  `<b>${sessionScope.member.id}</b>`

##### JSP 예제
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%
    pageContext.setAttribute("p1","page scope value");
    request.setAttribute("r1","request scope value");
    session.setAttribute("s1","session scope value");
    application.setAttribute("a1","application scope value");
%>
<html>
<head>
    <title>Title</title>
</head>
<body>
pageContext.getAttribute("p1") : <%=pageContext.getAttribute("p1")%><br>

pageContext.getAttribute("p1") : ${pageScope.p1}<br>
request.getAttribute("r1") : ${requestScope.r1}<br>
session.getAttribute("s1") : ${sessionScope.s1}<br>
application.getAttribute("a1") : ${applicationScope.a1}<br>

pageContext.getAttribute("p1") : ${p1}<br>
request.getAttribute("r1") : ${r1}<br>
session.getAttribute("s1") : ${s1}<br>
application.getAttribute("a1") : ${a1}<br>
</body>
</html>
```
