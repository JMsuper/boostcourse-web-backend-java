# SpringTest
## JUnit
JUnit이란 Java에서 테스트를 수행하기 위한 프레임워크이다.
- `@BeforeClass` : 테스트 클래스가 실행되기 전에 딱 한번 실행된다.
- `@AfterClass` : 테스트 클래스의 모든 테스트 메소드가 실행이 끝나고 딱 한번 실행된다.
- `@Before` : 테스트 메소드가 실행되기 전 매번 실행된다.
- `@After` : 테스트 메소드가 실행된 이후 매번 실행된다.
- `@Test` : 테스트 메소드임을 지정한다.

#### JUnit 라이프 사이클
```
public class JUnitLifeCycleTest {
	
	@BeforeClass
	public static void BeforeClassFunc() {
		System.out.println("@BeforeClass");
	}
	
	@AfterClass
	public static void AfterClassFunc() {
		System.out.println("@AfterClass");
	}
	
	@Before
	public void BeforeFunc() {
		System.out.println("@Before");
	}
	
	@After
	public void AfterFunc() {
		System.out.println("@After");
	}
	
	@Test
	public void testFunc1() {
		System.out.println("@Test1");
	}
	
	@Test
	public void testFunc2() {
		System.out.println("@Test2");
	}
}
```
실행결과
```
@BeforeClass
@Before
@Test1
@After
@Before
@Test2
@After
@AfterClass
```

## Mockito를 이용한 단위 테스트
```
package kr.or.connect.junit;

import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.runners.MockitoJUnitRunner;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;

import static org.mockito.BDDMockito.given;
import static org.mockito.Matchers.anyInt;
import static org.mockito.Mockito.verify;

@RunWith(MockitoJUnitRunner.class)
public class MyServiceTest {
    // 목 객체를 사용하는 MyService 객체를 생성하여 초기화하라는 의미
	@InjectMocks
    MyService myService;
    
    // calculatorService가 목 객체를 참조하도록 한다.
    @Mock
    CalculatorService calculatorService;

    @Test
    public void execute() throws Exception{
        // given
        int value1 = 5;
        int value2 = 10;
        // 현재 calculatorService는 테스트를 위해 생성된 Mock 객체
        // 즉, 가짜 객체이다. 따라서 given()메소드를 통해
        // calculatorService의 동장 방법을 규정할 수 있다.
        given(calculatorService.plus(5, 10)).willReturn(15);

        // when
        int result = myService.execute(value1, value2);

        // then
        // verify()메소드는 파라미터로 들어온 객체의 plus메소드가 호출된 적이 있는지 검증함.
        // plus(anyInt(),anyInt())는 어떤 정수든지 2개를 파라미터로 넣어서
        // plus()메소드가 호출했는지를 검증한다.
        verify(calculatorService).plus(anyInt(), anyInt());
//        verify(calculatorService).plus(5, 10);
        Assert.assertEquals(30, result);
    }
}
```


