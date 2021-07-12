# Spring 3. Spring bean & Dependencies

- 화면을 붙이고 싶은데??
  - View 템플릿이랑 Controller 가 필요하다.
- 회원가입에 관련해서 html 을 뿌려주고 싶다.
  - 그러면 member Controller 가 필요하고, memberController가 memberService 를 통해서 회원가입을 하고, 데이터를 조회할 수 있어야 한다.
    **이것을 memberController 가 memberService 에 의존한다** 라고 표현한다.



## 컴포넌트 스캔과 자동 의존관계 설정



### Controller

- `@Controller`
  - 이 annotation 을  달아주면 spring 이 실행되면서 컨테이너가 생기는데, 거기에서 `controller` 객체가 생긴다. 그리고 spring 이 관리를 한다.
    - 이것을 spring container 에서 spring bean 이 관리된다. 라고 표현한다.



### new 로 생성하는 것에 대해서

- ```java
  @Controller
  public class MemberController {
      private final MemberService memberService = new MemberService();
  }
  ```

  - 이것을 new 로 받는 것이 좋은 것일까??
    - MemberService 는 많은 곳에서 사용하고 있다.
    - 이것을 굳이 다 만들 필요는 없다. 그냥 하나를 공유하는 것이 좋다.
  - 해답
    - 하나를 등록시키는 것으로 공유를 하자.



#### 의존성 주입 (Dependency Injection)

- ```java
  @Controller
  public class MemberController {
  
      private final MemberService memberService;
  
      @Autowired
      public MemberController(MemberService memberService) {
          this.memberService = memberService;
      }
  }
  ```

  - `Add contructor`
  - `@Autowired`
    1. `MemberController` 는 spring container 가 뜰 때, 생성된다.
    2. 이 생성자를 호출한다.
    3. `@Autowired`가 있다면, spring 이 `memberService`  과 연결을 시켜준다.



##### @Autowired

- 순서

  1. `MemberController` 는 spring container 가 뜰 때, 생성된다.
  2. 이 생성자를 호출한다.
  3. `@Autowired`가 있다면, spring 이 `memberService`  과 연결을 시켜준다.

- 주의사항 ( 찾을 수 없음!)

  - error

    ```console
    'hello.hellospring.service.MemberService' that could not be found.
    ```

    - 찾을 수가 없다고 한다.

  - 이유

    - 본래 `@Autowired`가 되면 spring 이 서비스가 되면서 controller 에 넣어준다.
    - 그런데 `memberService`는 순수한 java class 이다.
      spring 이 이것의 정체를 알 수가 없다.

  - 해결방법

    - spring 이 알 수 있도록 annotation 을 달아주면 된다!

    - ```java
      // service / MemberService
      
      import org.springframework.stereotype.Service;
      
      @Service
      public class MemberService {
      ```

- 하나의 정형화된 패턴

  - code

    - ```java
      // service / MemberService
      
      import org.springframework.stereotype.Service;
      
      @Service
      public class MemberService {
      ```

    - ```java
      // service / MemoryMemberRepository
      
      import org.springframework.stereotype.Repository;
      
      @Repository
      public class MemoryMemberRepository implements MemberRepository{
      ```

  - 패턴
    - 각 annotation 과 class 의 이름이 동일한 부분이 있다.
    - controller, service, repository 가 정형화된 패턴이다.



#### 활용

- `MemberController` - `MemberService` - `MemberRepository`

- 과정

  1. ```java
     @Controller
     public class MemberController {
     
         private final MemberService memberService;
     
         @Autowired
         public MemberController(MemberService memberService) {
             this.memberService = memberService;
         }
     }
     ```

  2. ```java
     @Service
     public class MemberService {
     
         private final MemberRepository memberRepository;
     
         @Autowired
         public MemberService(MemberRepository memberRepository) {
             this.memberRepository = memberRepository;
         }
     ```

  3. ```java
     @Repository
     public class MemoryMemberRepository implements MemberRepository{
     ```





#### spring bean 등록하기

1. 컴포넌트 스캔방식

   - `@Service`, `@Repository`...

     - `@Service` 안으로 들어가보면!

       ```java
       @Component
       public @interface Service {
       ```
       - 다른 것도 다 이런 방식으로 되어있다.
       - 이게 달려있으면 다 하나씩 생성을 해서 등록을 시킨다.

   - 어디까지 스캔을 하느냐??

     - `hello.hellospring` 패키지까지만 스캔을 한다.

     - 추가 설정을 한다면 가능하다.

2. 자바 코드로 직접 스프링 빈 등록하기



**참고사항**

- 스프링은 스프링 컨체이너에 스프링 빈을 등록할 때, 기본으로 싱글톤으로 등록한다.
  - 유일하게 하나만 등록한다. 각각 하나만 등록해서 공유한다.
    - 각각 똑같은 인스턴스를 사용한다.
  - 같은 스프링 빈이면 모두 같은 인스턴스이다.
  - 설정으로 싱글톤이 아니도록 설정가능





## 자바 코드로 직접 스프링 빈 등록하기

- 관련한 annotation 을 지워본다.
  - `@Repository`, `@Service` ..
  - `@Controller` 는 어쩔 수 없이 사용해야한다.
- spring bean 으로 등록한 것만 인식을 한다.
  - new 를 통해서 작성한 것을 인식하지는 못한다.
- 장점
  - 몇 가지 구현 클래스를 변경해야할 때, 비교적 쉽게 변경할 수 있다.	
    - 컴포넌트 스캔과는 다르다.



### 활용

1. `hellospring` 패키지 / 파일 생성

2. 코드 작성

   - ```java
     package hello.hellospring;
     
     import hello.hellospring.repository.MemberRepository;
     import hello.hellospring.repository.MemoryMemberRepository;
     import hello.hellospring.service.MemberService;
     import org.springframework.context.annotation.Bean;
     import org.springframework.context.annotation.Configuration;
     
     @Configuration
     public class SpringConfig {
     
         @Bean
         public MemberService memberService() {
             return new MemberService(memberRepository());
         }
     
         @Bean
         public MemberRepository memberRepository() {
             return new MemoryMemberRepository();
         }
     
     }
     ```

     - 순서
       1. 실행되면서 MemberService, MemberRepository 가 등록된다.
       2. spring bean 에 등록되어있는 MemberRepository  를  MemberService에 넣어준다.



## DI 의 방법

- 방법

  1. 필드 주입

     - ```java
       @Autowired private MemberService memberService;
       ```

     - 좋은 방법은 아니다.

     - 중간에서 바꿀 수 있는 여지가 없다.

  2. setter 주입

     - ```java
       private MemberService memberService;
       
       @Autowired
       public void setMemberService(MemberService memberService) {
               this.memberService = memberService;
           }
       ```

     - `public`으로 노출이 될 수 있다.

     - 중간에 바꿔치기될 수도 있다.

       - 잘못 바꾸면 문제가 된다.
       - 어플리케이션 시작할 때 바꿔지는 거지 그다음은 안바뀌는 것이 좋다.
       - 개발은 호출하지 않아야될 메서드가 호출되면 안좋다.

  3. 생성자 주입

     - 이걸 사용하면  한번사용하고 그 다음에는 변경이 되지 않는다.
     - 의존관계는 실행 중에 동적으로 변하는 경우가 없어서 생성자 주입이 권장된다.
       - 바뀌어야 할 때는 아예 수정을 하고 다시 올린다.



#### 
