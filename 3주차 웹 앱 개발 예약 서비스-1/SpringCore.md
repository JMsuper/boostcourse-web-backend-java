# SpringCore
스프링 프레임워크를 알기 위해서 먼저 프레임워크와 라이브러리의 차이를 알아야 한다.
### 프레임워크와 라이브러리의 차이
라이브러리를 사용하는 애플리케이션 코드는 애플리케이션 흐름을 직접 제어한다. 단지 동작하는 중에 필요한 기능이 있을 때 능동적으로 라이브러리를 사용할 뿐이다.<br>
반면에 프레임워크는 거꾸로 애플리케이션 코드가 프레임워크에 의해 사용된다. 보통 프레임워크 위에 개발한 클래스를 등록해두고, 프레임워크가 흐름을 주도하는 중에<br>
개발자가 만든 애플리케이션 코드를 사용하도록 만다는 방식이다.<br>

## Spring Framework란?
- 엔터프라이즈급 어플리케이션을 구축할 수 있는 솔루션
- 원하는 부분만 가져다 사용할 수 있도록 모듈화가 잘 되어 있다. 따라서, 필요한 모듈만 가져와 사용하면 된다.
- IoC 컨테이너이다. 번역하면 역전 제어 컨테이너 인데, 어플리케이션에서 사용하는 도구들을 앱 단에서 능동적으로 생성하지 않고, 스프링이 생성 및 관리하는 것을 말한다.
- 선언적으로 트랜잭션을 관리할 수 있다.
- 완전한 기능을 갖춘 MVC Framework를 제공
- AOP를 지원한다. AOP는 관점 지향 프로그래밍을 의미하며, 코드상에 중복해서 사용되는 부분들을 모듈화하여 비즈니스 로직으로 부터 분리하는 것을 말한다.

## 스프링의 핵심 : 빈 팩토리
스프링의 핵심은 빈 팩토리 또는 애플리케이션 컨텍스트라고 불리는 것이다. 이 두가지는 DaoFactory를 일반화한 것이다.<br>
스프링에서는 애플리케이션 컨텍스트를 IoC 컨테이너라 하기도 하고, 스프링 컨테이너라고 하기도 한다. 또는 빈 팩토리라고 부르기도한다. 
- 빈 팩토리 : 스프링에서 빈의 생성과 관계설정 같은 제어를 담당하는 IoC 오브젝트.
- 애플리케이션 컨텍스트 : 빈 팩토리를 좀더 확장한 것

스프링은 여러 번에 걸쳐 빈을 요청하더라도 매번 동일한 오브젝트를 돌려준다. 애플리케이션 컨텍스트는 싱글톤을 저장하고 관리하는 싱글톤 레지스트리이다.<br>
싱글톤 레지스트리는 일반적인 싱글톤 패턴과 다르다. 싱글톤 패턴은 private 생성자를 사용하기 때문에 상속이 불가능하다. 또한 만들어지는 방식이 제한적이여서<br>
테스트가 힘들다. 그 외에도 서버에서 싱글톤 1개만 생성되는 것을 보장하지 않으며, 객체 지향관점에서  전역 상태를 갖는것은 바람직하지 않다.<br>
싱글톤 레지스트리는 이러한 단점들을 해결해준다. 빈을 생성하는 클래스는  public생성자를 갖는다.다만, 스프링이 빈이 싱글톤으로 생성되도록 관리하는 것이다.<br>
스프링의 싱글톤 빈으로 사용되는 클래스를 만들 때는 기존의 UsreDao처럼 개별적으로 바뀌는 정보는 로컬 변수로 정의하거나, 파라미터로 주고받으면서 사용하게 해야 한다.<br>
만약 멤버변수로 정의할 경우 멀티쓰레드 환경에서 데이터가 뒤죽박죽이 되는 상황에 직면할 수 있다. 만약 자신이 사용하는 다른 싱글톤 빈을 저장하려는 용도라면<br>
인스턴스 변수를 사용해도 좋다. 또한 일기전용의 변수도 인스턴스 변수로 사용해도 된다.<br>

