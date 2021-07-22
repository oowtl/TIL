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

