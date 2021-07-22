# Spring 6. spring Container



## 스프링 컨테이너

- 생성하기

  ```java
  ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
  ```

- 설명

  - `ApplicationContext` 는 인터페이스이다.
  - 스프링 컨테이너는 XML으로도 애노테이션으로도 만들 수 있다.
  - 부르는 이름
    둘이 나눠서 부르는데 최근에는 ApplicationContext 를 많이 사용한다.
    - BeanFactory
    - ApplicationContext

- 동작 과정

  - 스프링 컨테이너 생성

  - 스프링 빈 등록

    - method 이름을 빈 이름 (key) 로 등록한다.
    - return new 한 인스턴스 객체를 빈 객체라고 한다.

    - 주의사항
      - 빈의 이름은 항상 다른이름을 부여해야 한다.
      - 문제는 단순하게 해결하자.
        - 애매한 것은 하지말자~~

  - 스프링 빈 의존관계 설정 준비

    - 설명
      - 객체를 생성한 후에 각각에 맞도록 의존관계를 설정해준다.
      - 객체의 참조값들이 연결이 된다.
      - 빈을 생성하고 의존관계를 연결해주는 것으로 나뉘어져 있지만, 실제로는 생성자를 호출하면서 동시에 의존관계가 설정된다.
        실제로 자동으로 의존관계가 설정되는 경우도 있다..!



## 컨테이너에 등록된 빈 조회하기



- 모든 빈 조회

  ```java
  @Test
  @DisplayName("모든 빈 출력하기")
  void findAllBean() {
    String[] beanDefinitionNames = ac.getBeanDefinitionNames();
    for (String beanDefinitionName : beanDefinitionNames) {
      Object bean = ac.getBean(beanDefinitionName);
      System.out.println("name = " + beanDefinitionName + " object = " + bean);
    }
  }
  ```

- 애플리케이션 빈 조회

  ```java
  @Test
  @DisplayName("애플리케이션 빈 출력하기")
  void findApplicationBean() {
    String[] beanDefinitionNames = ac.getBeanDefinitionNames();
    for (String beanDefinitionName : beanDefinitionNames) {
      BeanDefinition beanDefinition = ac.getBeanDefinition(beanDefinitionName);
  
      if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
        Object bean = ac.getBean(beanDefinitionName);
        System.out.println("name = " + beanDefinitionName + " object = " + bean);
      }
    }
  }
  ```

  - 모든 빈 이름 조회하기
    - `String[] beanDefinitionNames = ac.getBeanDefinitionNames();`
  - 특정 이름의 빈 조회
    - `getBean( 빈 이름 );`
  - 빈의 역할에 따른 필터링?
    - `BeanDefinition.ROLE_APPLICATION`
      - 직접 등록한 애플리케이션 빈
    - `BeanDefinition.ROLE_INFRASTRUCTURE`
      - 스프링이 내부에서 사용하는 빈