#### 빈(Bean)이란?
스프링에서는 스프링이 제어권을 가지고 직접 만들고 관계를 부여하는 오브젝트를 빈(bean)이라고 부른다. 자바빈 또는 EJB에서 말하는 빈과 비슷한 오브젝트 단위의<br>
애플리케이션 컴포넌트를 말한다. 동시에 스프링 빈은 스프링 컨테이너가 생성과 관계설정, 사용 등을 제어해주는 제어의 역전이 적용된 오브젝트를 가리키는 말이다.<br>
- 애플리케이션에서 만들어지는 모든 오브젝트가 다 빈은 아니다. 그 중에서 스프링이 직접 그 생성과 제어를 담당하는 오브젝트만을 빈이라고 부른다.

## Spring IoC/DI 컨테이너
#### 컨테이너(Container)란?
- 컨테이너는 인스턴스의 생명주기를 관리한다. 인스턴스의 생성과 소멸을 다른 누군가가 대신 해주는 것이다.
- 생성된 인스턴스들에게 추가적인 기능을 제공한다.
- 톰캣에서 서블릿 컨테이너가 이에 해당한다. 개발자가 서블릿 클래스를 작성하지만, 이를 생성하고 관리하는 것은 서블릿 컨테이너가 하는 일이다.
- JSP파일 또한, JSP가 서블릿으로 바뀌고 인스턴스가 생성되는 것도 톰캣이 관리하는 것이다.

### IoC란?
개발자는 프로그램의 흐름을 제어하는 코드를 작성한다. 그런데, 이 흐름의 제어를 개발자가 하는 것이 아니라 다른 프로그램이 그 흐름을 제어하는 것을 IoC라고 말한다.<br>
IoC를 사용하므로서 애플리케이션에서 필요로 하는 객체들의 관리를 위임하게 된다. 이로서 개발자는 비즈니스 로직에만 집중할 수 있다.<br>
또한, 코드의 수정사항이 생길 때 직접 객체 생성을 수정할 경우 일이 많아지고, 각각의 객체들간의 관계를 알고 있어야 하기 떄문에,<br>
유지보수에서도 장점을 갖는다.<br>

### DI란?
DI는 클래스 사이의 의존 관계를 빈(Bean) 설정 정보를 바탕으로 컨테이너가 자동으로 연결해주는 것을 말한다.
```
public class Engine{

}

public class Car{
    private Engine engine = null;

    Car(){
        engine = new Engine();
    }
}
```
위 경우는 DI가 적용되지 않았다. 직접 객체를 생성하고 있다. 만약 사용할 클래스가 달라질 경우 매번 코드르 수정해줘야 한다.
```
@Component
public class Engine{

}

@Component
public class Car{

    @Autowired
    private Engine engine;

}
```
위 경우는 DI가 적용되었다. 코드에서 직접 객체를 생성할 필요없이 어노테이션을 적용시켜 객체의 관리를 위임하는 것이다.

### Spring에서 제공하는 IoC/DI 컨테이너
- BeanFactory : IoC/DI에 대한 기본 기능을 가지고 있다.
- ApplicationContext : BeanFactory의 모든 기능을 포함하며, 일반적으로 BeanFactory보다 추천된다.
  트랜잭션처리, AOP등에 대한 처리를 할 수 있다. BeanPostProcessor, BeanFactoryPostProcessor등을 자동으로 등록하고,<br>
  국제화 처리, 어플리케이션 이벤트 등을 처리할 수 있다.
  - BeanPostProcessor : 컨테이너의 기본 로직을 오버라이딩해서 인스턴스화와 로직등을 개발자가 커스텀할 수 있게 하는 것
  - BeanFactoryPostProcessor : 설정 메타데이터를 커스텀할 수 있게 하는 것

