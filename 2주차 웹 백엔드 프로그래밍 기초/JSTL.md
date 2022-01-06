# JSTL(JSP Standard Tag Library)
JSP 페이지에서 조건문 처리, 반복문 처리 등을 html tag형태로 작성할 수 있게 도와준다.<br>
초기에는 HTML코드와 JSP 스트립트릿 코드를 함께 썻다. 하지만, 프론트엔드 개발자 입장에서 유지보수가<br>
힘들다는 단점이 존재했다. 이런 문제를 해결하기 위해 등장한 것이 JSTL이다.<br>

#### JSTL 설치방법
참고링크 : https://www.boostcourse.org/web326/lecture/258521?isDesc=false<br>
참고링크 : https://stackoverflow.com/questions/31043869/intellij-and-jsp-jstl-cannot-resolve-taglib-for-jstl-in-tomcat7<br>
다운로드 링크 : http://tomcat.apache.org/download-taglibs.cgi<br>
위 링크에서 Impl, Spec, EL jar파일들을 다운받고 WEB-INF폴더 하위의 lib폴더에 넣어줘야 한다.<br>
인텔리제이를 사용할 경우, 톰캣 lib폴더에 추가해주고, project structure에 가서 라이브러리를 추가해야 한다.

#### JSTL 코어태그 : 변수 지원 태그 - set, remove
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<c:set var="value1" scope="request" value="kang"/>
<html>
<head>
    <title>Title</title>
</head>
<body>
성 : ${value1}<br>
<c:remove var="value1" scope="request"/>
성 : ${value1}<br>
</body>
</html>
```
taglib : jstl을 사용하기 위한 설정이다. 이때 uri값과 prefix를 지정해줘야 한다.<br>
set : 변수를 선언 및 초기화한다.<br>
remove : set되었던 변수를 제거한다.<br>

#### 코어태그 : 변수 지원 태그 - 프로퍼티, 맵의 처리 및 흐름제어 태그
`<c:set target="${some}" property="propertyName" value="anyValue">`<br>
some 객체가 자바빈일 경우 : some.setPropertyName(anyvalue)<br>
some 객체가 맵(map)일 경우 : some.put(propertyName, anyValue)<br>
<br>
if의 경우 java의 if와는 다르게 else가 없다.
```
<c:if test="조건">
    조건이 true일 경우에 실행되는 코드 body
</c:if>
```
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<c:set var="n" value="0" scope="request"/>
<html>
<head>
    <title>Title</title>
</head>
<c:if test="${n == 0}">
    n과 0은 같습니다.
</c:if>
<c:if test="${n == 10}">
    n과 10은 같습니다.
</c:if>
</body>
</html>
```

#### 코어 태그 : 흐름제어 태그 - choose
```
<c:choose>
    <c:when test="조건1">
      ...
    </c:when>
    <c:when test="조건2">
      ...
    </c:when>
    <c:otherwise>
      ...
    </c:otherwise>
</c:choose>
```
```
<c:choose>
    <c:when test="${score >= 90}">
        A학생입니다.
    </c:when>
    <c:when test="${score >= 80}">
        B학생입니다.
    </c:when>
    <c:otherwise>
        F학생입니다.
    </c:otherwise>
</c:choose>
```

#### 코어 태그 : 흐름제어 태그 - forEach
forEach 태그는 반복문을 수행하는 태그이다.
- 문법
```
<c:forEach var="변수" items="아이템" [begin="시작번호"] [end="끝번호"]>
...
${변수}
...
</c:forEach>
```
var : items 리스트의 각각의 원소들이 할당되는 변수<br>
items : 반복가능한 Collection. 배열, list, Enumeration, Map<br>
begin : items에 지정한 목록에서 값을 읽어올 인덱스의 시작값<br>
end : items에 지정한 목록에서 값을 읽어올 인덱스의 끝값
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%
    List<String> list = new ArrayList<>();
    list.add("hello");
    list.add("World");
    list.add("!!!!");

    request.setAttribute("list",list);
%>
<html>
<head>
    <title>Title</title>
</head>
<body>
<c:forEach items="${list}" var="item" >
    ${item} <br>
</c:forEach>>
</body>
</html>
```

#### 코어 태그 : 흐름제어태그 - import
url로 부터 읽어온 결과를 지정한 변수에 저장한다.
- 문법
```
<c:import url="URL" charEncoding="캐릭터인코딩" var="변수명" scope="범위">
    <c:param name="파라미터이름" value="파라미터값"/>
</c:import>
```
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<c:import url="https://google.com" var="urlValue" scope="request"/>

<html>
<head>
    <title>Title</title>
</head>
<body>
    ${urlValue}
</body>
</html>
```

#### 코어 태그 : 흐름제어태그 - redirect
지정한 페이지로 리다이렉트한다. Servlet에서 response.sendRedirect()를 수행하는 것과 유사하다.
- 문법
```
<c:redirect url="리다이렉트할 URL">
    <c:param name="파라미터이름" value="파라미터값">
</c:redirect>
```
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<c:redirect url="http://localhost:8080/WebservletTest_war_exploded/jstl05.jsp"/>
<html>
<head>
    <title>Title</title>
</head>
<body>

</body>
</html>
```

#### 코어 태그: 기타태그 - out
JspWriter는 JSP페이지가 생성하는 결과를 출력할 때 사용되는 출력 스트림이다. out태그는 해당 스트림에 데이터를 출력해준다.
- 문법
```
<c:out value="value" escapeXml="{true|false}" default="defalutValue"/>
```
escapeXml : 이 속성의 값이 true일 경우 '< , & , ' , "'와 같은 문자들을 변환해준다. 즉, javascript코드와
같은 코드들을 실행시키지 않고 코드 그 자체를 출력시켜준다.
```
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<c:set var="t" value="<script type='text/javascript'>alert(1);</script>"/>
<c:out value="${t}" escapeXml="true"/>
</body>
</html>
```



