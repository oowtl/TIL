# Spring 7. Spring Bean



## 스프링 빈 조회하기

- 형식
  - `.getBean(타입)`
  - `.getBean( 이름, 타입)`
  - `.getBean(구체)`
    - spring bean 에 등록된 인스턴스 타입으로 판단으로 한다.
      물론 구체를 넣으면 좋지 않다.
    - ServiceImpl 과 같은 구현 클래스도 들어갈 수는 있다는 뜻

- 특징
  - 조회할 것이 없다면 에러가 난다.

- 에러

  ```java
  import static org.junit.jupiter.api.Assertions.assertThrows;
  import org.springframework.beans.factory.NoSuchBeanDefinitionException;
  
  assertThrows(NoSuchBeanDefinitionException.class,
                  () -> ac.getBean("xxxx", MemberService.class));
  ```

  - 해석
    - 이 코드가 실행이 된다면, 해당하는 에러가 무조건 출력되어야 한다.



## 동일한 타입의 스프링 빈 조회

- 타입으로 조회할 때, 같은 타입이 둘 이상이면 에러가 난다.
- 여러가지 방법으로 들고 올 수는 있다.



## 스프링 빈의 상속관계

- 스프링 빈을 조회할 때, 부모타입을 조회하면 자식타입도 함께 조회한다.

  - **자식 타입은 다 끌려나온다!**

  - Object 타입으로 조회를 하면 모든 스프링 빈을 조회한다.

    - ```java
      Map<String, Object> beansOfType = ac.getBeansOfType(Object.class);
      ```

- 역할과 구현을 파악할 수 있도록 코딩을 하자!

  ```java
  @Bean
  public DiscountPolicy rateDiscountPolicy() {
    return new RateDiscountPolicy();
  }
  
  @Bean
  public DiscountPolicy fixDiscountPolicy() {
    return new FixDiscountPolicy();
  }
  ```

  - 앞의 `DiscountPolicy` 를 바꿔도 되지만, 역할을 명시하는 것으로 이게 무엇인지 파악할 수 있도록 하는 것이 좋다.

- 필요성

  - 기본 기능이지만, 순수한 자바 단에서 이것들을 실행시켜야 하는 경우가 있기도 하다.
  - 보통은 알아서 다 해주는 기능이 있다.



## BeanFactory & ApplicationContext

- 상속구조
  1. BeanFactory
     - 스프링 컨테이너의 최상위 인터페이스
     - 스프링 빈을 관리하고 조회하는 역할
  2. ApplicationContext
     - BeanFactory의 기능을 모두 상속 받아서 제공한다.
     - 둘의 차이는 뭘까요??
       - 이 녀석은 다른 친구들도 상속을 받는다.
         한마디로, 기본적인 것들을 총체적으로 다 받는다는 뜻!
     - 부가 기능 ( 상속받는 다른 것)
       1. MessageSource
          - 각 지역에 따른 언어출력 ( 한국 - 한국어, 영어권 -영어 출력)
       2. EnvironmentCapable
          - 로컬 개발, 운영 데이터베이스, 등의 환경변수 처리
       3. ApplicationEventPUblisher
          - 이벤트를 발행하고 구독하는 모델
       4. ResourceLoader
          - 편리한 리소스 조회 : 파일, 클래스 패스, 외부 URL 같은 것을 편리하게 조회하는 것
  3. **AnnotationConfigApplicationContext**
     - **이거는 설정파일에 따라서 달라진다.**
- 결론
  - BeanFactory 를 사용할 경우는 거의 없다.
    - 부가기능이 달려있는 ApplicationContext를 사용한다!
  - BeanFactory, ApplicationContext 를 스프링 컨테이너라고 한다.





## Java, XML 로 설정해보기

- 설정파일 별로 Bean을 설정하는 것이 다르다.
  - `AnnotationConfigApplicationContext`
    - `AppConfig.class` 로 설정할 때, 사용하는 것
  - `GenericXmlApplicationContext`
    - `appConfig.xml`
  - `XxxApplicationContext`
    - `appConfig.xxx`