## Spring IoC/DI 실습
### xml을 이용한 IoC
xml파일에 Bean의 정보를 입력하여 스프링 프레임워크가 해당 객체를 관리하도록 설정할 수 있다.<br>
xml파일은 `src/main/resources`에 저장한다. 기본적으로 `src`폴더 하위의 파일들은 자동으로 classPath에 저장된다.<br>
classPath는 말 그대로 JVM에서 클래스 파일을 찾는 데 기준이 되는 경로를 의미한다.<br>
src/main/resources/applicationContext.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

	<bean id="userBean" class="kr.or.connect.diexam01.UserBean"></bean>
</beans>
```
`<bean>`태그를 사용하여 빈 객체의 정보를 입력한다. `id`는 getBean()메소드를 통해 빈 객체를 가져올 때 사용되는 이름이며,<br>
`class`는 해당 빈의 클래스파일을 지정하는 속성이다.


```
public class ApplicationContext01 {

	public static void main(String[] args) {
		ApplicationContext ac = new ClassPathXmlApplicationContext("classpath:applicationContext.xml");
		System.out.println("초기화 완료!!");
		
		UserBean userBean = (UserBean)ac.getBean("userBean");
		userBean.setName("강");
		
		System.out.println(userBean.getName());
		
		UserBean userBean2 = (UserBean)ac.getBean("userBean");
		
		if(userBean == userBean2) {
			System.out.println("같은 인스턴스");
		}
	}

}
```
`ApplicationContext`는 인터페이스로 빈을 관리하는 클래스이다. 해당 클래스를 사용하려면 인터페이스를 실제 구현한 클래스들 중<br>
하나를 선택하여 사용한다. 그 중 ClassPathXmlApplicationContext는 입력받은 xml파일을 확인하여 해당 파일에 담긴 모든 빈들을<br>
메모리에 올려둔다. 즉, ClassPathXmlApplicationContext의 인스턴스가 생성될 때 xml파일을 확인하여 각각의 Bean들과 Bean들에는 <br>
해당하는 classPath를 확인하여 빈 객체를 생성한 후 메모리에 올려주는 것이다.<br>
<br>
xml에 지정했던 Bean의 `id`에 해당하는 이름을 getBean()메소드의 인자로 넣으면 해당하는 빈 객체의 래퍼런스를 반환한다.<br>
그런데 이때 Bean들은 다양한 종류이기 때문에 이를 모두 포함하기 위해 특정 type이 아닌 Object type으로 반환한다.<br>
따라서, getBean()을 통해 반환받은 래퍼런스를 변수에 할당할 때 형 변환을 시켜줘야 한다.<br>
<br>
애플리케이션 컨텍스트는 싱글톤 리포지토리이다. 따라서 Bean 객체는 1개만 생성되며 위 코드에서 `userBean1`과 `userBean2`는<br>
동일한 Bean 객체를 가리킨다.<br>

### xml을 이용한 DI
Engine 클래스의 인스턴스를 멤버 변수로 할당하는 Car 클래스를 정의할 것이다.<br>
Engine.java
```
public class Engine {
	public Engine() {
		System.out.println("엔진 생성자");
	}
	
	public void exec() {
		System.out.println("엔진이 동작합니다.");
	}
}

```
Car.java
```
public class Car {
	private Engine v8;
	
	public Car() {
		System.out.println("Car 생성자");
	}
	
	public Engine getEngine() {
		return v8;
	}

	public void setEngine(Engine e) {
		this.v8 = e;
	}

	public void run() {
		System.out.println("엔진을 이용하여 달린다.");
		v8.exec();
	}

}

