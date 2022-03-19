# Spring 5. Go To Spring



## Configuration, 설정정보



### `@Configuration`

- ```java
  import org.springframework.context.annotation.Configuration;
  
  @Configuration
  public class AppConfig {}
  ```

  - 이것이 붙어야 설정으로 사용한다.

### `@Bean`

- ```java
  import org.springframework.context.annotation.Bean;
  
  @Bean
  public MemberService memberService() {
      return new MemberServiceImpl(MemberRepository());
  }
  ```

  - 등록된 모든 메소드를 호출해서 반환된 객체를 스프링 컨테이너에 등록한다.
  - 등록된 이름을 그대로 스프링컨테이너에서 사용한다.
    - 그냥 그대로 사용하는 것이 좋을 것...



### AppConfig 추가해주기

- ```java
  import org.springframework.context.ApplicationContext;
  import org.springframework.context.annotation.AnnotationConfigApplicationContext;
  
  ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
  
  MemberService memberService = applicationContext.getBean("memberService", MemberService.class);
  ```

  - `ApplicationContext`
    - 스프링 컨테이너
  - `.getBean( { 꺼낼 Bean의 이름 } , { 꺼낼 Bean의 타입 } )`
    - 이거 통해서 빈을 가져온다.

- 키와 밸류로 해서 컨테이너에 들어간다.



## Why Spring Container?

1. 스프링 컨테이너가 해주는 기능이 정말 많다~
   이제 찾아보자...!