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