```
위 Car 클래스의 setEngine(Engine e)메소드가 중요하다. 반드시 `set~~` 형태로 지정해야 하며, `set`뒤에 다오는 명칭은<br>
xml파일에 정의된 property의 name과 일치해야 한다. 두 명칭이 일치하지 않으면 어플리케이션 컨텍스트는 해당 Bean을 찾지 못하여<br>
객체를 주입시키지 못할 것이다.<br>
applicationContext.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
  
	<bean id="e" class="kr.or.connect.xmldi.Engine"></bean>
	<bean id="c" class="kr.or.connect.xmldi.Car">
		<property name="engine" ref="e"></property>
	</bean>
</beans>
```
Bean객체안에 Bean객체를 주입시키기 위해 `<property>`태그를 사용한다. `naem`은 Car클래스의 property와 일치해야하며, ref는 주입될 Bean이<br>
누구인지 명시해주는 속성이다. 즉, Bean 클래스의 `setter`와 xml의 `<property>`가 xml을 이용한 DI의 핵심이다.
### Configuration 어노테이션을 이용한 DI
jdk5부터 xml파일 대신 어노테이션을 이용하여 빈을 등록할 수 있다. `Car`,`Engine`클래스는 이전과 동일하다. 달라지는 것은<br>
`ApplicationCofig.java`파일이 새롭게 생성된다는 것이다. 해당 config파일이 xml을 대신한다.<br>
ApplicationConfig.java
```
@Configuration
public class ApplicationConfiguration {
	
	@Bean
	public Car car(Engine e) {
		Car c = new Car();
		c.setEngine(e);
		return c;
	}
	
	@Bean
	public Engine engine() {
		return new Engine();
	}
}
```
`@Configuration` 어노테이션을 클래스에 적용하면, applicationContext의 인스턴스가 생성될 때 해당 클래스에 등록된 빈들이 생성되게 된다.<br>
그런데 위 코드에서 만약 `car` 빈이 먼저 생성되면 어떻게 될까? 다행이도 그럴일은 없다. 스프링은 파라미터를 받지 않는 빈들을 우선적으로<br>
생성하고, 해당 빈들이 파라미터로 들어가질 빈들을 생성하게 된다. 빈 메소드들은 반드시 메소드에서 반환되는 클래스 명칭과 일치할 필요는 없다.<br>
테스트해보니 이름이 다르더라도 빈은 잘 주입되고 있었다. 아마도, 스프링은 빈 메소드를 빈을 부르기 위한 명칭으로 사용하지만, 파라미터로<br>
사용되는 빈을 찾을 때는 빈 메소드 명칭이 아닌 클래스 명칭으로 찾는 것으로 추측된다.<br>

### ComponentScan 어노테이션을 이용한 DI
`@ComponentScan`어노테이션은 파라미터로 지정한 경로의 패키지의 클래스들 중에서 `@Component` 혹은 service, repository 등등의 어노테이션이<br>
적용된 클래스들을 빈 객체로 생성시켜주는 어노테이션이다. 그렇다면, config파일에 직접 bean을 설정하는 것은 필요없는 것일까?<br>
그렇지 않다. 라이브러리를 가져와 사용할 때 해당 클래스를 빈으로 등록해야 할 경우가 있다. 이 경우 라이브러리에 들어가 어노테이션을<br>
적용시킬 수 없기에 이러한 경우 직접 config파일에서 빈으로 등록해야 한다.<br>

`@Autowired`어노테이션이 적용된 멤버 변수는 스프링에 의해 자동으로 빈 객체 주입될 것이다. 따라서, setter가 없어도 알아서 주입시켜준다.<br>
Engine.java
```
@Component
public class Engine {
	public Engine() {
		System.out.println("Engine 생성자");
	}
	
	public void exec() {
		System.out.println("엔진이 동작합니다.");
	}

}
```
Car.java
```
@Component
public class Car {
	@Autowired
	private Engine v8;
	
	public Car() {
		System.out.println("Car 생성자");
	}
	
	public void run() {
		System.out.println("엔진을 이용하여 달립니다.");
		v8.exec();
	}
}
```
ApplicationCofig.java
```
@Configuration
@ComponentScan("kr.or.connect.componentscandi")
public class ApplicationConfig {

}
```
ApplicationContext.java
```
public class ApplicationContextExam4 {
	public static void main(String[] args) {
    ApplicationContext ac = new AnnotationConfigApplicationContext(ApplicationConfig.class);
		
		Car car = (Car)ac.getBean(Car.class);
		car.run();
	}
}
```





