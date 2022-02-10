# Spring MVC 구성요소
## Spring MVC 기본 동작 흐름
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/KakaoTalk_20220210_100805632.jpg" width=800><br>
위 그림에서 개발자가 작성해야 하는 부분은 파란색으로 빗금 칠해진 컴포넌트이다.(Controller, view name, Service, Repository, Database)<br>
Spring MVC의 핵심은 DispetcherServlet이다.
1. 클라이언트가 보낸 모든 요청은 Dispatcher Servlet이 받는다.
2. DispatcherServlet은 HandlerMapping으로 부터 요청에 맡는 controller를 dispatch한다. HandlerMapping은 요청 URL에 매핑된 controller를 선택하고, 해당 controller를 Dispatcher Servlet에게 전달한다.
3. DispatcherServlet은 controller의 실행되어야 할 비지니스 로직을 HandlerAdapter에게 보낸다.
4. HandlerAdapter는 controller의 비지니스 로직을 호출한다.
5. controller는 비지니스 로직을 실행하고 결과를 Model에 저장한다. 그리고 view name을 HandlerAdapter에게 전달한다.
6. DispatcherServlet은 view name에 해당하는 View를 처리하는 일을 ViewResolver에게 보낸다. ViewResolver는 view name에 매핑된 View를 반환한다.
7. DispatcherServlet은 랜더링된 프로세스를 반환된 View에 보낸다.
8. View는 Model에 담긴 데이터를 랜더링하고 응답을 반환한다.

## DispatcherServlet
- 프론트 컨트롤러(Front Controller)
- 클라이언트의 모든 요청을 받은 후 이를 처리할 핸들러에게 넘기고 핸들러가 처리한 결과를 받아 사용자에게 응답 결과를 보여준다.
- DispatcherServlet은 여러 컴포넌트를 이용해 작업을 처리한다.

### DispatcherServlet 내부 동작흐름
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/DispatcherServlet%EB%82%B4%EB%B6%80%EB%8F%99%EC%9E%91%ED%9D%90%EB%A6%84.JPG" width=700><br>

#### 요청 선처리 작업
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/Dispatcherservlet%EC%9A%94%EC%B2%AD%EC%84%A0%EC%B2%98%EB%A6%AC%EC%9E%91%EC%97%85.JPG" width=600><br>
- Locale결정 : 지역화. 사용자의 지역환경에 따라 다른 처리를 수행한다. 예를 들어 브라우저의 언어세팅된 것에 맞춰 응답하는 것이 있다.
- RequestContextHolder에 요청 저장 : thread 로컬 객체이다. request를 받아서 response를 응답할 때까지 HttpServletRequest 객체와
  HttpServletResponse 객체를 스프링이 관리하는 객체 안에서 사용할 수 있도록 하는 과정이다. 이 과정 덕분에 controller의 메소드에서는
  argument로 HttpServletRequest, HttpServletResponse 객체를 받을 수 있는 것이다.
- FlashMap 복원 : Redirect로 값을 전달할 때 사용한다. query param에 데이터를 담아 전송하면 보안, url 길이 등의 문제가 있다. 이를 해결
  하기 위해 FlashMap을 spring 3.1부터 지원한다. 이는 redirect이후 응답이 완료될 때 까지 session에 잠시 담았다가 소멸시키는 방식이다.
  참고자료 : https://ifuwanna.tistory.com/9
- 멀티파트요청 : 클라이언트로 부터 파일을 전송받을 때 multipart 타입으로 받게된다. 이를 처리하기 위한 과정이다.

##### 사용된 컴포넌트
- org.springframework.web.servlet.LocaleResolver
  - 지역 정보를 결정해주는 전략 오브젝트
  - 디폴트인 AcceptHeaderLocalResolver는 HTTP 헤더의 정보를 보고 지역정보를 설정한다.
- org.springframework.web.servlet.FlashMapManager
  - FlashMap 객체를 조회, 저장하기 위한 인터페이스
  - RedirectAttribute의 addFlashAttribute메소드를 이용해 저장
  - 리다이렉트 후 조회를 하면 바로 정보는 삭제된다.
- org.springframework.web.context.request.RequestContextHolder
  - 일반 빈에서 HttpServletRequest, HttpServletResponse, HttpSession 등을 사용할 수 있게 한다.
  - 해당 객체를 일반 빈에서 사용하게 되면, Web에 종속적이 될 수 있다.
- org.springframework.web.multipart.MultipartResolver
  - Multipart 파일 업로드를 위한 전략

