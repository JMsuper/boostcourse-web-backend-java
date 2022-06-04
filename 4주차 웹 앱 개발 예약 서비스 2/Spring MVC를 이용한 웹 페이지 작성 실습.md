# Spring MVC를 이용한 웹 페이지 작성 실습

## DispatchereServlet
DispatcherServlet이 FrontController 역할을 할 수 있도록 설정해야한다. 직접 설정하지 않으면 스프링은 DispatcherServlet이 프론트 컨트롤러의 역할을 수행하는 지 알 수 없다. 그리고, DispatcherServlet도 결국 서블릿이기 때문에 설정해야한다. 설정 방법으로는 3가지가 있다.
1. web.xml
2. ServletContainerInitializer
3. WebApplicationInitializer

### web.xml
```xml
<web-app>
  <display-name>Archetype Created Web Application</display-name>
  <servlet>
  	<servlet-name>mvc</servlet-name>
  	<servlet-class>>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>contextClass</param-name>
  		<param-value>org.springframework.web.context.support.AnnotationConfigWebApplicationContext</param-value>
  	</init-param>
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>kr.or.connect.mvc.exam.config.WebMvcContextConfiguration</param-value>
  	</init-param>
  	<load-on-startup>1</load-on-startup>
  </servlet>
  <servlet-mapping>
  	<servlet-name>mvc</servlet-name>
  	<url-pattern>/</url-pattern>
  </servlet-mapping>
</web-app>
```
서블릿 과정에서 각 서블릿을 web.xml에 등록했던 것과는 달리 Front Controller 하나만 web.xml에 등록한다.
이때 servlet-mapping에서는 `/`를 타나내는 url-pattern만 넣는다. 서블릿을 찾는 과정은 다음과 같다.
1. servlet-mapping에서 url-pattern과 일치하는 servlet-name을 찾는다.
2. 해당하는 servlet-name을 <servlet>태그에 담긴 서블릿 중에서 찾는다.
3. 일치하는 서블릿을 찾고 <servlet-class>에 해당하는 클래스를 생성한다.
4. <init-param>에 담긴 설정 값들을 불러온다. 와서 서블릿 객체 생성의 설정으로 넘긴다.

- AnnotationConfigWebApplicationContext : 어노테이션을 기반으로 하는 web application context의 구현체이다
- WebMvcContextConfiguration : WebMvcConfigurerAdapter를 상속한 자바 config 파일이다.

WebMvcContextConfiguration.java
```
// 자바 config 파일임을 명시
@Configuration
// 웹에서 필요로 하는 대부분의 빈 객체를 설정해준다. 
@EnableWebMvc
// 자동으로 Controller, Service, Repository, Component 어노테이션이 붙은 클래스를 찾아 스프링 컨테이너가 관리한다.
// DefualtAnnotationHandlerMapping, RequestMappingHandlerMapping구현체는 애플리케이션 컨텍스트에 있는
// 요청 처리 빈에서 RequestMapping 어노테이션이 적용된 클래스나 메소드를 찾아 HandlerMapping객체를 생성한다.
@ComponentScan(basePackages = {"kr.or.connect.mvcexam.controller"})
// @EnableWebMvc가 제공하는 설정들 이외에 커스텀한 설정을 적용시키기 위해서 WebMvcConfigureAdapter를 상속하여
// 메소드를 오바리이딩한다.
public class WebMvcContextConfiguration extends WebMvcConfigurerAdapter {
	// 정적 파일들에 대한 매핑
	@Override
	public void addResourceHandlers(ResourceHandlerRegistry registry) {
		registry.addResourceHandler("/assets/**").addResourceLocations("classpath:/META-INF/resources/webjars/").setCachePeriod(31556926);
		registry.addResourceHandler("/css/**").addResourceLocations("/css/").setCachePeriod(31556926);
		registry.addResourceHandler("/img/**").addResourceLocations("/img/").setCachePeriod(31556926);
		registry.addResourceHandler("/js/**").addResourceLocations("/js/").setCachePeriod(31556926);
	}
	
	// default servlet handler를 사용하게 한다.
	@Override
	public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
		configurer.enable();
	}
	
	// 특정 url에 대한 처리를 controller 클래스 작성 없이 매핑할 수 있도록 한다.
 	@Override
	public void addViewControllers(final ViewControllerRegistry registry) {
		System.out.println("addViewControllers가 호출됩니다");
		// "/" 라는 url이 들어오면 "main"이라는 view를 보여준다.
		registry.addViewController("/").setViewName("main");
	}
 	
 	@Bean
 	public InternalResourceViewResolver getInternalResourceViewResolver() {
 		InternalResourceViewResolver resolver = new InternalResourceViewResolver();
 		resolver.setPrefix("WEB-INF/views/");
 		resolver.setSuffix(".jsp");
 		return resolver;
 	} 	
}
  
```
  
- @EnableWebMvc
  - DispatcherServlet의 RequestMappingHandlerMapping, RequestMappingHandlerAdapter, ExceptionHandlerExceptionResolver, 
    MessageConverter 등 Web에 필요한 빈들을 자동으로 생성
  - 기본 설정 이외의 설정이 필요하다면 WebMvcconfigurerAdapter를 상속받도록 Java config class를 작성한 후, 필요한 메소드를 오버라이딩 한다.
  