### XML 설정하기

- 잘 사용하지는 않지만, 대충 알아두면 좋다.

- 컴파일 없이 설정정보를 변경해도 좋다는 것이 장점!

- Code

  ```xml
  <bean
        id="memberService" class="hello.core.member.MemberServiceImpl">
    <constructor-arg
                     name="memberRepository" ref="memberRepository" />
  </bean>
  
  <bean id="memberRepository" class="hello.core.member.MemoryMemberRepository" />
  
  ```

  - 이런 방식으로 주입을 시킨다는 것을 알아두자



- Error Meeting

  - `assertThat(memberService).isInstanceOf(MemberService.class);`

    - `isEqualTo` 로 하니까 맞을 수가 없다. 

      ```console
      org.opentest4j.AssertionFailedError: 
      expected: hello.core.member.MemberService
      but was : hello.core.member.MemberServiceImpl@5d8bafa9
      ```

      - 구현체에서 나오는 것과 인터페이스는 다른 것이다.
        이럴때 비교하는 것은 인스턴스로 비교해야한다.
        - 찾아봐도 잘 안나와서 대충 판단하기에는 이것의 부모인지를 판단하는 것 같다.
          memberService가 MemberService.class 의  자식인지 판단하는 것!



## 스프링 빈 설정 메타 정보 Bean Definition

- 스프링이 어떻게 다양한 설정정보를 지원할 수 있을까??

  - `BeanDefinition` 이라는 추상화가 존재한다.
    역할과 구현을 개념적으로 나눠서 가능하다.
    스프링은 java, xml 을 가리지 않아... 뭔지도 몰라
    - `BeanDefinition`  이 녀석은 `@Bean`, `<Bean>` 에 대응하는 메타정보를 생성한다.
      스프링 빈은 이 메타정보를 기반으로 스프링 빈을 생성한다.
  - **중요한 것은! BeanDefinition 자체가 추상화에 의존하도록 되어있다는 것!**

- 작동과정

  - `AnnotationConfigApplicationContext(AppConfig.class)`
    1. `AnnotatedBeanDefinitionReader`  이 친구를 통해서 다 읽어들인다.
  - `GenericXmlApplicationContext(appConfig.xml)`
    1. `XmlBeanDefinitionReader` 이 친구를 통해서 읽어들인다.

- 결과

  - `AnnotationConfigApplicationContext`

    ```console
    beanDefinitionName = memberService
    beanDefinition = Root
    bean: class [null];
    scope=;
    abstract=false;
    lazyInit=null;
    autowireMode=3;
    dependencyCheck=0;
    autowireCandidate=true;
    primary=false;
    factoryBeanName=appConfig;
    factoryMethodName=memberService;
    initMethodName=null;
    destroyMethodName=(inferred);
    defined in hello.core.AppConfig
    ```
    - 실제로는 붙어서 나옴.
    - 이러한 메타정보를 이용해서 bean 을 생성한다.

  - `GenericXmlApplicationContext`

    ```java
    beanDefinitionName = memberRepository
    beanDefinition = Generic
    bean: class [hello.core.member.MemoryMemberRepository];
    scope=;
    abstract=false;
    lazyInit=false;
    autowireMode=0;
    dependencyCheck=0;
    autowireCandidate=true;
    primary=false;
    factoryBeanName=null;
    factoryMethodName=null;
    initMethodName=null;
    destroyMethodName=null;
    defined in class path resource [appConfig.xml]
    ```

    - 이건 메타정보가 null 인 대신에 경로가 명확하다.

- 스프링으로 Bean 을 설정하는 방법은 직접 등록하는 방법과 factorymanager로 등록하는 방법이 있다.
  위의 결과를 보면 각각 `factoryBeanName`, `factoryMethodName` 가 다르다. 직접 설정을 하면 경로가 명확하게 나오고, factorymanager로 설정하는 것은 항목이 채워져서 나온다.