#### 요청 전달
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/DIspatcherServlet%EC%9A%94%EC%B2%AD%EC%A0%84%EB%8B%AC.JPG" width=600><br>
- HandlerExecutionChain 발견 : 이는 요청 url에 매핑된 handler가 있는지 여부를 확인하는 과정
- HandlerAdapter 발견 : handler는 찾았는데 HandlerAdapter가 없다는 것은 서버 오류이다

##### 사용된 컴포넌트
- org.springframework.web.servlet.HandlerMapping
  - HandlerMapping구현체는 어떤 핸들러가 요청을 처리할지에 대한 정보를 알고 있다.
  - 디폴트로 설정되어 있는 핸들러매핑은 BeanNameHandlerMapping과 DefaultAnnotationHandlerMapping 2가지가 설정되어 있다.
- org.springframework.web.servlet.HandlerExecutionChain
  - HandlerExecutionChain구현체는 실제로 호출된 핸들러에 대한 참조를 가지고 있다.
  - 즉, 무엇이 실행되어야 될지 알고 있는 객체라고 말할 수 있으며, 핸들러 실행전과 실행후에 수행될 HandlerInterceptor도 참조하고 있다.
- org.springframework.web.servlet.HandlerAdapter
  - 실제 핸들러를 실행하는 역할을 담당
  - 핸들러 어댑터는 선택된 핸들러를 실행하는 방법과 응답을 ModelAndView로 변환하는 방법을 알고 있다.
  - 디폴트로 설정되어 있는 핸들러어댑터는 HttpRequestHandlerAdapter, SimpleControllerHandlerAdapter, AnnotationMethodHanlderAdapter 3가지이다.
  - @RequestMapping과 @Controller 어노테이션을 통해 정의되는 컨트롤러의 경우 DefaultAnnotationHandlerMapping에 의해 핸들러가 결정되고,
    그에 대응되는 AnnotationMethodHandlerAdapter에 의해 호출이 일어난다.

#### 요청 처리
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/DispatcherServlet%EC%9A%94%EC%B2%AD%EC%B2%98%EB%A6%AC.JPG" width=600><br>
- 인터셉터 : 일종의 필터라고 생각하면 된다. 5주차에서 다시 학습

##### 사용된 컴포넌트
- org.springframework.web.servlet.ModelAndView
  - ModelAndView는 Controller의 처리 결과를 보여줄 view와 view에서 사용할 값을 전달하는 클래스
- org.springframework.web.servlet.RequestToViewNameTranslator
  - 컨트롤러에서 뷰 이름이나 뷰 오브젝트를 제공해주지 않았을 경우 URL과 같은 요청정보를 참고해서 자동으로 뷰 이름을 생성해주는
    전략 오브젝트. 디폴트는 DefaultRequestToViewNameTranslator.

#### 예외 처리
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/DispatcherServlet%EC%98%88%EC%99%B8%EC%B2%98%EB%A6%AC.JPG" width=500><br>
예외 발생시 HandlerExceptionResolvere를 호출하여 불려져야할 핸들러를 실행한다. 그런데 만약 해당 핸들러에서 ModelAndView를
리턴하지 않는다면 예외를 다시 던지고, 리턴한다면 요청처리를 재개한다.

##### 사용된 컴포넌트
- org.springframework.web.servlet.handlerexceptionresolver
  - 기본적으로 DispatcherServlet이 DefaultHandlerExceptionResolver를 등록한다.
  - HandlerExceptionResolver는 예외가 던져졌을 때 어떤 핸들러를 실행할 것인지에 대한 정보를 제공한다.

#### 뷰 렌더링 과정
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/DispatcherServlet%EB%B7%B0%EB%A0%8C%EB%8D%94%EB%A7%81%EA%B3%BC%EC%A0%95.JPG" width=600><br>
View 구현체를 찾으면 해당 구현체로 랜더링을 진행하고 요청 처리를 재개한다. 만약 없으면 ServletException을 던진다.

#### 사용된 컴포넌트
- org.springframework.web.servlet.ViewResolver
  - 컨트롤러가 리턴한 뷰 이름을 참고해서 적절한 뷰 오브젝트를 찾아주는 로직을 가진 전략 오프젝트이다.
  - 뷰의 종류에 따라 적절한 뷰 리졸버를 추가로 설정해줄 수 있다.

#### 요청 처리 종료
<img src="https://github.com/JMsuper/boostcourse-web-backend-java/blob/main/img/DispatcherServlet%EC%9A%94%EC%B2%AD%EC%B2%98%EB%A6%AC%EC%A2%85%EB%A3%8C.JPG" width=350><br>
HandlerExecutionChain이 존재하지 않으면 곧바로 RequestHandledEvent를 발생시킨다. 이는 요청 전달과정에서<br>
HandlerExecutionChain이 발견되지 않았을 때 곧바로 Http 404를 전달하는 것과 동일한 과정이다.




